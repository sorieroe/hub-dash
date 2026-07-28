# Task Master

Single source of truth for build state. Any session picks the top unblocked task in the active epic, works it, updates state here, commits. States: `todo · in-progress · blocked · review · done`. Gates (G0–G4) are founder reviews — see docs/14.

## E0 — Framework definition  `[review — G0]`
- [x] Vision, game design, simulation model (docs 01–03)
- [x] Market/platform/tech research (docs 04–06)
- [x] Meta-architecture, data model, component spec, agent factory (docs 09–11, 14)
- [x] Content pipeline doc from ground-truth research (doc 12)
- [x] Game-feel/progression doc from research (doc 13)
- [ ] **G0: founder reviews the full framework** — `blocked on founder`

## E1 — Pre-production design  `[blocked by G0]`
- [ ] Art bible v1 (style family, palette, motion rules, do/don't board)
- [ ] First-pass concept mockups for founder taste-check (SVG/HTML style frames)
- [ ] Character sheets: first 5 components (client swarm, LB, app server, cache, sql-db)
- [ ] UI screens ×7 (level select, brief, build canvas, running sim, failure moment, debrief, share card) — mobile + desktop
- [ ] Session storyboard: full first-session "Build Shortly" walkthrough
- [ ] **G1: founder approves the look** — nothing playable is built before this

## E2 — New repo & scaffolding  `[blocked by G0]`
- [ ] Founder creates the repo (name TBD — docs/08 #1); migrate this folder; archive branch in hub-dash
- [ ] Vite + TS workspace layout (`ui / render / sim / levels / shared`), CI (lint, typecheck, fixtures, golden replays)
- [ ] Cloudflare Pages preview deploys per PR

## E3 — Sim engine core  `[blocked by G1]`
- [ ] Fixed-tick loop, seeded RNG streams, integer sim time
- [ ] Queueing-station base component + knobs framework
- [ ] Incident/event bus → debrief data; metrics ring buffers (utilization, p50/95/99)
- [ ] Replay format + record/replay; determinism CI on two JS engines
- [ ] Headless runner (Node) + fixture harness

## E4 — Canvas & juice  `[blocked by G1]`
- [ ] React Flow editor: palette, connection grammar enforcement, knob panels, touch support
- [ ] Pixi overlay synced to viewport: request particles, queue pile-ups, five load states per art bible
- [ ] Failure spectacle pass (the make-or-break moment — doc 06 risk #1)
- [ ] Sound layer (per doc 13; respects browser autoplay rules)

## E5 — Component pack v1 (fan-out, one worktree each)  `[blocked by E3 start]`
- [ ] client-swarm · [ ] load-balancer · [ ] app-server · [ ] cache · [ ] db-sql
- [ ] db-kv · [ ] object-storage · [ ] message-queue · [ ] worker-pool · [ ] cdn
- [ ] rate-limiter · [ ] monitoring · [ ] backup-snapshot
- Each: doc 11 definition-of-done (physics + fixtures + character sheet + provenance + codex)

## E6 — Three PoC levels end-to-end  `[G2 → G3]`
- [ ] Level 1: URL shortener launch day (schema exists in `levels/`)
- [ ] Level 2 & 3: picked from doc 12's canonical easy list (candidates: pastebin, image host)
- [ ] Debrief screens with concept mapping; win/lose/star flow
- [ ] **G2: founder plays the vertical slice**; then judge-panel convergence per level
- [ ] **G3: two consecutive clean judge panels across all three levels**

## E7 — Judge-loop harness  `[parallel with E3–E6]`
- [ ] Playwright playtest agent runbook (drive Chromium, screenshot every state)
- [ ] Physics judge (headless replay vs on-screen numbers), art judge (screenshot vs character sheets), pedagogy judge (vs provenance), adversarial judge
- [ ] Convergence tracker: findings → tasks here; two-clean-panels rule

## E8 — Accounts & progression  `[blocked by G2]`
- [ ] Supabase schema per doc 10; local-first IndexedDB mirror + idempotent sync
- [ ] Anonymous play + claim flow; XP ledger + streaks + codex unlocks
- [ ] Daily challenge + share card generation
- [ ] PostHog funnels (attempted → failed → retried → passed)

## E9 — Money  `[blocked by G3]`
- [ ] Lemon Squeezy checkout + webhook worker + entitlements (doc 09 §4)
- [ ] Free/Pro gating per doc 05; "expense to your company" generator
- [ ] Replay-verification worker for leaderboards

## E10 — Launch  `[blocked by G4]`
- [ ] Landing page + SEO scenario pages; meltdown-GIF capture pipeline
- [ ] Show HN / dev.to / r/ExperiencedDevs launch plan
- [ ] **G4: founder go/no-go**

## Parked (deliberate)
Steam build · mobile Capacitor wrapper · AI interviewer mode · community level editor · teams tier — see docs/07 phases.
