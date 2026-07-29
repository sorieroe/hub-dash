# Raw Research Report: Game-Feel, Art Direction & Progression (2026-07-28)

Verbatim output of the game-feel research agent. Synthesized into `docs/13-game-feel-and-progression.md`; kept raw for the full technique tables, reference analyses, and sources (the art bible and art judges will need them). Claims marked **[secondary/unverified]** come from aggregator blogs.

---

## 1. Juice / Game-Feel Principles

### 1.1 The canon, distilled

**Steve Swink, *Game Feel* (2008)** — game feel = "real-time control of virtual objects in a simulated space, with interactions emphasized by polish." Three building blocks:
1. **Real-time control** — response within **under ~100ms**. For a drag-and-drop canvas: the component must track the pointer with zero perceptible lag; every drop/click/toggle produces feedback inside that window.
2. **Simulated space** — physicality: weight, collision, momentum. Even a diagram can have this — nodes with mass that settle, wires with springiness.
3. **Polish** — animation, sound, particles, camera effects layered on top *without changing the underlying simulation*. Polish is where most of the *feel* lives.

**"Juice It or Lose It" (Jonasson & Purho, GDC 2012)** — a bare Breakout clone iteratively layered with: tweening/easing on every moving element (nothing linearly interpolates), squash-and-stretch on collisions, particle bursts, screen shake, sound on every event, background color pulses, ball trails, blocks that wobble in on spawn, and an eye + face on the ball (instant character). Core doctrine: **"maximum output for minimum input."** The same game goes from clinical to delightful purely via feedback.

**"The Art of Screenshake" (Jan Willem Nijman, Vlambeer, 2013)** — ~30 tricks in order: basic animation + sound → lower enemy HP → higher rate of fire → bigger bullets → muzzle flash → less accuracy → impact effects at collision point → hit animation (flash white) → knockback → **permanence** (debris stays on screen as a record) → camera lerp → screen shake → **hit-stop/"sleep"** (~0.02–0.2s freeze on impact) → kickback → shell casings → more bass → bigger explosions → smoke → meaningful stakes → death animation. Transferable meta-principles: impact events deserve a multi-channel burst (visual + audio + physics + time simultaneously); permanence (scorch marks where a service crashed); random variation in every repeated effect.

**Disney's 12 principles applied to UI** (IxDF, UX Collective) — squash-and-stretch = press states conveying mass; anticipation = hover/wind-ups; secondary action = micro-reactions; follow-through = settling with overshoot; exaggeration = caricatured load states. Map 1:1 to node-canvas interactions.

### 1.2 Which techniques apply to a systems/tycoon-style game

Caveat: **juice must echo the core gameplay** — shooter-density shake is wrong for a management sim; slower games want subtler, more musical feedback; over-juicing a thinking game creates noise that fights readability. The tycoon-appropriate subset:

| Technique | Application |
|---|---|
| Squash & stretch + overshoot easing | Placement: node lands with squash, overshoots scale (0→1.12→1.0, back/elastic ease), settles. Drag pickup = stretch/lift with shadow |
| Particle bursts | Confetti/spark puff on placement, scale-up, test pass. Request "packets" are particles |
| Hit-stop | 100–150ms sim freeze + flash before a cascade-failure animation. Use rarely |
| Screen shake | Biggest events only (region outage, viral spike, meltdown). 2–6px, <300ms, behind reduce-motion check |
| Permanence | Scorch/soot on crashed components; a "war story" scar system |
| Number pop-ups | Floating "+XP", "+$", "p99 ↓ 40ms" ticks; count-up odometers (never snap numbers) |
| Progress ticks | Segmented bars, tick sound per segment, milestone "dings" (goal-gradient leverage) |
| Trails & afterimages | Traffic pulses leave glow trails; throughput = flow density/speed/thickness |
| Idle/ambient animation | Everything breathes at rest — bob, blinking LEDs, spinning fans (Mini Metro/Townscaper aliveness) |
| Anticipation & wind-up | Pre-failure wobble/rattle/red pulse accelerando — telegraphed failure is juicy AND teaches monitoring |
| Audio layers | Every interaction has a sound; sim state modulates a generative music bed |
| Random variation | ±5–10% pitch on SFX, particle jitter, slight rotation randomness |
| Faces/character | Components with eyes/expressions — proven, cheap empathy device |

