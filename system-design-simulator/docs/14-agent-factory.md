# The Agent Factory: How This Gets Built

The build process is itself designed like a pipeline: fan-out where work is parallel (components, levels, art), gates where judgment is human (look, feel, money), and automated judge loops that keep iterating until critics find nothing. Nothing user-facing ships on one agent's opinion.

## Stage gates (hard ordering)

```
G0 Framework review (founder) ──► G1 Concept art & UI review (founder)
      ──► G2 Vertical slice playable (founder + playtest agents)
            ──► G3 Three easy levels end-to-end (judge convergence)
                  ──► G4 Launch readiness (founder)
```

**G1 is absolute: no engine or gameplay code before the founder has approved concept visuals** — style frames, component character sheets, UI screens for every core surface (level select, build canvas, running sim, failure moment, debrief, share card), and an interaction storyboard of one full session. Concept deliverables are SVG/HTML — reviewable on a phone, and reusable as the actual asset base once approved.

## Phase A — Pre-production design (before any build)

Parallel agent tracks, all landing in `design/`:

1. **Art bible** (from doc 13's research): style family, palette, motion language, do/don't board.
2. **Component character sheets**: one agent per component filling template §7 (doc 11) — five load states + transitions, as SVG.
3. **UI screens**: high-fidelity HTML/SVG mocks of the seven core screens, mobile and desktop.
4. **Session storyboard**: the full "Build Shortly" first-session experience, frame by frame, including the first failure moment and the debrief.

Judged by: screenshot-based art judges scoring against the art bible (consistency, at-a-glance readability at 48px, state distinguishability) — then **founder review at G1**. Iterate until approved.

## Phase B — Foundations (post-G1, parallel tracks)

- **Engine track**: headless sim core + fixture harness (doc 11 §8) + determinism CI (golden replays on two JS engines).
- **Canvas track**: React Flow editor + Pixi overlay implementing the approved art.
- **Content track**: doc 12 pipeline produces the three PoC level definitions with provenance.
- **Platform track** (thin until G3): Supabase schema (doc 10), local-first sync, anonymous play.

Component implementation fans out **one worktree per component**, each PR judged against the doc 11 definition-of-done checklist.

## The judge loop (runs continuously from Phase B on)

Every preview deploy triggers a panel, each judge a separate agent with a distinct lens:

| Judge | Lens | Tooling |
|---|---|---|
| Playtest | "Can I complete the level? Where did I get confused? Did I *want* to retry?" | Drives real Chromium via Playwright (pre-installed in these cloud sessions), plays like a player, screenshots every state |
| Physics | "Replay the input log headlessly — do on-screen numbers match the engine? Is the failure the *stated* concept?" | Headless engine + fixtures |
| Pedagogy | "Is the debrief's claim true per the provenance sources? Would an interviewer endorse this lesson?" | Provenance docs |
| Art | "Do screenshots conform to the art bible? Are the five load states legible at mobile size?" | Screenshot diffing vs character sheets |
| Adversarial | "Break it: absurd topologies, spam-clicking, tiny screens, cheating the win condition" | Playwright |

**Convergence rule (loop-until-dry): a build passes its gate when two consecutive full panels produce zero actionable findings.** Findings become tasks; fixes redeploy; panel reruns. The founder is only pulled in at gates or when judges deadlock on taste.

## The task master

`TASKS.md` at the project root is the single backlog: epics → tasks with states (`todo / in-progress / blocked / review / done`), each task sized for one agent session and carrying its judge checklist. Any future session (or scheduled Routine) picks up the top unblocked task, works it, updates the file, commits. It's the coordination point that lets this run across many sessions without a human dispatcher.

## Standing guardrails

- Agents never decide **pricing, branding/name, art direction acceptance, or anything money-touching** — founder-only, at gates.
- All research claims in shipped content trace to provenance; judges reject "sounds right."
- Determinism CI is sacred: a PR that breaks golden replays cannot merge.
- Balance changes bump `sim_version` and partition leaderboards — never silently rewrite history.
- Visual changes post-G1 must cite the art bible; art drift is a judge finding.

## Cadence after G3

The steady-state loop for growing the catalog: content agent drafts level from provenance → fixtures + headless winnability check → art pass → judge panel → publish to CDN catalog. Target: one new level per week without founder involvement beyond spot checks, with the same convergence rule guarding quality.
