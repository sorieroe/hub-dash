# Game Feel, Art Direction & Progression (researched draft)

Researched 2026-07 across the game-feel canon (Swink's *Game Feel*, "Juice It or Lose It", Vlambeer's screenshake talk), art references, and published retention data. This doc seeds the art bible (E1) — nothing here is final until the G1 concept review.

## Art direction: "Living toy infrastructure"

**Style family**: chunky-rounded, flat-shaded "2.25D" components **with faces and expressive states** (Two Point Hospital × Duolingo DNA) sitting on a calm, minimal canvas (Mini Motorways DNA), finished to premium-mobile polish (Monument Valley bar). The *graph* stays diagram-legible — left-to-right request flow, thin clean edges — and the entire visual budget goes into the *nodes* (characterful, animated) and the *flow* (glowing traffic pulses). Failure is comedy, never punishment: smoke, dizzy eyes, a tiny fire extinguisher.

Why not the alternatives: pure Mini-Metro minimalism can't do "wobbles and strains" with personality; full cartoon illustration fights diagram readability and costs a fortune; isometric (SimCity-style) has occlusion/depth-sorting problems and wastes phone screens — flat 2D with subtle extrusion and drop shadows gets the toy feel without the costs (isometric stays on the shelf as a possible later "city view" skin — Isoflow proves engineers love the look). Diagram-gray boxes-and-arrows is the explicit anti-goal, but we keep the *mental model* engineers already parse.

**Palette**: warm neutral paper canvas (dark slate for dark mode — table stakes for engineers); saturated "tourist-map" accents with **fixed semantic hues** — requests are always warm amber pulses; healthy teal; strain amber→orange; failure red + desaturation; one celebratory color reserved *exclusively* for rewards. Load states are never color-only (colorblind rule): always paired with fill level, shake amplitude, particle density, and facial expression.

**Component visual language** (feeds every doc-11 character sheet): strong silhouette + one signature *load metaphor* + a face.
- **Database** — chunky silo that *visibly fills* (liquid = disk/pool); strain = wobble + bulge + sweat drop; sharding = the silo splits into two smaller silos with a pop.
- **Cache** — small spring-mounted box that vibrates happily on hits; cold cache = frost that melts as it warms (literalize the jargon).
- **Load balancer** — friendly traffic-cop whose arms physically deal pulses downstream; imbalance = arms visibly favoring one side.
- **Server** — toy computer with status-LED eyes and a CPU fan that spins with load; queue depth = a *physical line of request-dots bunching at its door* (the single most teaching-dense visual in the game).
- Failure is always telegraphed (wobble → rattle → pop) — juicy *and* pedagogically honest (it teaches monitoring) — and leaves **permanence**: a soot mark where something died. Edges: quiet at rest; traffic pulses whose density/speed/thickness encode RPS; latency = literal pulse travel time.

## The five highest-leverage juice techniques (build in this order)

1. **Placement juice** — the most-repeated action must feel great first: drag-lift shadow, drop squash-and-stretch with overshoot, dust puff, "thock" (±10% random pitch), plus a Townscaper-style unearned flourish (drop a DB, it sprouts a status LED and settles with a dust puff).
2. **Living traffic** — glowing request pulses with trails, flowing continuously. The one change that kills "gray boxes with lines."
3. **Expressive load states** — every component's idle-breathe → happy-hum → strain-wobble → telegraphed-failure arc, with queues visibly bunching.
4. **Failure spectacle** — ~120ms hit-stop (micro freeze), white flash, small brief screenshake (2–6px, <300ms, reserved for the biggest events), smoke, cascading dim of downstream nodes, soot permanence. Outages become the memorable moment.
5. **Number & progress feel** — odometer count-ups on RPS/requests-served, floating "+XP" / "p99 ↓ 40ms" pop-ups, segmented tick-tick-ding progress bars.

Guardrail from the literature: **juice must echo the game's tempo** — this is a thinking game, so feedback is musical and layered, not shooter-dense; over-juicing fights readability. Ship `prefers-reduced-motion` support and a shake toggle from day one.