**Idle-game "number go up" layer** (Pecorella, GDC): exponential curves, always-visible next goal, audio dings as dopamine triggers. Requests-served counter, revenue ticker, uptime meter are natural surfaces; "watch your architecture earn while you think" fits a traffic simulator unusually well.

## 2. Visual Style References

### 2.1 Per-reference analysis

**Mini Metro / Mini Motorways** — flat geometric minimalism; aliveness from **motion + procedural audio**, not detail: trains constantly move, stations pulse when crowding (overcrowding timer = growing ring — load state as geometry), every action triggers musical feedback. Mini Motorways palette "inspired by novelty tourist maps" — saturated, cheerful, colorblind modes designed in (color always paired with shape). **Lesson: minimalism scales to complex graphs and mobile, but requires motion and audio to carry life.**

**Two Point Hospital** — deliberately "Aardman-esque" claymation vibe: chunky, rounded, cute, chosen to be timeless and to make grim subject matter palatable through charm; characters with micro-behaviors. **Lesson: comedy + chunky rounded forms dramatizes failure without punishment — directly applicable to "infrastructure with personality."**

**Monument Valley / Alto's Odyssey** — "every frame worthy of public display"; restrained, premium calm, limited hues, soft gradients. **Lesson: take the finish bar, not the low-feedback pacing.**

**Factorio vs shapez.io vs Opus Magnum** — three flavors of machine-satisfaction: Factorio = dense industrial truth-telling, visually hostile to newcomers; shapez = Factorio's loop "distilled into something cleaner, calmer" (flat pastel minimal — closest existing aesthetic to a friendly systems sim); Opus Magnum = the satisfaction ceiling, clockwork machines where the reward IS watching your machine run, and **one-click GIF export of your solution became its viral loop**. **Lesson: "watching your design run flawlessly" is the genre's core dopamine; a shareable GIF of your architecture surviving a spike is a proven virality mechanic for exactly this audience.**

**Duolingo** — character-driven UI: mascots animated via **Rive state machines** (not baked video): modular pieces (8 head × 8 body animations → 64+ variations) blended in real time, reacting to app state. Files ~10–15× smaller than Lottie; runs web/iOS/Android. **Lesson: the state-machine-driven reactive character system is the exact right pattern for components-as-characters.** Brilliant.org also uses Rive — two learning products independently converged on it.

**Slay the Spire map** — branching node graph as the entire strategy layer; nodes are chunky readable icons; the map is a progression visualization. **Lesson: a level-select screen rendered as a node graph is thematically perfect (architecture diagrams ARE node graphs) and reads fine on phones.**

**Kingdom Rush** — hand-illustrated cartoon TD; readability via silhouettes, saturated faction color-coding, exaggerated proportions; began as a Flash browser game (cheap to render on web). **Lesson: chunky cartoon + silhouette discipline = readable at mobile scale while dense.**

**Townscaper** — the purest "placement feels good" reference: each click instantly generates architecture with a pop sound; the system adds **unearned flourishes** (seagulls, flags) so the world collaborates with you. **Lesson: reward placement with more than you asked for — drop a database and it sprouts a status LED, a hum, a settling dust puff. Emergent detail drives screenshot-sharing.**

**Modern node tools** — tldraw (most polished canvas SDK), Excalidraw (hand-drawn warmth — devs *love* a sketchy aesthetic in technical diagrams), n8n/FigJam (functional, not delightful). **Isoflow/FossFLOW** — isometric infrastructure diagramming "inspired by SimCity" — direct evidence engineers find isometric infra visuals appealing; open-source React. **Lesson: dev-tool canvases prove the interaction layer; none have simulation life — the open space.**

### 2.2 Style-family verdict

Hybrid: **chunky-rounded toy-like components (Two Point/Duolingo DNA, simplified toward shapez/Mini Motorways geometry) on a minimal Mini-Metro-style canvas, with Monument-Valley-grade finish.** Keep the *graph* minimal (thin clean edges, flat background); spend the visual budget on *nodes* (characterful, expressive) and *flow* (glowing traffic pulses). Mirrors what engineers already like (Excalidraw/Isoflow warmth) without pastiche.

