# Market Research: Competitors & Demand (researched draft)

Researched 2026-07. Prices verified on cited pages; Steam figures are Boxleiter-method estimates. Full source list in the research reports.

## Headline finding

**The mechanic is not novel; the *game* is.** At least 8–10 independent developers shipped some version of "drag-and-drop architecture + simulation" in the last ~18 months. Nearly all are free, solo-built, unmonetized, and got almost zero distribution (launch posts with 0–2 comments). Meanwhile the products actually making money in system design prep (Hello Interview, ByteByteGo, Educative) have **no live simulation at all**. Nobody has combined a credible sim + real game design (progression, scoring, spectacle) + interview curriculum + a business. That triple combination is the open lane — and the crowd of abandoned prototypes is evidence the pain is real *and* that a bare tool with no game loop and no distribution goes nowhere.

## Closest overlaps (take these seriously)

| Product | Has | Missing | Status |
|---|---|---|---|
| **SysSimulator** (syssimulator.com) | Drag-drop (18 components), genuinely sophisticated WASM discrete-event sim, 28 chaos scenarios, p50/p95/p99, AWS cost estimates, 56 blueprints + interview curriculum | No progression, no scoring loop, no game, no business model, no visible traction | Free, solo-built, no login |
| **LeetDesign** (leetdesign.com) | The "LeetCode of system design" framing: 22 graded problems by difficulty, traffic sim naming your bottleneck, invariant checks (SPOF, caching), re-grades on edit | Small catalog, unclear monetization, unknown traction | Free-looking |
| **InterviewReady Design Judge** (Gaurav Sen) | 60+ problems, AI mock interviewer, "gamified judge," claims 26K+ engineers, real paying audience | Judge/rubric-based grading — no evidence of live traffic simulation | Commercial; moved lifetime→yearly late 2025 (3rd-party, unverified) |
| **Hello Interview** | Best-in-class guided practice: AI reads your whiteboard, rubric feedback from ex-FAANG interviewers, 34 problems; $59/1mo, $99/yr, $349 lifetime | **No simulation** — nothing runs | The demonstrably-charging market leader in active prep |
| **paperdraw.dev / SystemForge / SDApp / Desyra / systemdesignsimulator.in** | Various prototype-grade drag-drop + sim or AI-feedback combos | Polish, content, game loop, distribution | Free prototypes, ~zero traction |
| **Request Rush** (itch.io) | The only actual *game* (tower defense of request waves, explicitly interview-inspired) | Curriculum, depth (one dominant build), polish | Free prototype |
| **Joy of System Design** (InterviewReady OSS) | Drag-drop puzzle version | Simulation; repo itself admits it "didn't gain significant traction" — the puzzle-only version is too thin | Open source, 184 stars |

Also watch: **NeetCode's site says its system design section is deprecated and being rebuilt with "interactive practice problems"** — an incumbent with 770K YouTube subscribers moving toward this space. The realistic competitive threat isn't the prototypes; it's Hello Interview or NeetCode bolting a simulator onto an existing paid funnel. Speed and game-feel are the counters.

## Incumbent (non-interactive) prep market

ByteByteGo (~$50–60/yr, $499 lifetime; newsletter 0→1M subs in ~27 months), Educative Grokking ($149–249/yr), DesignGurus (~$198 one-time, $499 lifetime), Exponent (~$150/yr), interviewing.io ($179–339/mock). All passive or human-powered. Their pricing is our pricing cover; their passivity is our pitch.

## Adjacent games — what the genre supports

- **while True: learn()** — the genre's best case: ~253K copies, ~$3.3M gross lifetime at $12.99. Its top review complaint is instructive: the ML theme didn't match the pipe-plumbing mechanic. *Our advantage: the mechanic IS the subject.*
- **Zachtronics** (TIS-100 ~$813K, Shenzhen I/O ~$1.9M): hardcore engineer-audience games reliably do 100–250K copies. TIS-100 outsold its more accessible sibling — don't over-dumb it down.
- **Mini Metro** (~1.4M copies) — the best mechanical analogy: minimalist network-capacity management where demand grows until your topology fails. **Factorio** (3.5M+ copies): throughput/bottleneck thinking is a massive market when the loop is elegant.
- **SadServers** — "Wordle for SREs" hit #2 on HN (597 points): huge organic appetite for hands-on infra practice. **iximiuz Labs**: ~$185K revenue in 2025 as a near-solo hands-on infra-learning business.
- **Wilco** (cautionary): $7M-seed "flight simulator for developers," wound down into Lemonade. VC-scale gamified dev-upskilling struggled; indie-scale economics (Boot.dev, iximiuz) worked.

## Demand signals

- ByteByteGo's 1M-subscriber newsletter bounds the reachable audience at high-six to seven figures.
- Ask HN / Blind threads explicitly complain prep is passive, shallow, and rehashes "the same tired examples"; one Blind user described wanting almost exactly this product ("select requirements and watch how the design changes").
- The prototype cluster itself is a demand signal: 8–10 builders independently felt this pain in 18 months.
- Willingness to pay in the niche is proven at $99–499.

## Strategic conclusions

1. **Differentiate on game, not mechanic**: spectacle of failure, campaign progression, daily challenge, leaderboards, juice. The free tools have none of it; that's the whole wedge.
2. **Distribution is the real moat** — every prototype died of obscurity, not of being wrong. The share-card loop, SEO scenario pages, and meltdown-video content are as core as the engine.
3. **Move fast**: the mechanic is being rediscovered monthly, and incumbents are inching toward interactivity.
4. **Study SysSimulator and LeetDesign firsthand** before building — cheap competitive intel on what a sim feels like without game design (action item in doc 08).
