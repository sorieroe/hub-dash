# Platforms & Monetization (researched draft)

Researched 2026-07. Sources in the research report; market-size figures are low-tier-vendor estimates, price points are verified.

## The answer to "web, iOS, Steam, or Play Store?"

**Web first. It isn't close.** Every successful comparable is web-first (LeetCode, Boot.dev, NeetCode, CodeCrafters, HelloInterview, Educative, AlgoExpert), and for structural reasons that apply directly to us:

- **No store cut** (Stripe/MoR ~3–5% vs 15–30%), instant updates, and — critical for our growth loop — *shareable, forkable URLs* for designs and results. Store apps can't do "click this link, see my architecture melt."
- The core play session (drag-and-drop canvas + live sim + reading graphs) is a **desktop-browser experience**, the same context where people already grind LeetCode.
- SEO: every scenario is a landing page ("Design Dropbox — interactive"). System design content has proven monster organic reach (ByteByteGo: 0 → 1M newsletter subs in ~27 months).

**Phone apps are a year-2 companion, not the product.** The commute use-case is real but fits *review* content (concept flashcards, incident replays, streak upkeep) — not the canvas. Ship it later via Capacitor, use 15% small-business store rates, and (post-Epic ruling) link out to web purchase where allowed.

**Steam is a marketing beat, not the business.** Genre reality check: *while True: learn()* is the best case at ~$1.8–3.3M gross *lifetime over 7 years*; Screeps managed only ~$478K gross; the median Steam indie does $5–15K lifetime. A $15–20 one-time "campaign mode" port (Electron, after web traction) can add five to low-six figures and press attention — worth doing *once*, never worth leading with. Subscriptions effectively don't exist on Steam, and our model is subscription-shaped.

## Willingness to pay (verified anchors)

| Product | Price | Note |
|---|---|---|
| LeetCode Premium | $35/mo · $159/yr | The category anchor |
| Educative | ~$149–199/yr effective | Grokking's home |
| DesignGurus | $88–138/course · $499 lifetime | Lifetime sells in this niche |
| AlgoExpert/SystemsExpert | $99–129 one-time | Solo-founder-scale success (~$2M ARR est.) |
| interviewing.io | $179–339 *per mock* | Proof of desperation-tier WTP |
| Exponent | $150/yr | |
| Boot.dev | $49/mo · ~$349/yr | Gamified; $10M ARR, 13 people |
| Brilliant | $161/yr · $749 lifetime | Gamified-learning pricing ceiling |

Serious prep clusters at **$149–199/yr or $99–129 one-time**, with punitive monthly pricing pushing annual. System design specifically gates senior+ offers — the highest-WTP segment in consumer edtech (an offer is worth a $10K–100K+ comp delta; interviewing.io's pivot to charging *engineers* proved it).

## Draft pricing

- **Free**: sandbox + 3–5 full scenarios + daily challenge + share cards + public design forks. This tier must be genuinely good — the endgame is Reddit/Blind canonization ("just play X for system design," the way NeetCode became the default answer), and that only happens to a real free tier.
- **Pro — $12/mo or $89–99/yr**: full catalog (20+ scenarios at launch, biweekly drops), graded rubrics against reference architectures, firefight + interview modes, progress analytics. Positioned to be the obvious #2 subscription in a prep stack next to LeetCode's $159.
- **Lifetime — $199–249** (early-bird): monetizes the job-hunt-burst churner, matches how this niche demonstrably buys (AlgoExpert/DesignGurus/Brilliant lifetime tiers), and fits tycoon-game buyer psychology.
- **Teams — ~$79/seat/yr (min 5)** + a prominent **"expense this to your company" flow** (CodeCrafters pattern: engineers have ~$2K/yr L&D stipends; the email-template generator is nearly free to build and unlocks B2B revenue without building enterprise features).

Conversion planning: edtech freemium averages ~2.6%; 3–5% is good; Duolingo's 9.1% took a decade of A/B testing. **Plan around 2–4%.**

## Churn (the known dragon)

Interview prep usage is episodic: grind, get offer, cancel. The market's proven mitigations, all of which we adopt: annual-anchored pricing, a lifetime tier, and — our structural advantage — *genuine-learning* content (campaign as an actual distributed-systems course, codex, daily challenge streaks) that retains people between job hunts in a way pure interview banks can't.

## Growth playbook (in order)

1. **Launch spike**: free sandbox + 3 scenarios + share cards → Show HN, dev.to, r/ExperiencedDevs. (Small "I built a system design simulator" posts have already hit traction on dev.to/Blind — demand signal, and a reason to move fast.)
2. **Share loop**: Wordle-style result cards (emoji-grid style: uptime %, cost, survival time) + forkable public designs. Wordle went <5K → millions of players in weeks *specifically* after adding the share card.
3. **SEO**: one page per scenario + codex entries per concept, compounding.
4. **Short-form video**: architecture-meltdown clips are natively demo-able ("watch this design die at 100K RPS") — content most prep products physically cannot make.
5. **Creator sponsorships** (once LTV is known): the proven paid channel for dev education — Boot.dev scaled $6K → $110K MRR in 15 months largely via ~324 sponsored YouTube channels.
6. **Teams/B2B** last.

## Revenue scenarios (assumptions stated, all estimates)

Blended ARPU ~$95–120/yr across monthly/annual/lifetime mix:

- **Conservative** — 25K signups yr 1 (one good HN launch, modest SEO), 1.5% paid → ~$35–40K/yr. A side project.
- **Moderate** — 100K signups over 18 months (share loop works, SEO compounds), 3% paid → ~$300–350K ARR. AlgoExpert-class solo business.
- **Optimistic** — 300K+ signups by yr 2–3 (sponsorship engine ROI-positive, category-default status), 4% paid → ~$1.4M ARR, plus a Steam port's $100–500K lifetime. Boot.dev ($10M ARR, 13 people) is the demonstrated ceiling for gamified dev education.

## Market size context

Third-party estimates put coding-interview platforms at ~$360M (2024) growing ~15%/yr, interview-prep tools broadly at ~$1.2B. Low-confidence vendor numbers; the honest read is "hundreds of millions per year, double-digit growth, and the system-design slice has the richest buyers and the weakest tooling."
