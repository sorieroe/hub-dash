# System Design Simulator (working title)

**A simulation game that teaches system design by letting you feel systems break.** Get a prompt ("Build Dropbox"), drag infrastructure onto a canvas, and watch simulated users hammer it: no database and the uploads visibly evaporate; one hot server and the queue backs up until latency explodes. Iterate until the design survives, get scored like an interviewer would score you.

> Drafted 2026-07-28 inside `hub-dash` because this session couldn't create a new GitHub repository. **This folder is self-contained** — lift it wholesale into a fresh repo (`git filter-repo` or plain copy) and delete it here.

## State of the draft

Concept, deep research (five parallel research passes: competitors, monetization/platforms, tech stack, ground-truth content sources, game-feel/progression), and the full build framework are complete (2026-07). **Currently at gate G0: founder review of this framework.** After G0 comes pre-production design (concept art + UI mocks for review at G1) — no gameplay code before G1 approval. Build state lives in [TASKS.md](TASKS.md).

## TL;DR of the research

- **The gap is real but specific.** 8–10 free indie prototypes of "drag-drop architecture + sim" shipped in the last ~18 months (closest: SysSimulator, LeetDesign) — all tools, none games, none with traction or revenue. The paid winners (Hello Interview $99/yr, ByteByteGo, Educative) have zero simulation. Nobody owns *sim + game design + curriculum + distribution*. Incumbents are drifting this way (NeetCode is rebuilding its system design section as "interactive") — speed matters.
- **Web-first, decisively.** Every successful comp is web-first; the share/fork-URL growth loop needs the web; store cuts and canvas-on-phone argue the same. Steam is a later $15–20 one-time marketing beat (genre best case: while True: learn(), ~$3M gross *lifetime*); mobile is a year-2 companion app (review/streaks), not the core game.
- **Pricing has proven anchors.** Free tier (sandbox + 3–5 scenarios + daily challenge) → Pro $12/mo / ~$99/yr → Lifetime ~$199–249 → Teams w/ "expense to your company" flow. Plan on 2–4% conversion. Revenue scenarios: ~$35K (conservative) / ~$325K (moderate) / ~$1.4M ARR (optimistic); Boot.dev ($10M ARR, 13 people, gamified dev ed) is the demonstrated ceiling.
- **Stack:** TypeScript · React + Vite + React Flow (editor) · PixiJS overlay (request particles — the SVG-animation trap is documented) · custom fixed-tick sim engine in a Web Worker (seeded, deterministic, queueing-theory-based, headless-testable) · Supabase + Cloudflare Pages (~$0 until real scale) · Lemon Squeezy (merchant of record) · Claude API for the v2 AI interviewer (~$0.003–0.015/call, paid-gated). Avoided by construction: tldraw ($6K/yr license), Unity/Godot web exports (12–35 MB), `Math.random()` in sim logic.

## Documents

| Doc | Contents |
|---|---|
| [01-vision.md](docs/01-vision.md) | The pitch, audience, why this can win, what it is not |
| [02-game-design.md](docs/02-game-design.md) | Core loop, twist mechanic, scoring rubric, five modes, progression |
| [03-simulation-model.md](docs/03-simulation-model.md) | The "physics": universal component laws, 17-component catalog with failure modes, traffic profiles, chaos events |
| [04-market-research.md](docs/04-market-research.md) | Competitor teardown, adjacent-game economics, demand signals, strategic conclusions |
| [05-platforms-and-monetization.md](docs/05-platforms-and-monetization.md) | Platform decision, verified pricing anchors, draft tiers, growth playbook, revenue scenarios |
| [06-tech-stack.md](docs/06-tech-stack.md) | Recommended stack, ruled-out traps, architecture sketch, 2-week walking skeleton, risks |
| [07-roadmap.md](docs/07-roadmap.md) | Phases 0–4 with go/no-go gates |
| [08-open-questions.md](docs/08-open-questions.md) | Decisions still owed — name, competitive teardown, AI-interviewer timing, … |
| [09-build-framework.md](docs/09-build-framework.md) | Our own system design: client-side sim scaling insight, payments, failure modes, cost |
| [10-data-model.md](docs/10-data-model.md) | Schema: XP ledger, concept mastery, replays, leaderboards, entitlements, anonymous-claim flow |
| [11-component-spec.md](docs/11-component-spec.md) | The 8-section template every palette component must fill (physics + character sheet + provenance + fixtures) |
| [12-content-pipeline.md](docs/12-content-pipeline.md) | Ground-truth sources with verified licenses, canonical 35-question backlog, component frequency, legal posture |
| [13-game-feel-and-progression.md](docs/13-game-feel-and-progression.md) | "Living toy infrastructure" art direction, juice priorities, sound model, evidence-based progression |
| [14-agent-factory.md](docs/14-agent-factory.md) | Build process: gates G0–G4, pre-production-first, fan-out worktrees, five-judge convergence loops |
| [TASKS.md](TASKS.md) | The task master — epics E0–E10 with live states |
| [levels/example-url-shortener.json](levels/example-url-shortener.json) | Proof that levels are data, not code |
