# Simulation Model ("the physics")

The engine's job: make the *pedagogically true* consequences of a design emerge from simple local rules, so failures feel discovered rather than scripted. Everything below is deterministic given a level seed — same design + same seed = same outcome (reproducible levels, fair leaderboards, shareable replays).

## 1. The world model

- **Fixed-tick simulation** (e.g. 20 ticks/sec of simulated time, time-compressed: 1 real second ≈ 1–15 sim minutes depending on level pacing).
- A design is a **directed graph**: nodes are component instances, edges are connections. Traffic sources inject **requests**; requests are typed (`read`, `write`, `upload`, `auth`, …) and carry a payload size and a data-criticality flag.
- Each request traverses the graph; at every node it consumes capacity, accrues latency, and either proceeds, gets queued, gets dropped, or fails. Requests carrying critical data that get dropped increment **data lost** — the most visible, most damning metric.

## 2. Universal component laws

Every component shares the same small rulebook (this keeps the engine simple and the lessons general):

1. **Capacity**: max requests/tick it can service (scaled by instance count / size knob).
2. **Queue**: bounded buffer in front of it. Little's Law does the teaching: as arrival rate approaches capacity, queue length and wait time explode. Latency multiplier ≈ `1 / (1 - utilization)` capped at queue overflow.
3. **Overflow behavior**: full queue ⇒ requests dropped (or timed out ⇒ clients **retry**, amplifying load — the retry-storm cascade emerges naturally).
4. **Failure**: components can die (chaos event or wear). A dead node routes nothing; whatever state it *exclusively* held is gone unless replicated/backed up.
5. **Cost**: every component bills $/sim-day. Budget is a level constraint, so over-provisioning is a scoring decision, not a free move.
6. **Blast radius**: the engine computes SPOFs live (articulation points in the graph + unreplicated state) — surfaced in scoring and in the "resilience" HUD.

## 3. Component catalog (v1 palette)

Each entry: what it does (its "positive function") and the negative space around it (what its absence, misuse, or limits cause). This table *is* the curriculum.

| Component | Function | Emergent lessons (absence / misuse / limits) |
|---|---|---|
| **Client swarm** (level-defined) | Generates typed traffic per profile | Retries on timeout (storms); abandons after bad latency (rage-quit metric) |
| **DNS** | Entry point, weighted routing | Misconfig/TTL lessons kept minimal in v1 |
| **Load balancer** | Spreads traffic across siblings | Without: one server saturates while twins idle. Itself a SPOF unless paired. Health checks: without them, it routes to dead nodes |
| **App server** | Services requests; stateless by default | Saturation → latency curve → timeouts. Storing state here (allowed early!) makes horizontal scaling silently wrong — sticky-session lesson |
| **SQL database** | Durable state, transactions, joins | **Absence: writes drop on the floor (the level-1 lesson).** Write throughput ceiling; single node = data annihilated on failure; read replicas add **replication lag** → stale-read incidents |
| **NoSQL / KV store** | High write throughput, simple lookups | No joins/transactions — levels that need consistency punish it; teaches "choose per access pattern," not "NoSQL = webscale" |
| **Cache** | Absorbs repeated reads | Hit-ratio physics; **stampede** on expiry of a hot key; stale data vs. TTL tension; cache-aside vs. write-through as a knob |
| **Object storage** | Cheap durable blobs | Uploading blobs *through* your DB melts it — the Dropbox level's core lesson; latency unsuitable for hot paths |
| **Message queue** | Decouples producers/consumers, absorbs bursts | Consumer lag as a visible backlog; unbounded growth ⇒ delayed side-effects ("emails sent 4 hours late"); teaches async vs. sync trade |
| **Worker pool** | Drains queues, background jobs | Sizing vs. lag; poison messages (later) |
| **CDN** | Serves static/cacheable content at the edge | Without: viral spike hits origin at full force; with: origin sees only cache-miss trickle; invalidation staleness |
| **Rate limiter** | Sheds abusive/excess load early | Without: one scraper starves real users; too strict: real users blocked (visible in rage-quits) |
| **Auth service** | Gates requests | A dependency-for-everything — its saturation takes the whole site down: teaches critical-path thinking |
| **Backup / snapshot** | Point-in-time durability | Costs money, does nothing… until the chaos event. Restore has RPO: you *still* lose the last N minutes — nuance most courses skip |
| **Monitoring** | Reveals internal gauges + alerts | Without it, HUD shows only what users see (errors), not why — the level plays in "fog of war." Cheap, teaches observability by making its absence annoying |
| **Search index** | Serves text queries | `LIKE '%x%'` against the DB is the trap; index lag teaches eventual consistency again in new clothes |

v2 candidates: sharded DB (resharding pain), geo-regions + data residency, websocket gateway (chat levels), feature store, warm standby vs. active-active.

## 4. Traffic profiles (level-defined)

Composable curves: steady-state, diurnal wave, linear growth, viral spike (sharp attack/decay), flash crowd (thundering herd at t=0, e.g. ticket sales), attack traffic (tagged malicious, only rate limiters/WAF discriminate). Read/write mix and payload sizes per scenario — Instagram is read-heavy with big blobs; a chat app is write-heavy with tiny payloads; the difference should be *felt* through which components melt first.

## 5. Chaos events (level-defined)

Instance death, AZ/region outage, disk-full on a stateful node, replication-link cut (partition: choose stale reads or refused writes — CAP made visceral), bad deploy (latency ×10 on one service), cert expiry (self-inflicted total outage, comedic). Each fires at a scripted or seeded time; the design's response is pure physics.

## 6. What we deliberately do NOT simulate (v1)

Packet-level networking, real consensus protocols (Raft elections are a black box: "replica promotes after 30 sim-seconds"), precise vendor pricing, per-language performance. Pedagogical honesty > operational fidelity; every simplification should still point at a *true* mental model.

## 7. Tuning philosophy

Numbers are stylized but ratio-true: a cache is ~100× faster than a DB read; object storage is ~10× cheaper per GB than DB storage; cross-region latency ~10× intra-region. Players should leave with correct *orders of magnitude* — which is exactly the back-of-envelope skill interviews test.
