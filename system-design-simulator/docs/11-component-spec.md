# Component Specification Template

Every palette component (load balancer, SQL DB, cache, …) is one self-contained definition that agents can build in parallel — one worktree per component, judged against this template. **A component is not "done" as code first: its character sheet (visual design) and its physics are designed together and reviewed before implementation** (pre-production gate, doc 14).

## The template (every component must fill all eight sections)

### 1. Identity & taxonomy
`id`, display name, category (entry / compute / state / async / edge / cross-cutting), unlock chapter, palette tier. Variants are *separate components* sharing a family (e.g. `db-sql`, `db-kv`, `db-document`, `db-wide-column`) — because "which database?" is itself interview content.

### 2. Function (the positive claim)
One sentence: what it does for a request. Plus its **connection grammar**: what it may connect to/from, enforced by the editor (an LB feeds stateless things; a cache fronts a store; a queue decouples producer from consumer). Illegal wiring is prevented *visually*, teaching topology by constraint.

### 3. Physics (capacity model)
- Service model: base service rate (req/tick), concurrency, service-time distribution (mean + variance — variance is a gameplay differentiator, e.g. SSD vs disk).
- Queue: bound, overflow behavior (drop / timeout→client-retry).
- Knobs the player can turn: instances, size tier, TTL, replica count, consistency mode… each knob maps to a real interview trade-off.
- Scaling law: what adding instances actually buys (linear for stateless; sub-linear or lesson-laden for stateful).

### 4. Failure modes (the negative space — this IS the curriculum)
For each: trigger condition (deterministic, from sim state), observable symptom (what the player sees), emitted incident event (feeds the debrief), and the named real-world concept (`cache-stampede`, `replication-lag`, `retry-storm`, `spof`). Every failure mode links a `concept_id` (doc 10 `concept_mastery`) and a codex entry.

### 5. Cost curve
$/sim-day as f(size, instances). Stylized but ratio-true (doc 03 §7). Over-provisioning must *hurt scores* — cost pressure is a core mechanic, not flavor.

### 6. Provenance (ground truth)
≥ 2 independent authoritative sources (doc 12 catalog) showing this component in solved interview answers, plus which canonical questions feature it. No provenance → not in the palette. This is the "we never ship an incorrect solution" guarantee, per component.

### 7. Character sheet (visual & audio design — reviewed as concept art BEFORE build)
The art direction (doc 13) governs style; each component defines:
- **Silhouette & metaphor**: readable at 48px on a phone. A database should *read* as "the thing that holds stuff" (chunky silo that visibly fills); a cache as "small and lightning-fast"; an LB as "the traffic conductor."
- **Five load states** with animation cues: `idle` (calm breathing), `warm` (happy hum), `strain` (wobble, color shift, sweat), `overload` (shake, queue visibly piling), `dead` (comedic, not gory — the little house-server catches a tiny fire).
- Transition animations (place/upgrade/die/revive), particle interactions (how requests visually enter/leave), one-line audio cues per state.
- Deliverable format: an SVG character sheet (all five states + transitions storyboard) — SVG because the concept art *is* the eventual web asset base, not a throwaway sketch.

### 8. Test fixtures (physics proven before shipped)
Golden scenarios the headless engine must reproduce exactly, e.g. for `db-sql`: "arrivals at 0.9× capacity → p99 rises but no drops; at 1.2× → queue overflow within N ticks; instance-death with `replicas=0` → all held state lost; with `replicas=2` → promotion after 30 sim-sec, zero data loss." Fixtures are CI — a balance patch that breaks a lesson fails the build.

## Definition of done (the judge checklist, doc 14)

1. Schema-valid definition file (Zod).
2. Character sheet reviewed & approved at the pre-production gate (founder review for the first wave; art-judge agents after the style is locked).
3. All fixtures green in headless CI.
4. Provenance links resolve, ≥2 sources.
5. Codex entry written (≤200 words, interview-vocabulary aligned).
6. Playtest agent confirms the five load states are *visually distinguishable in a running level* (screenshot judge).

## Worked example (abbreviated): `cache`

- **Function**: absorbs repeated reads in front of a store. Grammar: sits between compute and state; cannot be a system's only state.
- **Physics**: service rate 100× `db-sql` read rate; hit ratio emerges from keyspace skew in the traffic profile + TTL knob; misses pass through.
- **Failure modes**: `cache-stampede` (hot key expires under load → herd hits DB → DB overload cascade); `stale-read` (TTL too long for a level that penalizes staleness); `cold-start` (post-death empty cache ≠ recovered system — the second-order lesson).
- **Cost**: cheap per req, memory-priced by size tier.
- **Provenance**: appears in effectively every canonical read-heavy question (URL shortener, news feed, Instagram…) — doc 12 will pin citations.
- **Character**: small, quick, bright; idle = soft pulse; strain = flicker; stampede = it visibly "shatters" and the herd floods past it to the poor database.
- **Fixture**: seed 42, Zipf-skewed reads at 500 rps, TTL 60s, hot-key expiry at t=300 → DB utilization must spike >1.0 within 20 ticks unless request-coalescing knob is on.