## Animation system (three layers)

1. **Procedural (PixiJS/WebGL)** for everything simulation-driven — pulses, particles, glow, squash-stretch, shake. WebGL is the only layer that makes hundreds of simultaneous pulses + bloom cheap (benchmarks: SVG dies ~2k elements, canvas ~5k, WebGL 10k+).
2. **Rive state machines** for component characters (idle/strain/fail/celebrate, blinking, expressions) — the exact production pattern Duolingo and Brilliant *both* use for their characters; tiny files, designer-ownable, reacts to sim inputs without per-animation engineering. This slots into doc 06's stack as the character layer.
3. **One-shot overlays** (Rive/Lottie) for meta-celebrations (level-up, rank-up).

## Sound: "you can hear the incident"

The Mini Metro model (Disasterpeace): ~90% of the audio is code — samples triggered by simulation state, no line between SFX and music. Our port: each request path is an arpeggio, component type = timbre, load = dynamics/tempo — **a healthy system literally sounds harmonious and a failing one goes dissonant**, making audio a monitoring instrument (perfectly on-theme). Tone.js for the generative bed, Howler.js for one-shot SFX (it handles browser autoplay-unlock and iOS silent-mode quirks). Sound is never load-bearing (mobile users are muted); the visual layer carries feel alone. Build this after the visual loop exists — highest ceiling, not first.

## Progression: mastery, not calendar guilt

Designed around hard evidence, including what *backfires* for developers specifically:

**Adopt (evidence-backed):**
- **Solution-quality ranks, separate from activity XP.** Rank ladder named in-domain (Intern → SRE I → … → Distinguished Architect), earned via score quality (our four gauges — the Opus Magnum three-metric model), Codewars kyu→dan precedent: understated, craft-flavored ladders land with engineers. XP (Boot.dev-style) tracks volume/exploration and *never* buys rank.
- **Weekly leagues, opt-out, ~30-person cohorts with promotion AND demotion** — the single best-evidenced mechanic in learning apps: Duolingo measured +17% learning time and a 3× increase in highly-engaged learners. Ranked on scenario *scores*, with anti-grind design (their leagues bred XP-farming metas — ours rank quality, not volume).
- **Daily incident** — one shared daily challenge + spoiler-free emoji-grid share card (🟩🟩🟨 across cost/latency/uptime) + **Opus-Magnum-style GIF export of your architecture surviving the spike** (Opus Magnum's GIF export was its viral engine; ours is engineered for dev Twitter/LinkedIn).
- **Endowed progress + goal gradient** — new users start mid-progress-bar (the classic car-wash stamp-card study), segmented bars, always-visible next unlock.
- **Streaks, reframed forgiving**: weekly-target framing ("ship 3 days this week") or Boot.dev-style in-session accuracy sprees with reward chests; automatic freezes. Duolingo's own data: forgiveness mechanics *reduced* churn ~21%.

**Avoid (evidence-backed):**
- **Calendar-guilt streaks** — GitHub removed contribution streaks in 2016; the peer-reviewed ICSE 2021 study of the removal found streaks had bred single-commit token days, weekend compulsion, and social contagion, and developers endorsed the removal. This is our *exact* audience telling us what they hate.
- **Pay-to-win anything** — money must never buy rank, XP, or leaderboard position; content and cosmetics only. Purchased rank poisons a skill-signal ladder, and skill-signaling is half the product's value.
- **Guilt notifications** (the shame-owl), fake urgency, loss-framed countdowns — points-become-the-point gamification measurably erodes perceived learning quality, fatal for a "genuinely get better" product.

## What this means for E1 (pre-production)

The art bible operationalizes this doc; character sheets follow the component visual language above; the UI mocks apply the palette + juice rules; and the G1 review question for the founder is concrete: *does a static mock already look like something you'd want to touch?* If a still frame doesn't spark it, motion won't save it.
