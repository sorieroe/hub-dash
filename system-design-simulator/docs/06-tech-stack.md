# Tech Stack (researched draft)

Researched 2026-07. Full sourcing in the research agent's report; uncertain items marked. Summary recommendation first, rationale after.

## Recommended stack

| Layer | Choice | Why |
|---|---|---|
| Language | **TypeScript everywhere** | Shared types across UI, sim engine, and level schema; shapez.io's author's own retrospective ("would use TS") |
| Editor UI | **React + Vite + React Flow** (`@xyflow/react`, MIT) | Best-in-class node-graph editing for free (drag, ports, edges, minimap, touch); the de facto standard behind n8n/Stripe/Typeform-class editors |
| Particles & juice | **PixiJS v8 overlay canvas**, synced to the React Flow viewport transform | React Flow's SVG edge animation is a documented CPU trap at hundreds of elements; PixiJS batches *thousands* of request-particle sprites trivially (WebGL/WebGPU) |
| Simulation | **Custom fixed-tick engine (20–30 Hz) in a Web Worker** — seeded PRNG streams, integer time, queueing-station component models (M/M/1 hockey-stick, Kingman, Little's Law, bounded queues, retry storms) | Deterministic → fair leaderboards + kilobyte replays (`seed + input log`); headless in Node → CI balance tests and "is this level winnable?" validation; DES libs (SimScript, discrete-sim) exist but are hobby-grade — reference only |
| Backend | **Supabase** (auth + Postgres + storage) on **Cloudflare Pages** | $0 at 1K MAU; ~$25–100/mo at 100K MAU; Cloudflare's unmetered bandwidth is the "we went viral on HN" insurance. Firebase auth alone would be ~$275/mo at 100K MAU |
| Payments (web) | **Lemon Squeezy** (merchant of record, ~5% + 50¢) | Handles global VAT/sales tax — a real burden with direct Stripe for a tiny team; revisit Paddle past ~$500K ARR. Steam/Apple/Google are their own MoR channels — one `entitlements` table, per-channel unlock checks |
| Steam (later) | **Electron + steamworks.js** | The only maintained Steamworks path for web games; the exact funnel shapez.io rode (free web demo → wishlists → paid Steam build). Tauri has no Steamworks story and weaker canvas perf on macOS/Linux |
| Mobile (later) | **Capacitor** wrapper + IAP | Ships the same web build; React Native would force rebuilding the whole canvas stack. PWA install stays a free bonus channel, not the strategy |
| AI features (v2) | **Claude API** — Haiku-class for design reviews (~$0.003–0.005/call), Sonnet-class for interviewer mode (~$0.01–0.015/call), server-proxied, cached by design-graph hash, gated behind paid tier | At 100K MAU with 10% adoption ≈ $500–1,500/mo — fine *if* paid-gated; noise (<$20/mo) at 1K MAU |
| Levels | JSON + Zod schema, versioned + content-hashed, bundled with the app | Offline/Steam-safe; replays pin the level hash; the same schema powers a future community editor |

## Traps the research explicitly ruled out

- **tldraw SDK** — lovely canvas, but its 4.0 licensing (Sept 2025) requires a **$6,000/yr commercial license** for any commercial production use. Out.
- **Unity / Godot web export** — 12–35 MB wasm baselines kill the "click a link, playing in 5 seconds" virality loop, and neither provides a node editor. Unity's fine print (free under $200K revenue) is survivable, but the web tax isn't.
- **React Flow animated edges for traffic** — the single documented performance trap in this exact architecture. Structural editing in React Flow; *every* per-frame animation in Pixi; never animate via React state.
- **`Math.random()` / `Math.sin` in sim logic** — transcendental float functions differ across JS engines and silently break deterministic replays. Seeded integer-friendly PRNG (sfc32/PCG-class) + lookup tables only.

## Comparison to our existing stacks

hub-dash is a deliberate single-file, no-build, no-dependency PWA — perfect for a personal dashboard, wrong for this: a sim game needs a worker, a renderer, a test suite for the physics, and shared schema types, which means a real build (Vite) and a real repo. The Studworth app's stack is worth a side-by-side once we're in that repo's context; the analytics habit carries over regardless — wire PostHog in from day one (level funnels: attempted → failed → retried → passed is *the* fun metric).

## Architecture sketch

```
apps/web
 ├─ ui/           React + React Flow: palette, canvas, HUD, debrief screens
 ├─ render/       PixiJS overlay: particles, queue pile-ups, saturation glow
 ├─ sim/          pure TS, zero DOM: tick loop, stations, RNG streams, metrics
 │                (runs in a Web Worker in-app; runs in Node for CI/validation)
 ├─ levels/       JSON + Zod schema + content hashes
 └─ shared/       types, replay format {levelHash, simVersion, seed, inputLog}
```

## Walking skeleton (weeks 1–2): prove the fun before building the product

1. **Days 1–2** — Vite + React Flow with 5 hardcoded components (client source, load balancer, app server, cache, DB); drag/wire/delete.
2. **Days 3–5** — sim worker: fixed tick, seeded RNG, M/M/1 stations, bounded queues, one traffic ramp; utilization gauges + p95 readout.
3. **Days 6–8** — PixiJS particle layer: every request a moving dot, backlog as visible pile-up, drops flash and fall. **The make-or-break juice moment.**
4. **Days 9–10** — one real level (URL shortener launch day): brief, ramp, win conditions, win/lose screens.
5. **Days 11–12** — determinism check (same seed + inputs → identical metrics), replay-from-log, speed controls.
6. **Days 13–14** — deploy to Cloudflare Pages, put it in front of ~10 engineers. The fun signal: do they replay a failed level *unprompted*?

No backend, no auth, no payments, no AI in the skeleton.

## Top technical risks

1. **Fun/legibility risk (the big one)** — players must *see why* they failed. The particle layer and the utilization hockey-stick gauge are the product; invest there first. The existing competitors are un-fun interview tools; juice is the moat.
2. **Determinism drift** — integer sim time, RNG streams per subsystem, CI golden-replay tests on V8 + WebKit.
3. **iOS WKWebView memory/WebGL ceilings** for the eventual Capacitor build — test a mid-tier iPhone early; cap particle counts on mobile.
4. **Balance versioning** — any tuning change invalidates replays/leaderboards; version the rules module and partition leaderboards by sim version from day one.
5. **steamworks.js is community-maintained** — pin versions, keep the Steam surface minimal (achievements, cloud saves).
