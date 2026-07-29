# Raw Research Report: Competitors & Demand (2026-07-28)

Verbatim output of the competitive-landscape research agent. Synthesized into `docs/04-market-research.md`; kept raw here for the full tables and source URLs (the content pipeline's pedagogy judges will need them).

---

## 1. Direct competitors

### Tier 1 — Products that already combine drag-and-drop building + live simulation (closest overlaps)

A notable finding up front: **a cluster of at least 8 indie tools launched roughly 2024–2026 that attempt some version of this exact idea.** Most are free, solo-built, and appear to have little commercial traction — but the core mechanic is no longer novel.

| Product | What it is | Drag-and-drop | Live simulation | Interview curriculum | Pricing |
|---|---|---|---|---|---|
| **SysSimulator** (syssimulator.com) | Browser system design simulator | Yes, 18 component types, infinite canvas | Yes — Rust/WASM discrete-event sim, RPS config, P50/P95/P99 latency, 28 chaos scenarios (node crash, partition, memory pressure), live AWS cost estimates | Yes — 56 blueprints, 29 guides, explicit "interview preparation curriculum" | Free, no login |
| **LeetDesign** (leetdesign.com) | "Practice system design with an objective grader" | Draw-your-design canvas (exact UI mechanics unverified) | Yes — traffic simulation with hot spots, queue depth, p99 per flow; invariant checks (caching, replication, SPOF); named bottlenecks with utilization; re-grades on every edit | Yes — 22 "interview-complete" problems, Easy/Medium/Hard, community solutions | Pricing not visible on page (appears free); a separate leetdesign.pro also exists |
| **paperdraw.dev** (by Pratap Vhatkar) | "Whiteboard that runs" | Yes — LB, API gateway, caches, DBs, queues, "AI bits" | Yes — real-time latency/error/throughput/cache-hit; chaos: traffic spikes, cache failure, partitions, crashes; validation hints for missing components | Weak — tool-first, not problem-set-first | Free. Dev.to launch post (Mar 2026) got only 2 comments; also self-promoted on Blind |
| **SystemForge** (github.com/vijaygupta18/system-design-simulator) | Open-source "Interactive System Design Interview Simulator" | Yes — drag infra components, wire them | Yes — "production-scale traffic," scored across 5 interviewer dimensions; fully client-side | Interview-framed ("ace your HLD interview") | Free/open source (star count not captured) |
| **SDApp** (FIA Studios portfolio piece) | Visual system design simulator | Yes — LBs, caches, DBs on canvas | Yes — custom discrete-event engine modeling request flows, latency, component failures | Not evident | Appears to be a portfolio/demo project |
| **systemdesignsimulator.in** | "Visual Distributed Systems Playground" — drag-drop with latency/retries/failures and Monte Carlo statistics (per search snippet) | Yes | Yes (per snippet) | Unclear | Page fetch returned only a title; details unverified |
| **Request Rush** (piggyinabag.itch.io) | Cyberpunk **tower-defense game** explicitly inspired by system design interviews (Godot, HTML5) | Yes — buy/connect Gateways, Processors, Queues | Yes — waves of "Requests" as the attack traffic | No curriculum; pure game | Free prototype; single commenter noted one dominant optimal build (shallow strategy depth) |
| **Joy of System Design** (InterviewReady, GitHub) | Open-source drag-drop puzzle: pick YouTube/Instagram/WhatsApp/etc., fill ~half-empty diagram slots with correct components, get humorous scoring + explanations | Yes (into fixed slots) | **No** — pure matching puzzle, no traffic sim | Yes (interview-flavored) | Free; 184 stars / 23 forks; repo itself says the OSS version "didn't gain significant traction," which is why InterviewReady built a commercial "System Design Online Judge" |

**InterviewReady System Design Judge** (interviewready.io, Gaurav Sen) deserves its own note: marketed as "a platform like LeetCode where you can practice System Design problems online," 60+ questions, plus an "AI-powered Mock Interviewer and Gamified Design Judge." Platform claims 26,000+ engineers. It evaluates submitted designs; I found no evidence of a *live continuous traffic simulation* (it's judge/grader-style). Pricing: the site blocked fetching (redirect loop); third-party sources say it moved from lifetime to yearly access in late 2025, exact price unverified.

### Tier 2 — Interactive practice with canvas + AI grading, but no physics-style simulation

- **Hello Interview** (hellointerview.com): The market leader in "active" system design prep. Whiteboard-first guided practice — "diagram as you explain, the AI reads your drawing" — with step-by-step FAANG-loop prompts and rubric-based per-step AI feedback tuned by ex-FAANG interviewers. 34 system design problems (+LLD, coding, behavioral), ~35–45 min each. Pricing: $59/1mo (non-renewing), $99/yr, $349 lifetime. **No traffic/failure simulation** — the AI critiques the drawing; nothing "runs."
- **Codemia** (codemia.io): "Master system design through active practice." 120+ system design problems (plus 300+ DSA, 70+ OOD, 18 agentic-AI), interactive whiteboard, AI-powered evaluation/design review, peer mock interviews with collaborative whiteboard. Freemium; premium pricing not disclosed on landing page. No live simulation.
- **System Design Sandbox** (systemdesignsandbox.com): Free, ~11–20 guided scenarios (URL shortener, WhatsApp, YouTube) with structured phases (requirements → API → capacity → architecture), drag-and-drop architecture canvas, instant AI scoring. No traffic simulation found. Anonymous creator.
- **System Design School** (systemdesignschool.io): Ex-FAANG-built curriculum + practice questions with instant AI feedback and reusable design templates. Free tier; Pro $45/yr; Lifetime $99. No drag-drop simulation mentioned.
- **System Design Simulator** (systemdesignsimulator.org, by Rahul Kumar): Free, 106 interactive simulators — 27 HLD architectures with walkthroughs, **stress-test mode with synthetic load, and component swaps**; 32 system-internals sims (Raft, LSM-trees, CRDTs); 47 DSA visualizations. Important distinction: **pre-built animated diagrams you perturb, not build-from-scratch** — closer to interactive textbook than game.
- **Desyra** (dev.to launch Nov 12, 2025): Google engineer's side project; drag-and-drop UML canvas, AI feedback on scalability/reliability/security, 82 problems (20 free). Monetization undecided at launch; launch post had 0 comments — a data point on how crowded/quiet this space is.
- **AI mock-interview tools**: mockingly.ai (canvas + AI acting as senior engineer challenging your choices in real time), bugfree.ai (GPT-based mock system design interviewer), MockMe.ai (voice-based AI mock + sketching tool), interviewchamp.ai (question guides). All conversation/critique-based; none simulate traffic.

### Tier 3 — Incumbent content/course/mock platforms (passive or human-based)

- **ByteByteGo** (Alex Xu): Video/text courses + newsletter. ~$50–60/yr subscription; $499 lifetime. Newest interactive feature is in-browser *coding* problems — no architecture simulation. Positioned by reviewers as "gold standard" content.
- **Educative — Grokking Modern System Design Interview**: Text-based interactive courses. Standard ~$149/yr, Premium $199/yr (adds AI mock interviews), Premium Plus $249/yr (adds AWS Cloud Labs); individual courses $79–99. No simulation.
- **DesignGurus.io** (original "Grokking the System Design Interview"): System Design Roadmap ~$198 one-time; annual ~$159.60/yr or ~$53.20/mo; expert mock interviews $100–250/session. Course + text; no simulation.
- **Exponent** (tryexponent.com): Courses + peer/AI mocks, ~$72–$150/yr (sources vary by plan year); expert mocks from $249/hr; 5 free peer-mock credits/month. No simulation.
- **interviewing.io**: Anonymous mock interviews with senior FAANG engineers, from ~$179–225+ per 60-min session (packages reported up to ~$2K for 3 sessions with top-tier interviewers). Human-only, no tooling of this kind.
- **NeetCode**: Two system design video courses ("for Beginners" and "Interview"); third-party reviews note it "doesn't offer interactive exercises" for system design. Notably, NeetCode's own site says the system design section is **deprecated and being replaced with material including "interactive practice problems"** — an incumbent moving toward this space.
- **LeetCode**: No first-party interactive system design product found in any search — only community discussion posts. Its absence is repeatedly cited by the "LeetCode for system design" positioning of the tools above.
- **HackerRank**: has an AI-assisted "System Design Mock Interview" feature (help-center article found); depth unverified.

## 2. Adjacent games (engineering/simulation)

| Game | Mechanic | Commercial results (estimates where noted) | Review themes |
|---|---|---|---|
| **while True: learn()** (Luden.io, 2019, $12.99) | Visual pipe-and-filter puzzles themed as ML; client contracts, upgrades, meta-progression with money | Est. ~253K copies, ~$3.28M gross (Boxleiter estimate); 7,000–7,800 reviews, "Very Positive" (91/100 Steambase) | Praised as accessible; criticized because it "promises ML but delivers pipe-and-filter architecture" — i.e., theme/mechanic mismatch is the top complaint. Directly relevant warning for a system-design game |
| **TIS-100** (Zachtronics, 2015, $6.99) | Assembly programming puzzle, deliberately austere | Est. ~116K copies, ~$813K gross; 3,231 reviews | Outsold the more approachable Infinifactory ~2:1 — evidence that hardcore programmer-audience games can outperform "accessible" ones |
| **Shenzhen I/O** (Zachtronics, 2016, $14.99) | Circuit + assembly design with manual-reading as gameplay | Est. ~129K copies, ~$1.94M gross; 3,587 reviews | Beloved by engineers; niche ceiling around low hundreds of thousands of units |
| **Mini Metro** (Dinosaur Polo Club) | Minimalist network-capacity management under growing demand — arguably the best mechanical analogy for traffic-vs-capacity gameplay | 1M copies by Jul 2017, ~1.3–1.4M by Jul 2018 across PC/mobile/Switch | Elegant "load grows until your network fails" loop is exactly the tension a system design sim needs |
| **Factorio** (Wube, $35) | Factory/throughput/bottleneck engineering | 3.5M copies by end-2022, ~500K/yr with no discounts ever; estimates of $100M+ revenue | Proof that "systems thinking under throughput constraints" is a massive market when the game loop is excellent |
| **Screeps: World** (2016, ~$14.99) | MMO RTS controlled entirely by your JavaScript running 24/7 | ~1,900 Steam reviews, 86% positive; still alive (10th anniversary event Jun–Aug 2026) | Real code as gameplay retains a small, devoted audience for a decade |
| **Elevator Saga** (free browser, open source) | Program elevator scheduling in JS, throughput levels | GamifyList claims ~3M users (unverified); repo magwo/elevatorsaga unmaintained; still gets HN front-page reposts in 2026 | Perennial appeal of tiny "program against simulated load" toys |
| **NetSim** (netsim.erinn.io, free) | Browser lessons where you hand-craft packets to learn routing, spoofing, DoS, MitM | Free education project (CS4G) | Simulation-first teaching of networking works pedagogically |
| **SadServers** (sadservers.com) | "Wordle for SREs": real broken Linux VMs to fix, timed scenarios; used for hiring assessments | Show HN Oct 2022: **597 points, 128 comments**, hit #2 on HN (and got hugged to death) | Huge organic appetite for hands-on infra troubleshooting practice; commenters wanted cheaper/browser-based VMs |
| **iximiuz Labs** | Hands-on Linux/Docker/K8s playgrounds + challenges | **$185K revenue and 21K new users in 2025** (per search result citing its public recap) | A one-person hands-on infra-learning business is viable at ~$200K/yr scale |
| **Wilco** (trywilco.com) | "Flight simulator for developers": quests on a fake-but-real production stack (deploys, incident root-causing) | Raised $7M seed (2022); site now says "Goodbye from Wilco — Wilco is joining Lemonade" (acqui-hire/wind-down; date not stated on page) | Cautionary tale: VC-scale gamified upskilling for devs struggled to become a business |
| **Datacenter sims on Steam** | "Datacenter Simulator" (first-person: rack servers, patch cables, survive power cuts/heatwaves), "Data Center Simulator Game" (campus management, UPS, fires, insider threats), "Startup Company" (build a website business; 2,894 reviews, 80% positive), "Game Dev Tycoon" (22,166 reviews, 94%) | Mixed; tycoon framing of tech infrastructure sells modestly, dev-industry nostalgia (Game Dev Tycoon) sells very well | Physical/business layers, not architecture logic |

## 3. The gap — what exists vs. the proposed concept

**Does anything already combine (a) drag-and-drop architecture building, (b) live traffic/failure simulation, and (c) interview-prep curriculum?**

**Yes — partially, several times over, but nothing has all three done well plus game design plus traction:**

- **SysSimulator** is the closest single product: (a) yes, (b) yes (genuinely sophisticated: WASM discrete-event engine, chaos scenarios, cost model), (c) yes (blueprints + guides + stated interview curriculum). But it is a free, solo-built (explicitly "with assistance from Claude AI") *tool*, not a *game*: no progression, scoring loop, difficulty curve, or business model. No evidence of meaningful distribution.
- **LeetDesign** is the closest to the "LeetCode of system design" framing: (a) yes, (b) yes (deterministic grader + traffic sim naming the bottleneck), (c) yes (22 problems by difficulty). Small problem count, unclear/no monetization, unknown traction.
- **SystemForge / paperdraw / SDApp / systemdesignsimulator.in** are the same idea at prototype quality.
- **InterviewReady's Design Judge** has (a)-ish + (c) + gamification language and a real paying audience (26K+ engineers claimed), but grading appears rubric/judge-based, not live simulation.
- **Hello Interview** has (c) at the highest quality and an AI-read whiteboard for (a), no (b) — and it's the one demonstrably charging successfully ($59/$99/$349).
- **Request Rush** is the only one that is actually a *game* (tower defense of requests), but it's a free prototype with no curriculum and shallow strategy.
- **Joy of System Design** proved the drag-drop-only puzzle version is too thin (repo admits low traction).

**The real gap, precisely stated:** nobody has shipped a *polished, progression-driven game* — failure spectacle, escalating levels, scoring/leaderboards, "Build Dropbox → Build Facebook" campaign — on top of a credible simulation engine, tied to a serious interview curriculum, with a real business behind it. The mechanic space is crowded with free half-projects; the product/game-design and go-to-market space is empty. The competitive risk is less "someone already did it" and more "many people tried the tool version, none converted it into a durable product, and Hello Interview/Codemia can bolt a simulator onto their existing paid funnels." Also note NeetCode is publicly rebuilding its system design section around "interactive practice problems."

## 4. Demand signals

- **Audience size:** ByteByteGo newsletter passed **1,000,000 subscribers (July 2024**, per Alex Xu's own post; 500K in July 2023 — doubling in a year). Alex Xu's book copy counts aren't public (~2,969 Goodreads ratings on one edition), but the newsletter alone bounds the reachable audience in the high hundreds of thousands to millions.
- **Willingness to pay:** Hello Interview $99/yr–$349 lifetime; ByteByteGo $499 lifetime; Educative up to $249/yr; DesignGurus ~$198; expert mocks $100–400/session (interviewing.io from $179, packages reported near $2K). System design prep is one of the most monetized niches in dev education.
- **Explicit dissatisfaction with passive prep:** the Oct 2023 Ask HN thread and Blind threads complain that resources "rehash the same tired examples (Twitter, URL shorteners)," that "all the prep out there is very shallow," and that video/reading is hard to internalize while "interactive resources make it easier to remember." One Blind user highlighted wanting a tool "where you select requirements and watch how the design changes."
- **Hands-on formats get outsized organic reception:** SadServers' Show HN hit 597 points/#2 on HN; Elevator Saga still resurfaces on HN in 2026; iximiuz Labs did $185K in 2025 as essentially a solo project.
- **Builder-side signal:** at least 8–10 independent developers built some version of this in the last ~18 months and posted to Dev.to/Blind/HN — strong evidence the pain is widely felt; equally strong evidence that a bare tool with no distribution gets ~0–2 comments.
- **Game-side signal:** "engineering-brain" games reliably sell 100K–250K+ copies (Zachtronics, while True: learn()) at $7–15, and capacity-management games (Mini Metro 1.4M, Factorio 3.5M+) show the mechanic scales far beyond programmers when the loop is elegant. The recurring review complaint to heed: while True: learn() was dinged for its theme (ML) not matching its mechanic (plumbing) — a system design game has the advantage that the mechanic *is* the subject.

## 5. Verification caveats

- All Steam sales figures are Boxleiter-method estimates from steam-revenue-calculator.com, not audited.
- InterviewReady pricing could not be fetched (redirect loop); "moved from lifetime to yearly in late 2025" is from a third-party search snippet.
- systemdesignsimulator.in and systemdesignsimulator.com pages returned only titles; feature claims for the former come from search snippets.
- Elevator Saga's "3M users" comes from gamifylist.com — weakly sourced.
- iximiuz Labs' $185K/21K figures came via a search-result snippet citing its public recap; not fetched directly.
- Wilco→Lemonade transition date not stated on its goodbye page.
- Alex Xu book unit sales: not publicly verifiable.
- Codemia premium pricing: not disclosed on landing page.

## Sources

- https://syssimulator.com/
- https://leetdesign.com/ (also https://leetdesign.pro/)
- https://dev.to/pratapvhatkar/i-built-a-system-design-simulator-drag-simulate-and-break-your-own-architectures-in-minutes-1jl0 (paperdraw.dev)
- https://github.com/vijaygupta18/system-design-simulator (SystemForge)
- https://github.com/InterviewReady/joy-of-system-design
- https://interviewready.io/course-page/system-design-judge / https://get.interviewready.io/
- https://piggyinabag.itch.io/request-rush
- https://fiastudios.com/works/sdapp
- https://www.systemdesignsimulator.in/ ; https://systemdesignsimulator.com/ ; https://systemdesignsimulator.org/
- https://www.hellointerview.com/practice/overview
- https://codemia.io/
- https://www.systemdesignsandbox.com/
- https://systemdesignschool.io/pricing
- https://dev.to/desyra/i-built-an-ai-tool-to-practice-system-design-like-leetcode-would-love-feedback-47kd
- https://www.mockingly.ai/ ; https://medium.com/@bugfreeai/using-gpt-to-mock-system-design-interviews-9d9215caa84d
- https://javarevisited.blogspot.com/2026/01/is-bytebytego-worth-it-in-2026-for.html
- https://www.designgurus.io/blog/educative-vs-designgurus-system-design-courses-compared ; https://www.designgurus.io/mock-interviews
- https://www.educative.io/courses/grokking-the-system-design-interview
- https://www.tryexponent.com/courses/system-design-interviews ; https://igotanoffer.com/en/advice/best-system-design-mock-interview-platforms
- https://igotanoffer.com/blogs/tech/interviewingio-alternatives ; https://medium.com/@mockingbird_71808/i-paid-225-for-interviewing-io-was-it-worth-the-money-fbc9aee76acb
- https://neetcode.io/courses ; https://grokkingthesystemdesign.com/platforms/neetcode-system-design/
- https://steam-revenue-calculator.com/app/619150/while-true:-learn() ; https://steambase.io/games/while-true-learn/steam-charts
- https://steam-revenue-calculator.com/app/370360/tis-100 ; https://steam-revenue-calculator.com/app/504210/shenzhen-io
- https://www.gamedeveloper.com/design/zachtronics-i-shenzhen-i-o-i-is-a-game-for-people-who-code-games
- https://www.pcgamesinsider.biz/news/67489/mini-metro-has-actually-sold-close-to-14m-copies/
- https://www.pcgamer.com/factorio-has-sold-35-million-copies/ ; https://gamermatters.com/factorio-sells-about-500000-copies-each-year-despite-never-having-sales/
- https://store.steampowered.com/app/464350/Screeps_World/ ; https://www.gamespress.com/Screeps-the-Worlds-First-MMO-RTS-Sandbox-for-Programmers-Celebrates-It
- https://github.com/magwo/elevatorsaga ; https://gamifylist.com/app/elevator-saga ; https://news.ycombinator.com/item?id=47204504
- https://netsim.erinn.io/ ; https://hackmag.com/devops/www-netsim
- https://news.ycombinator.com/item?id=33344142 (SadServers Show HN) ; https://sadservers.com/scenarios
- https://labs.iximiuz.com/ ; https://labs.iximiuz.com/about
- https://techcrunch.com/2022/06/22/roger-wilco/ ; https://trywilco.com/ (Lemonade goodbye page)
- https://store.steampowered.com/app/4178810/ (Datacenter Simulator) ; https://store.steampowered.com/app/1917160/Data_Center_Simulator_Game/ ; https://store.steampowered.com/app/606800/Startup_Company/ ; https://store.steampowered.com/app/239820/Game_Dev_Tycoon/
- https://substack.com/@bytebytego/note/c-21351907 (500K milestone) ; https://x.com/alexxubyte/status/1818676015646097858 (1M milestone)
- https://news.ycombinator.com/item?id=37910952 (Ask HN: system design practice)
- https://www.teamblind.com/post/i-built-a-system-design-simulator-drag-simulate-and-break-your-own-architectures-in-minutes-vs1na4d1 (Blind self-promo threads)
- https://blog.pragmaticengineer.com/system-design-interview-an-insiders-guide-review/