## 3. Isometric vs Flat-2D vs Diagram-Style

| Dimension | Flat 2D | Isometric (2.5D) | Diagram-style |
|---|---|---|---|
| Graph readability | Best — nothing occludes | Tall objects occlude; alignment ambiguity | Best-in-class familiar |
| Sim overlays | Clean, one plane | Must respect depth; z-order "a nightmare" | Clean |
| Mobile scaling | Excellent | Diagonal footprints waste screen | Good |
| Emotional richness | Medium-high (Mini Motorways proves it) | Highest "toy world" appeal | Lowest — reads as work software (the anti-goal) |
| Production cost | Low | Medium-high (8-direction sprites, depth sorting) | Lowest |

**Verdict: flat 2D with "2.25D" cheats** — front-facing chunky components with subtle vertical extrusion, drop shadows, parallax. Isometric viable as a later "city view" skin. Preserve the diagram *mental model* (left-to-right flow) so engineers instantly parse it.

### Web tech per approach (benchmark data)

- **SVG/DOM (incl. React Flow)**: workable to ~2k nodes/edges (Cylynx benchmarks); struggles with continuous 60fps particle animation.
- **Canvas 2D (Konva)**: near-constant to ~5k nodes/edges.
- **WebGL (PixiJS)**: ~10k nodes/11k+ edges; hardware sprites, glow/bloom filters, particle systems — the only option making hundreds of traffic pulses + bloom cheap. [One source's benchmarks; treat as indicative.]
- **Production pattern**: DOM/React Flow interaction shell + custom canvas, or a PixiJS scene with thin DOM UI on top (the game-grade architecture).
- **Rive**: state-machine vector animation, official web (WASM/WebGL) runtime; ideal for characters; ~10–15× smaller than Lottie; designed for *interactive input-driven* animation. Duolingo + Brilliant ship it in production web apps.
- **Lottie**: one-shot celebration overlays only (playback, not reactive).
- **Sprite sheets**: heaviest (5s anim ≈150KB vs 30KB Lottie vs ~16KB Rive) but trivially fast in PixiJS; right for dense particles.

## 4. Progression & Retention — Evidence

### 4.1 What's proven

**Duolingo (primary-ish: former CPO Jorge Mazal, Lenny's Newsletter):**
- **Leagues**: opt-out weekly cohorts of 30 with promotion/demotion → **learning time +17%**, highly-engaged learners **tripled**; D1 +1pt, D7 +2pt, D14 +3pt. Most of the 30 are always near gaining or losing something. Lineage: FarmVille 2 leagues.
- **Streaks**: share of DAU with 7+ day streaks nearly **3×'d to over half of all DAU**; streak-saver notification measurably reduced churn; major contributor to 4.5× DAU growth over 4 years. Streak Freeze reduced churn ~21% for at-risk users **[secondary/unverified]**; "streaks = 3.6× long-term engagement, badges +30%" figures are aggregator-blog **[secondary/unverified]**.
- **Failed experiments (instructive)**: a Gardenscapes-style moves-counter mechanic — months of work, "completely neutral. No change to retention." Referrals: +3% new users only. **Ported mechanics that don't fit the core loop do nothing; social-comparison and loss-aversion did the heavy lifting.**

**Boot.dev** (closest comp): XP, levels, leaderboards, guilds, boss battles, **gems** (spendable on XP boosts or saving a "sharpshooter" spree — 15 flawless exercises → random-rarity loot chest). Their streak analog is *accuracy within sessions*, not calendar guilt — a developer-palatable reframe. [Retention attribution is self-reported marketing.]

**Brilliant.org**: deliberately sparse — streaks (3 problems/day) + 10-level weekly leagues + careful difficulty ramps; Rive animations as motivational feedback. A "minimum effective dose" gamification stance for a premium learning brand.

**Daily-challenge model (Wordle / chess.com puzzle)**: scarcity (one/day) creates anticipation, prevents burnout, anchors routine, same challenge for everyone → water-cooler effect; spoiler-free emoji share grid is "one of the most elegant pieces of social design." Direct port: daily architecture incident + shareable result card.

**Codewars**: kyu→dan martial-arts ranks + separate **Honor** (activity currency). Developers respond to *mastery-flavored*, understated ladders; kyu framing signals craft, not casino. LeetCode contest ELO functions as portfolio signal.

**Psych foundations**: Hook model (trigger → action → variable reward → investment — players build architectures they return to, a naturally strong investment step). **Goal-gradient effect** (effort accelerates near goals) and **endowed progress effect** (the car-wash stamp-card study) justify: always-visible next milestone, segmented bars, starting new users at "Level 1 with 20% XP."

### 4.2 What backfires (critical for a developer audience)

- **GitHub removed contribution streaks (May 2016)** — the canonical cautionary tale for exactly this audience. Peer-reviewed ICSE 2021 quasi-experiment: streaks had driven single-commit "streak-keeping" days, weekend working, socially contagious compulsive patterns; after removal, long streaks and token contributions dropped; developers endorsed the removal as ending "an unhealthy relationship" with the metric. **Calendar-guilt streaks are the single riskiest mechanic for this audience.**
- **Streak anxiety generally**: without forgiveness, streaks create compulsion and displace the goal ("Duolingo burnout" is a Reddit genre); Duolingo's own Streak Freeze is evidence forgiveness *increases* retention.
- **Overjuiced gamification saps intrinsic motivation** (overjustification literature, SSRN analysis of Duolingo): when points become the point, perceived learning quality drops — fatal for a "genuinely learn" product.
- **Dark patterns that alienate developers**: coercive notifications (the guilt-owl; ~5% complaint rates **[secondary]**), pay-to-win perception (buying XP/rank poisons a skill-signal ladder — monetization must be content/cosmetics, never rank), fake urgency. Nir Eyal's "peddler vs dealer" test is a usable internal ethics bar.
- **Leaderboard cheating**: Duolingo leagues spawned XP-farming metas; any public ladder for engineers will be botted — rank on *quality of solution* (cost/latency/resilience, Opus Magnum's three metrics), not time-spent XP.

## 5. Sound Design

- Audio is *the* dopamine trigger in incremental games; Jonasson/Purho and Nijman treat sound as inseparable from feel. A sim without sound loses half its aliveness.
- **Mini Metro model (Disasterpeace/Rich Vreeland)**: ~90% of the audio work was *code* — samples triggered by simulation state; no line between SFX and music. Total serialism: each metro line is a musical sequence (length = station count, timbres = station types, dynamics = occupancy); crowded stations *sound* different. **Direct port: each request path is an arpeggio; component type = timbre; load = dynamics/tempo; healthy = harmonious, failing = dissonant — audio becomes a monitoring tool ("you can hear the incident").**
- **Web constraints**: browsers block audio until a user gesture (AudioContext resumed post-gesture; iOS strictest). Standard pattern: unlock on first pointer event (Howler auto-attempts). Never depend on sound for critical info; expect mobile users muted; visible mute toggle.
- **Libraries**: **Howler.js** (de facto standard for game SFX on web; handles iOS silent-mode + autoplay policy). **Tone.js** (DAW-grade scheduling/synths for the generative layer). Architecture: Tone.js procedural bed keyed to sim state + Howler one-shots with ±10% pitch.

## 6. Recommendation (as synthesized into docs/13)

Style: "living toy infrastructure." Palette: warm neutral canvas + saturated tourist-map accents with fixed semantic hues; load states never color-only. Component language: silhouette + load metaphor + face (database silo fills/wobbles/splits; cache vibrates/frosts; LB deals pulses; server with LED eyes and CPU fan, queue as dots bunching at its door). Animation: PixiJS procedural + Rive state-machine characters + one-shot overlays; `prefers-reduced-motion` from day one. Progression: mastery ranks (Intern → Distinguished Architect) from quality scores, separate XP, forgiving weekly-target streaks, opt-out leagues of ~30 ranked on scores, daily incident + emoji share card + GIF export, endowed progress everywhere, money never buys rank. First five juice priorities: placement juice → living traffic → expressive load states → failure spectacle (hit-stop + shake + soot) → number/progress feel; generative audio bed next.

## Sources

**Juice / game feel**
- Juice it or Lose it (GDC Vault): https://www.gdcvault.com/play/1016487/Juice-It-or-Lose ; video: https://www.youtube.com/watch?v=Fy0aCDmgnxg ; summary: https://roblog.co.uk/2024/03/juicy-games/
- Art of Screenshake trick list: https://theengineeringofconsciousexperience.com/jan-willem-nijman-vlambeer-the-art-of-screenshake/ ; https://pepwuper.com/jan-willem-nijman-co-founder-of-vlambeer-on-the-art-of-screenshake/
- Swink, Game Feel ch.1: http://mycours.es/gamedesign2014/files/2014/10/Game-Feel-Steve-Swink-chapter-1.pdf ; https://eolt.org/articles/game-feel/ ; survey: https://arxiv.org/pdf/2011.09201
- Juice mistakes / strategy fit: https://www.gamedeveloper.com/design/6-mistakes-that-ll-drain-the-juice-out-of-your-game ; https://www.gameanalytics.com/blog/squeezing-more-juice-out-of-your-game-design ; https://garden.bradwoods.io/notes/design/juice
- Disney principles in UI: https://ixdf.org/literature/article/ui-animation-how-to-apply-disney-s-12-principles-of-animation-to-ui-design ; https://uxdesign.cc/disneys-12-principles-of-animation-exemplified-in-ux-design-5cc7e3dc3f75
- Idle-game math (Pecorella GDC): https://www.gdcvault.com/play/1023876/Quest-for-Progress-The-Math ; slides: https://media.gdcvault.com/gdceurope2016/presentations/Pecorella_Anthony_Quest%20for%20Progress.pdf

**Visual style**
- Mini Motorways: https://www.gamedeveloper.com/audio/-i-mini-motorways-i-and-the-delicate-art-of-marrying-complexity-and-minimalism ; https://dinopoloclub.com/press/mini-metro/ ; https://twinfinite.net/features/mini-motorways-interview-dinosaur-polo-club-talks-aesthetics-traffic-their-name/
- Two Point Hospital: https://en.wikipedia.org/wiki/Two_Point_Hospital ; https://mcvuk.com/development-news/when-we-made-two-point-hospital/
- Monument Valley: https://gdcvault.com/play/1020878/Designing-Monument-Valley-Less-Game ; https://architizer.com/blog/practice/materials/an-interview-with-ken-wong-of-monument-valley/ ; https://designerfund.com/blog/secrets-behind-the-success-of-monument-valley
- Opus Magnum: https://www.gamedeveloper.com/business/road-to-the-igf-zachtronics-i-opus-magnum-i- ; https://www.pcgamer.com/opus-magnum-review/
- Factorio vs shapez: https://gamefoundry.games/blog/factory-games-most-satisfying-automation ; https://news.ycombinator.com/item?id=25529140
- Townscaper: https://www.gamedeveloper.com/game-platforms/how-townscaper-works-a-story-four-games-in-the-making ; https://cubiccreativity.wordpress.com/2022/11/12/the-videogame-corner-townscaper/
- Duolingo × Rive: https://rive.app/blog/duolingo-s-ai-powered-video-call-brings-lily-to-life ; https://elisawicki.blog/p/how-exactly-is-duolingo-using-rive ; https://dev.to/uianimation/how-duolingo-uses-rive-for-their-character-animation-and-how-you-can-build-a-similar-rive-mascot-5d19 ; Brilliant × Rive: https://rive.app/blog/how-brilliant-org-motivates-learners-with-rive-animations
- Slay the Spire map: https://slaythespire.wiki.gg/wiki/Map_Generation ; https://steamcommunity.com/sharedfiles/filedetails/?id=2830078257
- Kingdom Rush: https://www.nettosgameroom.com/2025/04/kingdom-rush-review.html ; https://malloy.people.clemson.edu/publications/kingdomRush/stith.pdf
- Node tools: https://github.com/tldraw/tldraw ; https://codepic.cc/blog/excalidraw-vs-tldraw ; Isoflow: https://isoflow.io/ ; https://github.com/victortassinari/FossFLOW ; https://news.ycombinator.com/item?id=24168152

**Perspective & web tech**
- Iso vs top-down: https://gamedev.net/forums/topic/666091-tower-defense-style-2d-top-down-isometric-or-3d/ ; https://gamedev.net/forums/topic/658814-isometric-vs-2d-design-what-do-you-prefer-and-why/
- Graph rendering benchmarks: https://www.cylynx.io/blog/a-comparison-of-javascript-graph-network-visualisation-libraries/ ; https://graphaware.com/blog/scale-up-your-d3-graph-visualisation-webgl-canvas-with-pixi-js/ ; https://velt.dev/blog/best-canvas-library-web-mobile-apps ; https://www.svggenie.com/blog/svg-vs-canvas-vs-webgl-performance-2025
- Rive vs Lottie vs sprites: https://rive.app/blog/rive-as-a-lottie-alternative ; https://unicornicons.com/learn/rive-vs-lottie ; https://lottiefiles.com/blog/lottie-animations/lottiefiles-or-rive ; https://sizeim.com/2025/12/01/the-ultimate-guide-to-sprite-sheets-lottie-and-video-for-lightweight-motion-in-ads/

**Progression & retention**
- Duolingo (Mazal, primary): https://www.lennysnewsletter.com/p/how-duolingo-reignited-user-growth ; https://www.reachcapital.com/resources/thought-leadership/product-lessons-from-duolingos-former-chief-product-officer-jorge-mazal/
- Duolingo secondary stats [unverified]: https://trophy.so/blog/duolingo-gamification-case-study ; https://www.strivecloud.io/blog/gamification-examples-boost-user-retention-duolingo
- Streak criticism: https://thedecisionlab.com/insights/consumer-insights/streak-creep-the-perils-of-too-much-gamification ; https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6846283 ; https://nerdsip.com/blog/gamification-gone-wrong-when-streaks-become-the-point
- GitHub streak removal (ICSE 2021): https://neverworkintheory.org/2021/10/03/how-gamification-affects-software-developers.html ; https://arxiv.org/abs/2006.02371v1 ; https://dl.acm.org/doi/10.1109/ICSE43902.2021.00058
- Boot.dev: https://www.boot.dev/lessons/565dd496-0765-4e10-b074-85931fba340f ; https://www.classcentral.com/report/review-boot-dev/ ; https://blog.boot.dev/news/bootdev-beat-2024-05/
- Brilliant: https://trophy.so/blog/brilliant-gamification-case-study ; https://brilliant.org/help/features/
- Wordle/daily: https://uxmag.com/articles/the-fascinating-psychology-tricks-that-make-wordle-so-addictive ; https://factspark.blog/posts/wordle-the-daily-puzzle-that-conquered-the-world
- Codewars: https://docs.codewars.com/gamification/ ; https://github.com/codewars/codewars.com/wiki/Honor-&-Ranks
- Hook model + ethics: https://www.nirandfar.com/how-to-manufacture-desire/ ; https://yukaichou.com/gamification-analysis/hook-model-octalysis-habit-addiction/ ; https://fourweekmba.com/hook-model/
- Goal-gradient / endowed progress: https://www.researchgate.net/publication/239776073 ; https://www.researchgate.net/publication/23547282 ; https://learningloop.io/plays/psychology/endowed-progress-effect

**Sound**
- Mini Metro audio: https://designingsound.org/2016/02/18/the-programmed-music-of-mini-metro-interview-with-rich-vreeland-disasterpeace/ ; https://killscreen.com/articles/music-urban-commute-designing-mini-metros-soundtrack/
- Howler.js + autoplay: https://github.com/goldfire/howler.js/ ; https://github.com/goldfire/howler.js/issues/939 ; https://goldfirestudios.com/howler-js-modern-web-audio-javascript-library
- Tone.js: https://tonejs.github.io/
- Reduced motion: https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion ; https://learn.microsoft.com/en-us/xbox/accessibility/xbox-accessibility-guidelines/117
