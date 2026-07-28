# Game Design Draft

## Core loop (one level, 5–15 minutes)

1. **Brief.** A scenario card: "You're launching PhotoShare. Expected: 5k signups on day one, each uploading ~20 photos. Investors gave you $500/mo of infra budget. Keep p99 latency under 800ms and lose zero uploads."
2. **Build.** Drag components from a palette onto the canvas, wire them together, set knobs (instance count, cache TTL, replica count). The palette is *constrained per level* — early levels offer 4 components, later levels offer 25.
3. **Run.** Hit play. The traffic profile executes: request particles flow along your edges, gauges fill, queues grow. Time is compressed (a "day" in 90 seconds) and you can pause/rewind.
4. **Break.** Something fails — visibly and legibly. Data particles fall off the canvas and shatter when there's nowhere to store them. An overloaded server glows red, then drops requests. A dead single database takes everything with it.
5. **Diagnose.** The incident report tells you *what* happened, not what to do: "14:02 — DB write queue exceeded capacity. 4,312 uploads lost." A hint system (optional, costs score) nudges toward *why*.
6. **Iterate → pass.** Fix, rerun, pass the SLOs. Then the twist hits (see below) and the level continues, or you bank your stars.

## The twist mechanic (what makes it a *game*)

Real interviews escalate requirements mid-answer; so do levels. Each level has 2–4 **phases** that mutate the world while your system runs:

- *Growth*: traffic 10×'s over a compressed week.
- *Viral spike*: a celebrity posts your app — 50× burst for 3 minutes.
- *Chaos event*: a disk dies, an availability zone goes dark, a deploy goes bad. (Your single-node DB "at your house" literally catches fire — this is the redundancy lesson.)
- *Requirement change*: "Legal says EU user data must stay in the EU." "Product wants read-your-own-writes."
- *Attack*: credential stuffing, a scraper, a small DDoS — the security lesson.

Surviving phase 1 with an over-engineered design *hurts* you in scoring (cost efficiency), which teaches the real lesson: right-size for stated requirements, evolve under pressure. That's precisely what interviewers grade.

## Scoring (the interviewer's rubric, gamified)

Four gauges, combined into 1–3 stars plus a percentile:

| Gauge | Measures | Interview analog |
|---|---|---|
| **Reliability** | SLO compliance: uptime, p99 latency, error rate | "Does it work at scale?" |
| **Durability** | Data lost / data received | "Where does state live? What survives a crash?" |
| **Efficiency** | Infra spend vs. budget; utilization | "Did you over/under-provision? Do you know why each box exists?" |
| **Resilience** | Survived chaos events; SPOF count at end of level | "What breaks first? What's your blast radius?" |

Post-level: a **debrief screen** maps what happened to the named concept ("You just experienced a *cache stampede*. In an interview, mention request coalescing.") — the explicit bridge from game to interview vocabulary.

## Modes

1. **Campaign** — the spine of the product. Chapters ordered by concept, not company: Storage → Scaling reads → Scaling writes → Async & queues → Resilience → Geo-distribution → Security. Each chapter's boss level is a classic interview question (URL shortener → pastebin → Instagram → Dropbox → WhatsApp → Uber → "Design Facebook" as the finale).
2. **Daily challenge** — one fixed scenario + seed for everyone, one attempt, shareable result card (Wordle mechanics). Primary growth loop.
3. **Firefight** — you inherit a *broken* running system and must stabilize it live. Teaches diagnosis; closest to on-call reality; great for shorts/clips.
4. **Sandbox** — free build, arbitrary traffic dials. Cheap to ship, good for content creators.
5. **Interview mode (v2, AI-powered)** — an AI interviewer asks *why* ("Walk me through what happens when a write arrives"), mutates requirements conversationally, and grades your explanation, not just your diagram. This is the feature that fully closes the gap to a real interview, and a strong paid-tier anchor.

## Progression & retention

- **Component unlocks**: new palette items arrive with the chapter that teaches them. (You *earn* the message queue.)
- **Stars gate boss levels**; percentiles + leaderboards per level (seeded runs make them fair).
- **Streaks** on the daily challenge; spaced-repetition "remix" levels that re-test old concepts under new skins.
- **Codex**: every component and failure mode gets an encyclopedia entry as you encounter it — this becomes the study-reference product surface (and an SEO surface on web).

## Tone & aesthetic (draft)

Clean, diagrammatic, slightly toy-like — closer to Mini Metro / Two Point Hospital than to a gritty terminal. Failures should be *comedic*, not punishing: data shattering like glass, a tiny fire on the home-server. The vibe target: "the interviewer is going to love this story."

## Explicitly deferred

Realtime multiplayer, community level editor (v2 — levels-as-data makes it feasible), mobile-native gesture polish beyond "works well on a tablet/phone in landscape," cosmetics economy.
