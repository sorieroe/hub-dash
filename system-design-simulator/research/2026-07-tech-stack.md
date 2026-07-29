# Raw Research Report: Technical Architecture (2026-07-28)

Verbatim output of the tech-stack research agent. Synthesized into `docs/06-tech-stack.md`; kept raw for full comparison matrices and sources. Confidence markers: claims tagged **[uncertain]** need verification.

---

## 1. Canvas / Graph UI Layer

### Comparison matrix

| Option | Node-graph editing | Particle animation (100s–1000s) | Mobile touch | Bundle (gzipped, approx.) | License / cost | Maturity |
|---|---|---|---|---|---|---|
| **React Flow (@xyflow/react)** | Excellent, purpose-built (drag, ports, edges, selection, minimap, undo helpers) | Weak natively — SVG `stroke-dasharray` edge animation is a documented CPU bottleneck at hundreds of edges | Good (built-in touch/pinch) | ~45–55 KB **[uncertain, order-of-magnitude]** | Core MIT forever; Pro subscription ($169–$289/mo) is only for pro examples/support, not required | Very mature; v12 (2025); used in production by Stripe, Typeform, Shopify; the de facto standard behind n8n-style editors |
| **tldraw SDK** | Excellent infinite canvas, but whiteboard-shaped, not node-graph-shaped (ports/edges are DIY) | Moderate | Excellent | Several hundred KB **[uncertain]** | **SDK 4.0 (Sept 2025): commercial production use requires a paid license — $6,000/yr for teams ≤10; free hobby license is non-commercial only (watermarked); 100-day trial** | Mature, but the licensing change makes it a poor fit for a commercial indie game |
| **PixiJS (v8)** | DIY (you build node editing yourself) | Excellent — WebGL/WebGPU sprite batching, thousands of moving sprites is its core use case; supports OffscreenCanvas + Web Worker rendering via `WebWorkerAdapter` | Good (you wire gestures) | ~100 KB **[uncertain]** | MIT, free | Very mature, active v8 line |
| **Konva** | Middle ground — scene graph with built-in drag/hit-testing, good for "UI-ish" canvases | Mediocre — Canvas2D; own docs concede PixiJS wins for many animated objects | Good | ~50 KB **[uncertain]** | MIT, free | Mature |
| **Plain SVG/Canvas** | All DIY | SVG: no (DOM cost); raw Canvas2D: OK to ~1–2K simple particles **[well-known, unsourced]** | DIY | 0 | — | — |
| **Phaser** | DIY node editing (it's a game framework, not an editor toolkit) | Excellent | Good | ~300 KB+ **[uncertain]** | MIT, free | Very mature; Vampire Survivors originally shipped on it |
| **Godot 4 web export** | DIY; poor DOM/text interop | Excellent in-engine | OK but web export is its weak spot | **~25–35 MB wasm for an empty project** — disqualifying for a web-first product | MIT, free | Web export improving (4.5 added wasm SIMD) but still WebGL2-only, Safari flakiness |
| **Unity WebGL** | DIY | Excellent in-engine | Historically poor mobile-browser support | ~12–15 MB minimum | Runtime Fee cancelled (Sept 2024). Personal free under $200K revenue/funding, splash optional in Unity 6; Pro $2,200/seat/yr above $200K | Mature, but web builds are heavy; mobile-browser Unity officially discouraged **[widely reported, not in fetched sources]** |

### Key findings

- **The critical performance trap is animating request particles as React Flow edges.** React Flow's built-in "animated edges" use CSS `stroke-dasharray`, which multiple production teams (ROUTE06 / Liam ERD) found to be the main CPU bottleneck at hundreds of simultaneous animated SVG elements. React Flow itself is fine for *the editor* (drag/drop, ports, selection, viewport) at the node counts this game needs.
- **The proven pattern is a hybrid: React Flow for the graph editor + a WebGL canvas layer (PixiJS) for particles**, positioned in the same viewport and synchronized with React Flow's pan/zoom transform. PixiJS comfortably batches thousands of sprites, and can render from a worker via OffscreenCanvas.
- **tldraw is out** for a commercial product unless you pay $6K/yr from day one — its 4.0 licensing (Sept 2025) requires a commercial license for any commercial production use.
- **Game engines are the wrong tool for the editor half**: Godot's 25–35 MB baseline wasm and Unity's 12–15 MB + mobile-browser weakness kill "instant web demo" virality, and neither gives you a node-graph editor.
- **What similar products actually use:** diagramming/node tools (n8n, Langflow, Dify, Stripe docs, Typeform internals) → React Flow; whiteboards (Excalidraw = custom canvas; Miro/Figma = custom WebGL) **[common knowledge, unsourced]**; web factory games (shapez.io) → custom JS engine on raw canvas; the direct competitor set mostly uses React + drag-drop canvas libraries with light SVG animation — none does high-density particle flow well, which is an opportunity.

### Direct competitors found during this research

- **systemdesignsimulator.org** — free; stress-test mode showing which component saturates first.
- **systemdesignsandbox.com** — drag-drop components + AI feedback.
- **ScaleDojo (scaledojo.dev)** — challenges + "Murphy's Lab" chaos simulator + AI feedback (closest to the concept incl. AI reviewer).
- **Paperdraw.dev** — drag-drop + live latency/error-rate/cache-hit simulation.
- **InterviewReady's "Joy of System Design"** (open source), **Request Rush** (itch.io tower-defense-style requests game), a Medium-published game by Rodolfo Marcos, and vijaygupta18/system-design-simulator (GitHub).

None appears to be a polished *game* (levels, win conditions, juice, Steam) — they're interview-prep tools. The gamified/Steam angle looks open.

## 2. Simulation Engine

### DES vs fixed-tick — recommendation: **fixed-tick core with queueing-theory-informed service models**

- **Discrete-event simulation (DES)** (SimPy-style) gives exact queue dynamics. Prior art in JS/TS: **SimScript** (TypeScript, includes M/M/C examples), **discrete-sim** (TypeScript, SimPy-inspired generators, zero deps), sim.js (old), simjs-updated. All are small hobby projects — fine as *reference implementations*, not foundations. DES doesn't map naturally onto a 60 fps visualization of continuous flow.
- **Fixed-tick simulation** (how tower-defense/factory games work) advances the world in constant steps (20–30 sim ticks/sec, decoupled from render at 60 fps with interpolation — the standard "fix your timestep" pattern). Trivially deterministic, trivially serializable, maps 1:1 to visible particles.
- **Hybrid**: run fixed ticks; within each tick, each component behaves as a queueing station: arrivals → bounded queue → N servers with seeded service-time distributions → departures routed downstream. Model individual request entities so particles are real — cap visible entities and switch to statistical aggregation ("1 particle = 10 requests") above a threshold.

### Queueing theory worth encoding (well-established math)

- **Utilization**: ρ = λ/μ per component. The whole game loop is "keep ρ < 1 everywhere as λ grows."
- **M/M/1**: mean queue length L = ρ/(1−ρ); mean latency W = 1/(μ−λ). The *hockey-stick*: latency flat until ~70–80% utilization then explodes — the single most teachable curve in the game.
- **Little's Law**: L = λW — sanity check inside the sim and teaching overlay.
- **Kingman's approximation**: Wq ≈ (ρ/(1−ρ)) · ((Ca²+Cs²)/2) · (1/μ) — makes "SSD DB" vs "spinny disk DB" differ by variance, not just mean.
- **Tail latency**: per-request latency histograms, p50/p95/p99; win conditions phrased as real SLOs. Fan-out amplifies tails — an advanced-level mechanic.
- **Failure modes to encode**: bounded queues → load shedding (visible dropped particles), retry storms (retries add λ → metastable congestion collapse), cache hit ratio shifting λ off the DB, replication lag, thundering herds on cache expiry.

### Determinism, replays, workers

- **Seeded PRNG**: small fast seedable generator (mulberry32/sfc32/PCG-class). One RNG stream per subsystem so adding a component doesn't perturb unrelated draws. All randomness through the seeded RNG; never `Math.random()`.
- **Cross-browser float determinism**: basic IEEE-754 ops reproduce across JS engines; **transcendentals (`Math.sin`, `Math.pow`) are implementation-defined and can diverge** — avoid in sim logic or use lookup tables. Integer/fixed-point math for all sim state is the bulletproof route lockstep games use.
- **Replays** = `{levelId, levelContentHash, simVersion, seed, inputLog}` (tick-stamped edit events). Kilobytes; a replay is just re-running the sim. Version-stamp with engine version — balance changes break old replays; keep old rule modules or invalidate gracefully.
- **Web Worker architecture**: sim runs in a dedicated worker at fixed tick rate; posts compact per-tick snapshots (transferable Float32Arrays) to the main thread; UI sends tick-stamped input events in. Optionally render off-main-thread too (PixiJS WebWorkerAdapter + OffscreenCanvas) — an optimization, not a requirement; Safari's OffscreenCanvas-WebGL support has historically lagged **[verify current status]**.
- **Sim-speed controls** (pause/1×/4×/step) fall out of fixed-tick for free.

### Concrete recommended architecture

```
apps/web
 ├─ ui (React + React Flow): editor, HUD, charts (uPlot/lightweight)
 ├─ render (PixiJS overlay): particles, saturation glows, synced to RF viewport
 ├─ sim-worker (pure TS, zero DOM deps):
 │    fixed tick 20–30 Hz · seeded RNG streams · integer time
 │    component behaviors as data-driven "server station" models
 │    metrics ring buffers (per-component ρ, queue depth, latency histos)
 └─ shared: level schema types, replay format, protocol types
```
Sim package must run headless in Node — balance tests in CI, fast-forward level validation ("is this level winnable?"), later server-side replay verification for leaderboards (anti-cheat).

## 3. App Shell & Backend

### Frontend framework/hosting
- The game is client-side; SSR buys nothing inside the game. **Vite SPA** (+ static/Astro marketing site) beats Next.js. 
- **Hosting cost**: Cloudflare Pages has **unlimited bandwidth on the free tier**; Vercel free = 100 GB, Pro = $20/seat with 1 TB then $40/100 GB overage. ~2 TB/mo workload shows ~$2,400/yr difference. 1K MAU: $0. 100K MAU: still ~$0–5/mo static hosting on Cloudflare (Workers Paid $5/mo if needed).

### Auth + data + sync

| | Supabase | Firebase | Convex |
|---|---|---|---|
| Model | Postgres + auth + storage + realtime | Firestore per-operation billing | Reactive functions/compute billing |
| 1K MAU | Free ($0) | Free | Free |
| 100K MAU | **$25/mo Pro covers 100K MAU, 8 GB DB, 250 GB bandwidth**; overages modest | Auth alone ~$275/mo at 100K MAU; read/write-heavy workloads 3–5× (up to 10×) Supabase cost | Middle; predictable; smaller ecosystem |
| Fit | **Best**: relational fits levels/progress/leaderboards; RLS; SQL leaderboards trivial | Surprise-bill risk with chatty clients | Nice DX for realtime, but little realtime needed |

**Recommendation: Supabase.** Total backend at 100K MAU plausibly **$25–100/mo**.

### Payments (web) — merchant of record matters
- **Stripe direct**: 2.9% + 30¢ but *you* own global VAT/sales-tax registration and filing.
- **MoR options**: **Lemon Squeezy** (5% + 50¢, indie-friendly, license keys built in; Stripe-owned **[acquisition announced mid-2024; verify current status]**) or **Paddle** (~5% + 50¢, better past ~$500K ARR). Both collect/remit tax in 100+ jurisdictions.
- Rule of thumb: global + small → Lemon Squeezy (or Polar); past $500K ARR → Paddle; US-only → Stripe.
- **Steam (30%) and Apple/Google IAP (15–30%) are their own MoR channels** — one `entitlements` table in Supabase, per-channel unlock checks.

### Level content pipeline
- **Levels as data**: JSON (authored as YAML/TS, compiled), validated with Zod/JSON Schema. Ship official levels in the bundle (offline/Steam-safe); fetch level-pack updates from CDN by content hash. Keep `schemaVersion` + migrations; replays pin `levelContentHash`.
- Community levels later: Supabase storage + moderation flag; headless sim validates solvability before publishing.

## 4. Cross-Platform Path

### Mobile: **Capacitor**, not React Native, not bare PWA
- **Capacitor** wraps the exact web build in WKWebView/Android WebView with native plugins (IAP, haptics, push). Days of work, one codebase.
- **React Native** would mean rewriting the entire canvas/rendering stack — non-starter.
- **PWA install**: free bonus channel only; iOS PWAs can't use App Store IAP, storage-eviction risk.
- Plan for: WKWebView memory ceilings for WebGL **[well-known]**, audio unlock gestures, App Store review of thin wrappers (games generally pass — fully interactive).

### Steam: **Electron + steamworks.js**
- **Electron (or NW.js) + `steamworks.js`** (ceifa; successor to semi-abandoned Greenworks) for achievements, cloud saves, overlay, Workshop — the only maintained Steamworks bindings for web games. **Tauri has no supported Steamworks path and uses WebKit on macOS/Linux (slower canvas/WebGL)** — wrong choice despite its popularity.
- Precedents:
  - **Vampire Survivors**: Phaser → Steam via Electron; ported to Unity from v1.6 (2022) for performance and console ports. Web stack got them to PMF; consoles required a rewrite.
  - **shapez.io**: custom JS engine, canvas; free web demo + paid Steam build via Electron; the exact funnel (web demo → wishlists → purchase). Author would use TypeScript if starting again.
  - **CrossCode**: HTML5 on Steam via NW.js; console ports needed a specialist partner **[recalled, not verified]**.
- Gotchas: Electron bundle ~100 MB+, Steam Deck/Proton generally fine **[uncertain]**, keep fully offline-capable (Steam users hostile to login walls), integrate achievements early.

### Sequencing implication
Web first (free levels + paid unlock via Lemon Squeezy) → Steam (Electron, paid up-front) → mobile via Capacitor (IAP unlock). Consoles: out of scope for this stack (engine rewrite; accept the trade now).

## 5. AI Integration (Claude API)

**Feasibility: high, and a genuine differentiator.** Two features:

1. **AI reviewer/interviewer**: serialize player's graph + sim metrics to compact JSON (~1–3K tokens), send with a cached system prompt (rubric, component semantics, level brief), get structured critique.
2. **Scenario generation**: traffic-profile variants / twist events for existing levels, validated by the headless sim before use (never ship raw model output as a level).

**Cost model** (Claude API pricing at research time: Sonnet-class $3/M input, $15/M output — intro $2/$10 through 2026-08-31; Haiku-class $1/$5; Opus-class $5/$25):
- Typical review call ≈ 2K input (mostly cache-hit) + 600 output → **Sonnet ≈ $0.010–0.015/review; Haiku ≈ $0.003–0.005** (likely sufficient for rubric critiques).
- Heavy user (10 reviews/mo) ≈ $0.05–0.15/mo. 100K MAU × 10% adoption × 10 reviews ≈ **$500–1,500/mo** — gate behind paid tier or daily free quota. At 1K MAU it's noise (<$20/mo).
- Scenario generation → Batches API at 50% discount.
- Ops: proxy through backend (never ship the API key), rate-limit per account, cache reviews keyed by design-graph hash (~30–50% cost cut **[estimate]**).

## 6. Recommendation

(Full table as synthesized into docs/06.) Walking skeleton weeks 1–2: React Flow canvas w/ 5 node types → sim worker (M/M/1 stations, ramp) → PixiJS particle overlay (**the make-or-break juice moment**) → one full URL-shortener level → determinism check + replay → deploy to Cloudflare Pages, 10-engineer playtest. Fun signal: do they replay a failed level unprompted?

Main risks: (1) fun/legibility; (2) React re-render perf (keep React Flow structural; animate in Pixi); (3) determinism drift (integer math, golden replays CI on V8+WebKit); (4) iOS WKWebView limits; (5) steamworks.js maintenance (pin versions); (6) AI cost creep / prompt injection via community levels (quota + graph-hash cache; treat level text as untrusted); (7) tldraw/Unity licensing avoided by construction; (8) balance versioning (partition leaderboards by sim version from day one).

## Sources

**Canvas/UI layer**
- https://reactflow.dev/examples/nodes/stress · https://reactflow.dev/pro · https://www.npmjs.com/package/@xyflow/react · https://github.com/xyflow/xyflow
- https://dev.to/route06/tuning-edge-animations-in-reactflow-for-optimal-performance-3g32 · https://liambx.com/blog/tuning-edge-animations-reactflow-optimal-performance
- https://github.com/xyflow/xyflow/discussions/4975 · https://velt.dev/blog/react-flow-guide-advanced-node-based-ui
- https://tldraw.dev/pricing · https://tldraw.dev/community/license · https://biggo.com/news/202509190115_tldraw_SDK_4.0_Licensing_Debate · https://tldraw.dev/blog/20-things-i-wish-ai-chatbots-knew-about-tldraw
- https://aircada.com/blog/pixijs-vs-konva · https://konvajs.org/docs/faq.html · https://konvajs.org/docs/sandbox/Jumping_Bunnies.html · https://www.pkgpulse.com/blog/fabricjs-vs-konva-vs-pixijs-canvas-2d-graphics-libraries-2026
- https://app.cinevva.com/guides/godot-vs-unity-web-games · https://dev.to/ziva/godot-4-fur-web-spiele-export-wasm-und-browser-performance-4315 · https://github.com/JohannesDeml/Godot-Web-LoadingTest · https://best-games.io/blog/godot-web-export-optimization-guide

**Simulation**
- https://github.com/Bernardo-Castilho/SimScript · https://www.discrete-sim.dev/ · https://simjs.z5.web.core.windows.net/ · https://github.com/btelles/simjs-updated
- https://gafferongames.com/post/deterministic_lockstep/ · https://simplified.media/guides/fixed-timestep-loops · https://jakubtomsu.github.io/posts/fixed_timestep_without_interpolation/ · https://bugnet.io/blog/how-to-debug-desync-in-deterministic-lockstep-games · https://github.com/pietrobassi/deterministic-lockstep-demo
- https://web.dev/articles/offscreen-canvas · https://pixijs.com/8.x/guides/concepts/environments · https://github.com/pixijs/pixijs/issues/5105 · https://developer.mozilla.org/en-US/docs/Web/API/OffscreenCanvas

**Backend/payments/hosting**
- https://makersden.io/blog/convex-vs-supabase-2025 · https://justinmckelvey.com/blog/supabase-vs-firebase · https://makerkit.dev/blog/saas/supabase-pricing · https://zuplo.com/learning-center/api-authentication-pricing · https://cadence.withremote.ai/blog/convex-vs-supabase-vs-firebase
- https://www.globalsolo.global/blog/stripe-vs-paddle-vs-lemon-squeezy-2026 · https://www.artisangrowthstrategies.com/blog/paddle-vs-stripe-vs-lemon-squeezy-2026 · https://www.buildmvpfast.com/blog/lemon-squeezy-vs-polar-paddle-merchant-of-record-2026
- https://dev.to/johalputt/contrarian-view-2026-startups-should-skip-vercel-for-nextjs-15-use-cloudflare-pages-for-40-5gl4 · https://speedvitals.com/blog/cloudflare-pages-vs-vercel/ · https://www.devpick.io/compare/cloudflare-pages-vs-vercel

**Cross-platform**
- https://www.webgamedev.com/publishing/desktop · https://github.com/ceifa/steamworks.js/ · https://github.com/greenheartgames/greenworks · https://liana.one/integrate-electron-steam-api-steamworks
- https://capacitorjs.com/ · https://capgo.app/blog/transform-pwa-to-native-app-with-capacitor/ · https://ionic.io/resources/articles/building-cross-platform-apps-with-capacitor
- https://en.wikipedia.org/wiki/Vampire_Survivors · https://phaser.io/news/2024/02/vampire-survivors-space-54 · https://github.com/tobspr-games/shapez.io · https://github.com/tobspr-games/shapez-community-edition
- https://www.cgchannel.com/2024/09/unity-scraps-controversial-runtime-fee-but-raises-prices/ · https://unity.com/blog/unity-is-canceling-the-runtime-fee · https://unity.com/products/pricing-updates

**Competitors**
- https://systemdesignsimulator.org/ · https://www.systemdesignsandbox.com/ · https://scaledojo.dev/ · https://github.com/InterviewReady/joy-of-system-design · https://piggyinabag.itch.io/request-rush · https://dev.to/pratapvhatkar/i-built-a-system-design-simulator-drag-simulate-and-break-your-own-architectures-in-minutes-1jl0 · https://rodolfo-marcos07.medium.com/i-read-system-design-interview-twice-then-i-built-a-game-so-you-dont-have-to-f3f866b30b1f · https://github.com/vijaygupta18/system-design-simulator

**AI pricing**: Anthropic Claude API pricing via the claude-api skill (cached 2026-06-24): Sonnet-class $3/$15 per MTok (intro $2/$10 through 2026-08-31), Haiku-class $1/$5, Opus-class $5/$25; Batches API −50%; prompt caching reads ~0.1×.
