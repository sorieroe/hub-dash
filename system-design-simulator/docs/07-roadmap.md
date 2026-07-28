# Roadmap (draft)

Synthesis of docs 01–06. Each phase ends with a go/no-go gate — the point is to spend two weeks proving fun before spending months building a business.

## Phase 0 — Walking skeleton (~2 weeks): prove the fun

Vite + React Flow + PixiJS particles + sim worker, one level (URL shortener launch day), five components, deterministic replays, deployed to Cloudflare Pages. No backend, auth, payments, or AI. Detail in doc 06.

**Gate:** put it in front of ~10 engineers. Do they replay a failed level unprompted? If the failure spectacle doesn't land, iterate on juice/legibility here — do not proceed on momentum.

## Phase 1 — Free launch (~4–6 weeks): prove distribution

- 3–5 polished scenarios spanning chapter 1 concepts (storage, caching, SPOF) + sandbox mode.
- Wordle-style share cards (uptime %, cost, survival time as an emoji-grid-style artifact) + public forkable design links.
- Anonymous play; Supabase accounts only for saving progress. PostHog funnels from day one (attempted → failed → retried → passed).
- Launch: Show HN, dev.to, r/ExperiencedDevs. Meltdown GIFs as the content format.

**Gate:** a launch spike that *retains* — some cohort returns for the daily challenge without prompting, and shares happen organically. Benchmark from research: successful Show HN = 3.5K–43K visitors; the tell is week-2 retention, not day-1 traffic.

## Phase 2 — Monetize (~2–3 months): prove the business

- Catalog to 15–20 scenarios (levels are JSON — this is authoring, not engineering), chapters 2–4 (write scaling, queues, resilience).
- **Pro tier**: $12/mo / $89–99/yr / $199–249 lifetime early-bird (rationale in doc 05). Lemon Squeezy checkout. Free tier stays genuinely good.
- Firefight mode (inherit a broken system) — the most clippable mode, timed with monetization for a second press beat.
- "Expense this to your company" email generator (CodeCrafters pattern) — near-zero effort, unlocks stipend money.

**Gate:** ≥2% free→paid conversion and flat-or-growing weekly actives. Below that, fix retention before adding channels.

## Phase 3 — Deepen & widen (months 4–9)

- **AI interviewer mode** (the flagship paid feature): Claude-powered "why?" interrogation and conversational requirement twists, server-proxied, cached by design-graph hash. Costs ~$0.003–0.015/call — paid-gate it.
- Chapters 5–7 (geo-distribution, security, the "Design Facebook" finale).
- Creator sponsorships once LTV is known (the Boot.dev channel: they scaled $6K→$110K MRR largely via sponsored YouTube).
- Teams tier + SEO codex build-out.

## Phase 4 — Platform beats (months 9+, each its own go/no-go)

- **Steam**: one-time $15–20 "campaign edition" via Electron + steamworks.js. A marketing beat and five-to-low-six-figure channel, per genre comps — not the business.
- **Mobile companion** via Capacitor: codex review, daily challenge, streaks — not the full canvas.
- Community level editor (levels are already JSON + a headless validator) — only after moderation capacity exists.

## What we're explicitly not doing

Realtime multiplayer, consoles (would force an engine rewrite — see Vampire Survivors' Unity port), VC-scale ambitions before Phase 2 proves conversion (Wilco is the cautionary tale), ads.
