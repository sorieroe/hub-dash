# Founder Context

Context from the founding conversations (2026-07) that isn't captured elsewhere in docs 01–14. Read this first if you're an agent/session starting fresh — it calibrates how to work with the founder, not what to build.

## Working relationship & decision authority

- The founder is an experienced engineer but has **no game-development background** — game-design and art decisions should be research-grounded and evidence-cited (as docs 13/14 do), presented for taste-check at gates rather than assumed. "Use someone else's testing as a source of truth" is their stated preference for progression/retention mechanics.
- The founder wants Claude running **continuous build → deploy → playtest → judge loops** through the whole lifecycle (docs/14), with founder involvement concentrated at the G0–G4 gates. Iterate until AI judges find nothing left to improve, then bring it for human review.
- Hard rule restated by the founder twice: **nothing playable gets built before concept visuals are approved** (look, component/character design, UI screens, session flow). This is gate G1 and it is absolute.

## Sibling projects (pattern mines, not dependencies)

The founder's other active repos are **softjot**, **softshot**, and **studworth** — described as "built pretty well and detailed." When work touches a problem those apps already solved (auth flows, payments, analytics wiring, deploy setup, mobile packaging), **attach the relevant repo read-only via the GitHub MCP / `add_repo` and mine it for patterns and lessons-learned** before re-deriving from scratch. Attach per-need, not up front. Related: the founder's PostHog org is "Studworth" — the analytics habit from those projects carries over here (PostHog from day one, per docs 06/09).

The **hub-dash** repo is the founder's meta/ops dashboard connecting their project fleet — this project was drafted on a branch there only because the original cloud session couldn't create repos. It is not a code source for this project.

## Design nuances from the founding conversation not fully captured in docs 02–03

1. **The sim should be able to run continuously while the player edits.** The founder described building "while it runs… showing how it lets everything pass through and work" — live editing against running traffic is a first-class interaction (not only discrete build→run→debrief cycles). Doc 02's Firefight mode covers the repair case; the campaign loop should also support hot-editing, with the phase structure layered on top.
2. **Data modeling is explicit study content**, not just an implementation detail. The founder called out data-modeling interviews specifically. Levels and debriefs should exercise schema/access-pattern choices (SQL vs KV vs wide-column per workload, partition-key choice) as *named, scored decisions* — the component variants in doc 11 carry this, but level rubrics should surface it explicitly.
3. **Firsthand competitive impression (founder's own eyes, not agent research):** they looked at LeetDesign and found it "not fun or approachable… you have to type in things… kind of boring" — same read on the other prototypes. This is direct validation that the game-feel bet (docs 13) is the differentiator, from the target buyer themselves.
4. **The "good addiction" framing** is the product's ethical north star in the founder's words: addictive like a game people *want* to return to, defensible because every session genuinely improves a valuable skill. Doc 13's anti-dark-pattern guardrails are the operationalization; keep that intent when tuning retention mechanics.

## Loose ends from the founding sessions

- The founder may have an **old repo with an earlier version of this idea** somewhere on their GitHub account — never confirmed (account listing needed an interactive approval that wasn't available). Worth a quick check from a local session; mine it if it exists.
- Naming (docs/08 #1) remains the founder's call; the descriptive name "system design simulator" is squatted three ways (.org/.in/.com) — the repo name is a placeholder, not the brand.
