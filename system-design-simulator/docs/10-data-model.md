# Data Model

What the user owns, how progress/XP/rank is tracked, and how it stays trustworthy. Postgres (Supabase) with a local-first IndexedDB mirror (doc 09). Naming is draft; shapes are the point.

## Design principles

1. **Append-only where it matters.** XP is an event ledger, not a mutable counter — auditable, sync-safe, cheat-anomaly-detectable, and "how did I earn this?" is always answerable. Displayed totals are materialized.
2. **Attempts are facts; progress is a view.** Every run is an immutable `attempts` row; "best score / stars / mastery" are derived. Nothing user-facing is ever the only copy of truth.
3. **Everything version-pinned.** Attempts/replays store `(level_content_hash, sim_version)` — balance patches never corrupt history; leaderboards partition by sim version (doc 06 risk #4).
4. **Idempotent sync.** All client-originated rows carry client-generated UUIDs; sync is upsert-on-conflict-do-nothing. Offline queues drain safely, retries are harmless.

## Core tables

```sql
-- Identity ---------------------------------------------------------------
profiles (
  user_id      uuid PK REFERENCES auth.users,
  handle       citext UNIQUE,           -- public leaderboard name
  avatar       text,                    -- cosmetic id, earned not bought
  created_at   timestamptz,
  settings     jsonb                    -- sound, colorblind palette, reduced-motion
)

-- Content catalog (rows written by content pipeline, read-only to clients)
levels (
  level_id     text PK,                 -- 'ch1-url-shortener'
  content_hash text NOT NULL,           -- current published hash
  chapter      text, difficulty text,   -- easy | medium | hard
  status       text,                    -- draft | live | retired
  provenance   jsonb                    -- sources per doc 12
)

-- Play facts --------------------------------------------------------------
attempts (
  attempt_id   uuid PK,                 -- client-generated
  user_id      uuid NULL,               -- NULL = anonymous (claimable later)
  level_id     text, level_hash text, sim_version text,
  started_at   timestamptz, ended_at timestamptz,
  outcome      text,                    -- passed | failed | abandoned
  stars        int,                     -- 0–3
  gauges       jsonb,                   -- {reliability, durability, efficiency, resilience}
  duration_ticks bigint
)

replays (
  attempt_id   uuid PK REFERENCES attempts,
  seed         bigint,
  input_log    bytea,                   -- compressed tick-stamped edit events, KBs
  verify_state text DEFAULT 'unverified',  -- unverified | verified | mismatch
  verified_at  timestamptz
)

-- Progression -------------------------------------------------------------
xp_ledger (
  event_id     uuid PK,                 -- client-generated
  user_id      uuid,
  occurred_at  timestamptz,
  amount       int,                     -- always positive; no XP loss mechanics
  reason       text,                    -- level_first_pass | star_upgrade | daily_challenge
                                        -- | codex_read | streak_bonus | concept_mastery
  ref          jsonb                    -- {attempt_id} / {level_id} / {concept_id}
)

user_progress (                          -- materialized from attempts, per (user, level)
  user_id uuid, level_id text,
  best_stars int, best_gauges jsonb, attempt_count int,
  first_passed_at timestamptz, last_played_at timestamptz,
  PRIMARY KEY (user_id, level_id)
)

concept_mastery (                        -- the study-value spine: per concept, not per level
  user_id uuid, concept_id text,        -- 'cache-stampede', 'replication-lag', ...
  exposures int, demonstrations int,    -- saw it happen vs. designed past it
  mastery numeric,                      -- decays gently -> spaced-repetition "remix" prompts
  PRIMARY KEY (user_id, concept_id)
)

streaks (
  user_id uuid PK,
  current int, longest int,
  last_active_date date, tz text        -- streak day computed in the PLAYER's timezone
)

-- Competition -------------------------------------------------------------
daily_challenges ( challenge_date date PK, level_id text, seed bigint )

daily_runs (
  user_id uuid, challenge_date date,
  attempt_id uuid, score numeric, rank int,          -- one attempt/day; share card source
  PRIMARY KEY (user_id, challenge_date)
)

leaderboard_entries (
  level_id text, sim_version text, user_id uuid,
  score numeric, attempt_id uuid,
  status text,                          -- provisional | verified (only verified ranks globally)
  PRIMARY KEY (level_id, sim_version, user_id)
)

achievements ( achievement_id text PK, name text, criteria jsonb, icon text )
user_achievements ( user_id uuid, achievement_id text, earned_at timestamptz,
  PRIMARY KEY (user_id, achievement_id) )

codex_unlocks ( user_id uuid, entry_id text, unlocked_at timestamptz,
  PRIMARY KEY (user_id, entry_id) )     -- encyclopedia fills in as failures are experienced

-- Money -------------------------------------------------------------------
entitlements (
  user_id uuid, product text,           -- pro | lifetime | team_seat
  source text,                          -- lemonsqueezy | apple | steam | grant
  status text,                          -- active | expired | refunded
  expires_at timestamptz NULL,          -- NULL = perpetual
  PRIMARY KEY (user_id, product, source)
)
billing_events (                         -- raw webhook archive, append-only, audit trail
  event_id text PK, source text, received_at timestamptz,
  payload jsonb, processed boolean
)
-- teams / team_members: deferred to the Teams phase (doc 07)
```

## Derived, not stored: rank

Player rank/level (the "you are Silver-III"-style badge) is a **pure function** of the ledger + verified progress — e.g. `f(total_xp, verified_stars, concept_mastery_breadth)`. Never a column; recomputed in a view. Changing the rank formula later is a migration of *nothing*. (Exact rank/league design lands in doc 13 with the progression research.)

## Anonymous → account claim flow

Anonymous play writes attempts/xp locally with a device-scoped anonymous id. On signup: one RPC stamps `user_id` onto the uploaded anonymous rows (the UUIDs already exist — no renumbering), streak recomputes, done. Nobody ever loses pre-signup progress — the #1 churn trap in funnels with a signup wall.

## Anti-cheat posture (proportionate, not paranoid)

- XP: client-trusted, server anomaly-checked (rate caps per reason code; a ledger makes weirdness legible).
- Leaderboards/daily challenge: **replay-verified only** (doc 09). Deterministic replays make forgery harder than just being good at the game.
- Entitlements: server-only writes, webhook-driven.

## The local mirror (IndexedDB)

Same shapes, plus a `pending_sync` outbox. The sim itself needs *none* of this to run — a player with no account and no network still gets the full game loop; the data model is for identity, memory, competition, and money, not for play.
