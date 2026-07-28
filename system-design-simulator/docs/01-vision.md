# Vision

## One-liner

**A simulation game that teaches system design by letting you feel systems break.** You get a prompt — "Build Dropbox" — you drag infrastructure onto a canvas, and simulated users hammer whatever you built. No database? Watch their uploads evaporate. One overloaded server? Watch the queue back up, latency spike, and users rage-quit. You iterate until the system survives, then you get scored like an interviewer would score you.

Working title: **Loadbound** (alternates: Uptime, Bottleneck, SLO: The Game, SysDes.io — naming is an open question, see doc 08).

## The problem

System design is one of the highest-leverage interview skills in tech — senior+ offers hinge on it — and it has the *worst* study tooling of any interview subject:

- **Coding rounds have LeetCode**: instant feedback, graded difficulty, streaks, a clear "am I ready?" signal.
- **System design has… reading.** Books (Alex Xu), courses (Grokking, ByteByteGo), YouTube walkthroughs. All passive. You watch someone else design Dropbox and nod along. There is no feedback loop that tells you *your* design is wrong, and no rep-building mechanic.
- The skill itself is causal reasoning — "if traffic does X and I'm missing Y, then Z fails" — which is exactly what a simulation can teach and a video cannot.

## The insight

Every system design answer is really a set of claims about failure: *this component exists because without it, this specific thing goes wrong under this specific load.* That's enumerable. If we enumerate the components, their functions, their capacity limits, and their failure modes — and wire them into a lightweight physics engine for traffic — then "studying system design" becomes *playing* system design. The learning is embodied: you don't memorize "caches reduce database load," you watch your database melt, add a cache, and watch the graph flatten.

## Who it's for

1. **Primary: interview preppers.** Engineers with an interview in 2–8 weeks, already paying $35–160/yr for prep tools, high willingness to pay, urgent motivation. This is the beachhead and the revenue engine.
2. **Secondary: learners.** CS students and junior engineers who want to genuinely understand distributed systems. Longer retention, lower urgency.
3. **Tertiary (later): teams.** Onboarding/training content for companies and bootcamps — B2B licensing once the content library exists.

## Why this can win

- **The feedback loop is the moat.** Everyone else sells explanations; we sell *consequences*. A design that survives the simulation is a design you can defend out loud in a real interview.
- **It's inherently shareable.** "My Dropbox design survived 1M concurrent users at $4.2k/mo infra cost — can you beat it?" is a screenshot people post. Passive courses have no equivalent.
- **It's fun on a commute.** Levels are 5–15 minute sessions. Streaks, stars, daily challenges — the Duolingo loop applied to a subject people are *already* forced to study.
- **Content compounds.** Every classic interview question (URL shortener, chat app, news feed, ride sharing…) is a level. Levels are data, not code — the engine is built once, the catalog grows forever, and community-made levels are a plausible v2.

## What this is not

- Not another course. Explanations exist in-game, but the product is the simulator.
- Not an infra-accurate emulator. The physics are *pedagogically* honest (queueing, saturation, SPOFs, durability), not packet-accurate. Fun and clarity beat fidelity every time they conflict.
- Not multiplayer-first. Async competition (leaderboards, shared challenges) yes; realtime multiplayer is a v3 idea at best.

## Success criteria for the draft phase

1. A playable vertical slice where one level ("URL shortener, launch day") is genuinely fun and teaches at least three real concepts.
2. Evidence (playtests, waitlist signups) that interview preppers would pay for a catalog of these.
3. A content pipeline where adding level #2 takes days, not weeks.
