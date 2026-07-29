# Raw Research Report: Ground-Truth Content Sources (2026-07-28)

Verbatim output of the solved-question sources research agent. Synthesized into `docs/12-content-pipeline.md`; kept raw for the **full 35-question ranked table, component frequency table, verified licenses, and all source URLs** — the content pipeline and pedagogy judges depend on these. Licenses verified against LICENSE files unless marked otherwise.

---

## 1. Catalog of Authoritative Solved-Question Sources

### Tier A — Openly licensed, directly usable/adaptable

| Source | URL | Coverage | Depth | License / Access | Citability |
|---|---|---|---|---|---|
| **The System Design Primer** (donnemartin) | github.com/donnemartin/system-design-primer | 8 fully solved questions + full concepts curriculum | Full: capacity estimates, diagrams, code, trade-offs | **CC BY 4.0** (verified in LICENSE.txt — an earlier page summary said MIT; the LICENSE.txt fetch confirms CC BY 4.0). ~360k stars | **Best-in-class**: commercial adaptation allowed with attribution |
| **Grokking open companion** (design-gurus/grokking-system-design) | github.com/design-gurus/grokking-system-design | Approach-level walkthroughs for 40+ questions, 12 building-block pattern summaries, cheat sheets | Sketch/framework level, NOT full worked solutions | **CC BY 4.0** (verified). Only 167 stars but published by DesignGurus themselves | Excellent: an official free-and-open version of the Grokking question canon |
| **awesome-scalability** (binhnguyennus) | github.com/binhnguyennus/awesome-scalability | Links-only catalog of real-world architecture case studies, talks, papers | N/A (curation) | MIT (per repo page fetch — medium confidence, verify LICENSE) . ~72.8k stars | Great as an index to primary sources |

The Primer's 8 solved problems: Pastebin/Bit.ly, Twitter timeline & search, web crawler, Mint.com, social-network data structures, KV store for search engine, Amazon sales ranking, scaling to millions of users on AWS.

### Tier B — Free on the web, copyrighted (read/corroborate/cite, don't copy)

| Source | URL | Coverage | Depth | Access |
|---|---|---|---|---|
| **HelloInterview** | hellointerview.com/learn/system-design | **31 problem breakdowns: ~16-18 free, ~13 premium** (free at fetch time: Bitly, Dropbox, Local Delivery (Gopuff), Ticketmaster, FB News Feed, Tinder, LeetCode, WhatsApp, Rate Limiter, YouTube, FB Live Comments, YouTube Top-K, Uber, Web Crawler, Ad Click Aggregator, FB Post Search. Premium: Yelp, Instagram, Strava, Distributed Cache, Online Auction, Job Scheduler, News Aggregator, Price Tracker, Robinhood, Google Docs, Payment System, Metrics Monitoring, Online Chess, ChatGPT) | **The deepest free written solutions on the web**: functional/non-functional reqs, capacity estimation, API design, data model, HLD diagram, multiple deep dives with "good vs great" answers, difficulty tags, company logos per question. Verified via Ticketmaster breakdown: final design = LB, API gateway, microservices, PostgreSQL, Redis (cache + distributed lock), Elasticsearch, CDN, SSE/WebSocket, Stripe | Free web with account; premium tier for locked content. Also free "System Design in a Hurry" crash course + patterns page |
| **codeKarle** (Sandeep Kaul) | codekarle.com (+ YouTube playlist PLhgw50vUymyckXl3D1IlXoVl94wknJfUC) | ~15 written+video solutions: TinyURL, Twitter, Netflix/YouTube, WhatsApp/Messenger, Uber, Zoom, notification system, e-commerce, etc. | Full architecture diagrams + transcript-style writeups | Free web, no license stated (default copyright) |
| **NeetCode** | neetcode.io/courses/system-design-interview | "System Design Interview": How to Approach + ~10 problems (Rate Limiter, TinyURL, Twitter, Discord, YouTube, Google Drive, Google Maps, KV Store, Distributed Message Queue, +1). Plus "System Design for Beginners" | Text + diagrams; reviews rate it beginner-level | Much publicly accessible; Pro for videos |
| **Exponent** | tryexponent.com/blog/system-design-interview-guide | Question lists by category with company attributions (incl. newer AI/LLM questions: LLM serving at Google, OpenAI Playground, batch inference API at Anthropic); free question database at /questions | Guide = list + framework only; solved content in paid course | Freemium |
| **DesignGurus blog** | designgurus.substack.com/p/30-system-design-interview-questions | "30 questions ranked by difficulty" with company attributions | List + commentary | Free web |

### Tier C — Paid, canon-defining (fact-checking references only)

| Source | Coverage | Access |
|---|---|---|
| **Alex Xu, System Design Interview Vol 1** (ByteByteGo) | 16 chapters: Scale to millions, Back-of-envelope, Framework, **Rate Limiter, Consistent Hashing, KV Store, Unique ID Generator, URL Shortener, Web Crawler, Notification System, News Feed, Chat System, Search Autocomplete, YouTube, Google Drive** (13 designs + 3 method chapters), 188 diagrams | Paid book (~$40); same content on bytebytego.com |
| **Alex Xu + Sahn Lam, Vol 2** | 13 designs: **Proximity Service, Nearby Friends, Google Maps, Distributed Message Queue, Metrics Monitoring, Ad Click Aggregation, Hotel Reservation, Distributed Email, S3-like Object Storage, Gaming Leaderboard, Payment System, Digital Wallet, Stock Exchange**, 300+ diagrams | Paid book |
| **bytebytego.com** | The 29 book designs as a course + more; newsletter freemium (~$100-150/yr); full platform ~$499/yr or $999 lifetime. **ByteByteGoHq/system-design-101 repo (~86.5k stars) is CC BY-NC-ND 4.0** — no commercial use, no derivatives | Paid |
| **Grokking the System Design Interview** (DesignGurus, orig. Educative) | 15+ case studies: TinyURL, Pastebin, Instagram, Dropbox, FB Messenger, Twitter, YouTube/Netflix, Typeahead, API Rate Limiter, Twitter Search, Web Crawler, FB Newsfeed, Yelp/Nearby Friends, Uber, Ticketmaster + 20 concept lessons | $148 lifetime / subscription |
| **Grokking the Advanced System Design Interview** | Real-system deep dives: Dynamo, Cassandra, Kafka, Chubby, GFS, HDFS, BigTable (+ Spanner, Raft, MapReduce, ZooKeeper) | Paid |

### Tier D — YouTube walkthrough canons (watch/take factual notes; no transcript republishing)

- **Jordan has no life** (youtube.com/@jordanhasnolife5163) — the deepest free video canon. "Systems Design Interview Questions With Ex-Google SWE" numbered playlist; verified entries: #3 Dropbox+Google Drive, #5 Netflix+YouTube, #8 Web Crawler, #9 Yelp/Google Places, #11 Ticketmaster/StubHub, #12 Google Docs, #17 Top-K Leaderboard, #18 Count Unique Active Users, #22 Recommendation Engine, #31 Distributed Priority Queue; total ~35+ **(exact count unverified)**. Companion notes at jordanhasnolife.substack.com (real-system deep dives: DynamoDB, Facebook Memcache, Meta TAO, Dropbox Magic Pocket, Amazon Aurora, Google Borg, TikTok Monolith).
- **Gaurav Sen** — "System Design" playlist ~30 videos; more conceptual/entertaining than rigorous; paid platform InterviewReady.
- **codeKarle** — videos + free written versions (best of both).
- **HelloInterview's and Exponent's channels** — mock-interview walkthroughs.

### Dataset-style collections

No true "dataset with solutions" exists. Closest:
- **checkcheckzz/system-design-interview** (~23.4k stars): question list + 40+ company blog links, no solutions, no license.
- **shashank88/system_design** (~9k stars): links/prep resources only.
- **karanpratapsingh/system-design** (~44.7k stars): full free course, Chapter V has 5 solved case studies (URL Shortener, WhatsApp, Twitter, Netflix, Uber). **License: CC BY-NC-ND 4.0 (verified)** — NOT usable commercially or in derivative form.
- **ashishps1/awesome-system-design-resources** (~40k stars): links organized easy/medium/hard. **GPL-3.0** (treat as do-not-embed).
- **david8zhang/system-design-notes**: markdown notes compiled from Jordan's videos etc. (derivative, unclear rights).

**Implication:** the only openly-licensed *solution text* legally adaptable into a commercial product is the System Design Primer + DesignGurus' CC BY 4.0 companion. Everything else is *fact-checking corroboration* — which is fine, because architectures are unprotectable ideas/facts (§5).

## 2. Primary-Source Engineering Blogs (real architectures for flavor events)

| Interview question | Canonical primary sources | Flavor-event material |
|---|---|---|
| **Chat (WhatsApp/Discord/Slack)** | HighScalability "The WhatsApp Architecture Facebook Bought For $19 Billion" (Erlang/FreeBSD, ~70M msg/s peak, ~10 engineers); Discord "How Discord Stores Billions of Messages" (2017, MongoDB→Cassandra) and "How Discord Stores Trillions of Messages" (2023 — 177 Cassandra nodes→72 ScyllaDB, Rust data services, p99 read 40-125ms→15ms); Slack "Real-time Messaging" + "Flannel: An Application-Level Edge Cache" (thundering-herd reconnects, 4M simultaneous conns) | Cassandra hot partitions; NYE message spikes; mass-reconnect stampedes |
| **News feed / Instagram / Twitter** | Meta "TAO: The Power of the Graph" (2013) + USENIX ATC '13 paper; Instagram "Sharding & IDs at Instagram" (Postgres logical shards, 64-bit IDs); Raffi Krikorian "Timelines at Scale" (InfoQ) — hybrid push/pull, celebrity fan-out | Justin Bieber like-storms; celebrity fan-out blowing up push model |
| **Dropbox / Google Drive** | dropbox.tech "Inside the Magic Pocket" + "Scaling to exabytes and beyond" (2016 AWS exodus; 4MB blocks, 1GB volumes, erasure coding, SMR drives); "Streaming File Synchronization"; "Rewriting the heart of our sync engine" (Nucleus/Rust, 2020) | The AWS exodus; block-level dedup/delta sync |
| **YouTube / Netflix** | Netflix TechBlog "Distributing Content to Open Connect" + "Content Popularity for Open Connect"; Zuul, EVCache, Chaos Monkey posts; HighScalability "YouTube Architecture" (early Vitess-era MySQL sharding) | Chaos Monkey; ISP-embedded cache appliances; nightly proactive cache fill |
| **Uber / ride sharing** | HighScalability "How Uber Scales Their Real-Time Market Platform" (2015 — DISCO dispatch, driver phones as distributed storage); Uber "H3: Hexagonal Hierarchical Spatial Index"; "Schemaless" series; Ringpop | Surge events; datacenter failover using driver phones |
| **Rate limiter** | Stripe "Scaling your API with rate limiters"; Cloudflare rate-limiting posts *(titles from memory — verify)* | |
| **KV store** | Amazon Dynamo paper (SOSP '07) — the most-cited artifact in the genre; DynamoDB USENIX ATC '22 paper | Shopping-cart availability story |
| **Payments** | Airbnb "Avoiding Double Payments in a Distributed Payments System"; Stripe blog *(titles unverified)* | Idempotency-key war stories |
| **Google Docs** | Google Drive OT posts (2010); Figma "How Figma's Multiplayer Technology Works" *(unverified titles, high confidence)* | |
| **Ticketmaster** | No strong first-party post; use the Nov 2022 Taylor Swift Eras presale meltdown (heavily reported) as flavor | Verified-fan queue collapse |
| **Search / crawler** | Brin & Page "Anatomy of a Large-Scale Hypertextual Web Search Engine"; Facebook "The Life of a Typeahead Query" *(unverified)* | |
| **Caching** | Facebook "Scaling Memcache at Facebook" (NSDI '13) | Thundering herds, leases |
| **Aggregators** | **HighScalability** ("Real-World Architectures"); ByteByteGo newsletter, Quastor/systemdr substacks | |

All engineering-blog *facts* are freely usable; the posts' text is not.

## 3. The Canonical Question List (~35, ranked by cross-source frequency)

Frequency across: Primer (P), Alex Xu V1/V2 (X1/X2), Grokking (G), Grokking-open (GO), HelloInterview (H), NeetCode (N), codeKarle (CK), Jordan (J), Gaurav Sen (GS), DesignGurus-30 (D), Exponent (E), ashishps1 (A). Difficulty = consensus of HelloInterview tags + DesignGurus tiering.

| # | Question | Sources (≈count) | Difficulty | Key components/concepts exercised |
|---|---|---|---|---|
| 1 | **URL shortener / Pastebin** (TinyURL, Bit.ly) | P,X1,G,GO,H,N,CK,J,D,E,A (11) — "the single most universal question" | Easy | Hashing/base62, unique ID gen, KV store, cache, redirect latency, read-heavy scaling |
| 2 | **News feed / Twitter / Instagram** | P,X1,G,H,N,CK,J,GS,D,E,A (11) — "most reported question at Meta" | Medium | Fan-out write vs read, celebrity problem, cache, graph data model, ranking, pagination |
| 3 | **Chat system** (WhatsApp/Messenger/Discord/Slack) | X1,G,H,N,CK,J,GS,D,E,A (10) | Medium | WebSockets, stateful gateways + consistent hashing, ordering, delivery receipts, presence, wide-column store |
| 4 | **Video platform** (YouTube/Netflix) | X1,G,H,N,CK,J,GS,D,E,A (10) | Medium-Hard | Object storage, CDN, transcoding pipeline (queue+workers/DAG), adaptive bitrate, view counts |
| 5 | **Cloud file storage/sync** (Dropbox/Drive) | X1,G,H,N,CK,J,D,E,A (9) | Easy-Medium | Blob storage, presigned URLs, chunking/dedup/delta sync, metadata DB, sync notifications |
| 6 | **Rate limiter** | X1,G,H,N,D,E,A (7) | Medium | Token/sliding-window algorithms, Redis, distributed counters, gateway placement |
| 7 | **Ride sharing** (Uber/Lyft) | G,H,CK,J,GS,D,E,A (8) | Hard | Geospatial index (geohash/H3), real-time location ingest, matching, WebSockets, surge |
| 8 | **Web crawler** | P,X1,G,H,J,D (6) | Hard | Frontier queue, politeness, dedup/Bloom filter, DNS, distributed workers |
| 9 | **Ticket booking** (Ticketmaster) | G,H,J,D,E (5) + Hotel Reservation variant (X2,D) | Medium | ACID/transactions, distributed locking w/ TTL, seat-hold contention, virtual queue, SQL |
| 10 | **Search autocomplete / typeahead** | X1,G,D,GS,A (5) | Medium | Trie, top-k, prefix caching, aggregation pipeline |
| 11 | **Notification system** | X1,CK,D,A,H-premium (5) | Medium | Queues/pub-sub, fan-out, APNs/FCM gateways, retry/idempotency, rate control |
| 12 | **Distributed KV store** | P,X1,N,E,Grokking-Advanced (5) | Hard | Consistent hashing, replication, quorum R/W, vector clocks, gossip, LSM/SSTables |
| 13 | **Proximity service / Yelp** | X2,G,H,J,D (5) | Medium-Hard | Geohash/quadtree, search index, read-heavy cache |
| 14 | **Distributed message queue / Kafka** | X2,N,D,E,Grokking-Advanced (5) | Hard | Append-only log, partitions, consumer groups, replication, offsets, ZooKeeper/KRaft |
| 15 | **Google Maps / navigation** | X2,N,J,D (4) | Hard | Map tiles, routing graph, geospatial sharding, ETA, location streams |
| 16 | **Payment system** (Stripe-like) | X2,H-premium,D,E (4) | Hard | Idempotency keys, ledger, exactly-once, saga/workflow, reconciliation, PSP |
| 17 | **Distributed cache** | D,A,H-premium,Grokking-Modern (4) | Medium | LRU, consistent hashing, write-through/aside, hot keys, replication |
| 18 | **Collaborative editor** (Google Docs) | H-premium,J,D,A (4) | Hard | OT/CRDT, WebSockets, versioning, presence |
| 19 | **Top-K / trending / leaderboard** | X2,H,J×2,checkcheckzz (4) | Medium-Hard | Redis sorted sets, count-min sketch, stream processing, lambda architecture |
| 20 | **Unique ID generator** | X1,D (2 standalone; ubiquitous embedded) | Easy | Snowflake IDs, clock skew, coordination |
| 21 | **Ad click aggregator** | X2,H (2) | Hard | Kafka + stream processing, exactly-once aggregation, OLAP store |
| 22 | **Metrics monitoring / logging** | X2,H-premium (2) | Hard | Time-series DB, pull vs push, downsampling, alerting |
| 23 | **Job scheduler** | H-premium,A,J (3) | Medium-Hard | Priority/delay queues, cron semantics, leader election, idempotency |
| 24 | **Social misc** (Tinder, TikTok, Strava) | H,GS,D,E,A (3-4 each) | Medium | Recommendation, geo, swipe-match queues, feeds |
| 25 | **Stock exchange / Robinhood** | X2,H-premium,J (3) | Hard | Matching engine, in-memory order book, sequencer, event sourcing |
| 26 | **Live comments / streaming** (FB Live, Twitch) | H,A,J (3) | Medium-Hard | SSE/WebSocket fan-out at scale, pub-sub, cell architecture |
| 27 | **Nearby friends** | X2,G (2) | Medium | Ephemeral location, Redis pub-sub, TTL |
| 28 | **S3-like object storage** | X2,D (2) | Hard | Erasure coding, metadata service, placement, durability math |
| 29 | **Hotel reservation** | X2,D (2) | Medium | Inventory, overbooking, concurrency |
| 30 | **Digital wallet** | X2,A (2) | Hard | Distributed transactions, event sourcing |
| 31 | **Search (FB post / Twitter search)** | G,H (2) | Hard | Inverted index, ingestion pipeline, ranking |
| 32 | **Distributed lock / priority queue / coordination** | D (Chubby), J (#31), Grokking-Advanced (3) | Hard | Consensus (Raft/Paxos), leases, fencing tokens |
| 33 | **Email service** | X2 (1) | Hard | — |
| 34 | **Online auction / flash sale** | H-premium + variants (2) | Medium | Contention, queues, inventory |
| 35 | **ChatGPT/LLM serving** (emerging 2024-26) | H-premium, E (2, growing) | Hard | GPU queuing, streaming tokens (SSE), stateful sessions |

Note: no source publishes true asked-frequency data; corroboration count across prep canons is the standard proxy. DesignGurus claims companies "draw from the same pool of 25-30 classic problems."

## 4. Component Frequency (drives palette + unlock order)

Derived from ~90 solved designs. Percentages are **analytical estimates, not formal counts**.

| Component | ≈% of solutions | Notes |
|---|---|---|
| Load balancer | ~95-100% | Universal; L4 vs L7 in deeper solutions |
| App servers / microservices | ~100% | Universal |
| Cache (Redis/Memcached) | ~85% | Cache-aside dominant; Redis the default named tech (also locks, geo, sorted sets, pub-sub) |
| Sharding/partitioning | ~70% | Partition-key choice is the #1 "scaling writes" move |
| SQL DB (Postgres/MySQL) | ~65% | HelloInterview defaults to Postgres |
| API gateway | ~60% (near-100% in HelloInterview) | Folds rate limiting/auth |
| Message queue (Kafka/SQS/RabbitMQ) | ~60% | Async work, fan-out, buffering |
| Replication (leader-follower, quorum) | ~60% | |
| NoSQL — KV/document | ~50% | |
| Object storage (S3) | ~40% | Presigned-URL upload pattern in all modern solutions |
| CDN | ~40% | |
| Rate limiter (as component) | ~40% | |
| Consistent hashing | ~35% explicit | Underlies cache/KV/stateful-WebSocket routing |
| Search index (Elasticsearch) | ~30% | |
| WebSocket/SSE realtime gateway | ~30% | |
| NoSQL — wide-column (Cassandra/ScyllaDB) | ~25% | Chat messages, time-series-ish |
| Stream processing (Flink/Spark) | ~20% | Ad clicks, top-k, metrics, trending |
| Unique ID generator (Snowflake) | ~20% | |
| Geospatial index | ~15% | Uber, Yelp, nearby friends, Tinder, Strava |
| Distributed lock | ~15% | Booking, auctions, schedulers, flash sales |
| Coordination service (ZooKeeper/etcd) | ~12% | |
| Bloom filter / probabilistic DS | ~10% | Crawler dedup, cache penetration, top-k |
| Workflow engine / saga | ~8% | Payments, orders |
| Time-series DB | ~5% | Metrics |
| CRDT/OT engine | ~3% | Docs only |
| Graph DB | <5% | **Notably rare — sources treat "use a graph DB" as a trap answer** |
| Matching engine / order book | ~3% | Exchange only |

**Suggested unlock order** (frequency × pedagogy; matches HelloInterview's 8 published patterns): LB + app server + SQL → cache → replication → sharding → object storage + CDN → queue + workers → NoSQL variants → WebSocket + consistent hashing → search index → locks → stream processing → geo → coordination/consensus → specialists.

## 5. Legal / Copyright Posture

**Safe (US law):**
- **Facts and ideas are not copyrightable.** 17 U.S.C. §102(b); *Feist v. Rural* (499 U.S. 340, 1991): facts may be "copied at will." Reference architectures, component choices, capacity-math methods, and trade-off reasoning are unprotectable ideas.
- Corroborating level solutions against Xu/HelloInterview/Grokking and independently re-expressing the same architecture in your own scenario text, diagrams, and rubric is standard and safe — every prep competitor already does this to each other.

**Not safe:**
- Copying/closely paraphrasing solution *text*; reproducing distinctive diagrams; scraping paywalled content (ToS breach + copyright + CFAA-adjacent risk).
- **YouTube transcripts:** ToS prohibits scraping; republishing transcripts or bulk commercial scraping is a clear violation. Watching videos and taking factual architecture notes is fine.

**Verified licenses:**
| Work | License | Commercial adaptation? |
|---|---|---|
| donnemartin/system-design-primer | **CC BY 4.0** (verified) | Yes, with attribution |
| design-gurus/grokking-system-design | **CC BY 4.0** (verified) | Yes, with attribution |
| karanpratapsingh/system-design | **CC BY-NC-ND 4.0** (verified) | **No** (facts still usable) |
| ByteByteGoHq/system-design-101 | CC BY-NC-ND 4.0 (per repo page) | **No** |
| ashishps1/awesome-system-design-resources | GPL-3.0 | Avoid embedding |
| binhnguyennus/awesome-scalability | MIT (verify) | Yes (links list) |
| checkcheckzz/system-design-interview | none stated | Default all-rights-reserved; links usable |
| Alex Xu books, Grokking, HelloInterview, codeKarle, YouTube | Full copyright | Facts/ideas only; cite, don't copy |

**Trademark angle:** prep sites use real names under nominative fair use; a commercial *game* shipping branded levels is riskier. Standard practice: **original fictional scenarios transparently mapping to the classics** ("StubCrush" ≈ Ticketmaster, "BoxDrop" ≈ Dropbox), real companies only in factual flavor text, plus a "based on the classic 'Design X' interview question" citation line (doubles as credibility signaling).

**Recommended pipeline posture:** (1) draft each level from ≥3 independent sources; (2) store only your own normalized structured representation (components, edges, constraints, rubric) — facts, not expression; (3) attribute CC BY sources; (4) link out to sources in an in-game "learn more" panel.

## Sources

**Solved-question canons:** [System Design Primer](https://github.com/donnemartin/system-design-primer) ([LICENSE](https://github.com/donnemartin/system-design-primer/blob/master/LICENSE.txt)) · [HelloInterview learn hub](https://www.hellointerview.com/learn/system-design) · [HelloInterview breakdowns](https://www.hellointerview.com/learn/system-design/problem-breakdowns/overview) · [HelloInterview Ticketmaster](https://www.hellointerview.com/learn/system-design/problem-breakdowns/ticketmaster) · [HelloInterview patterns](https://www.hellointerview.com/learn/system-design/in-a-hurry/patterns) · [ByteByteGo course](https://bytebytego.com/courses/system-design-interview) · [ByteByteGo newsletter](https://blog.bytebytego.com/about) · [system-design-101 repo](https://github.com/ByteByteGoHq/system-design-101) · [Grokking (DesignGurus)](https://www.designgurus.io/course/grokking-the-system-design-interview) · [Grokking open companion](https://github.com/design-gurus/grokking-system-design) · [Grokking Advanced](https://www.designgurus.io/course/grokking-the-advanced-system-design-interview) · [Educative Grokking Modern](https://www.educative.io/courses/grokking-the-system-design-interview) · [NeetCode course](https://neetcode.io/courses/system-design-interview) · [Exponent guide](https://www.tryexponent.com/blog/system-design-interview-guide) · [Exponent course](https://www.tryexponent.com/courses/system-design-interviews) · [codeKarle](https://codekarle.com/) · [Jordan has no life](https://www.youtube.com/@jordanhasnolife5163) · [Jordan Substack](https://jordanhasnolife.substack.com/archive) · [Gaurav Sen playlist](https://www.youtube.com/playlist?list=PLSLKQmDu5k9Rx3RRfMzs-AZuNPl2zZqVs) · [DesignGurus 30 ranked](https://designgurus.substack.com/p/30-system-design-interview-questions)

**Repos/collections:** [ashishps1/awesome-system-design-resources](https://github.com/ashishps1/awesome-system-design-resources) · [binhnguyennus/awesome-scalability](https://github.com/binhnguyennus/awesome-scalability) · [checkcheckzz/system-design-interview](https://github.com/checkcheckzz/system-design-interview) · [karanpratapsingh/system-design](https://github.com/karanpratapsingh/system-design) ([LICENSE](https://github.com/karanpratapsingh/system-design/blob/main/LICENSE)) · [shashank88/system_design](https://github.com/shashank88/system_design) · [david8zhang/system-design-notes](https://github.com/david8zhang/system-design-notes)

**Primary engineering sources:** [Discord: Trillions of Messages](https://discord.com/blog/how-discord-stores-trillions-of-messages) · [Dropbox: Inside the Magic Pocket](https://dropbox.tech/infrastructure/inside-the-magic-pocket) · [Dropbox: Scaling to exabytes](https://dropbox.tech/infrastructure/magic-pocket-infrastructure) · [Slack: Flannel](https://slack.engineering/flannel-an-application-level-edge-cache-to-make-slack-scale/) · [Slack: Real-time Messaging](https://slack.engineering/real-time-messaging/) · [Instagram: Sharding & IDs](https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c) · [Meta: TAO](https://engineering.fb.com/2013/06/25/core-infra/tao-the-power-of-the-graph/) · [HighScalability: WhatsApp](https://highscalability.com/the-whatsapp-architecture-facebook-bought-for-19-billion/) · [HighScalability: Uber](https://highscalability.com/how-uber-scales-their-real-time-market-platform/) · [Uber: H3](https://www.uber.com/us/en/blog/h3/) · [Uber: Schemaless](https://www.uber.com/mx/en/blog/schemaless-part-two-architecture) · [Netflix: Open Connect](https://netflixtechblog.com/distributing-content-to-open-connect-3e3e391d4dc9)

**Legal:** [Feist v. Rural (Cornell LII)](https://www.law.cornell.edu/supremecourt/text/499/340) · [Idea-expression dichotomy / §102(b)](https://pressbooks.uiowa.edu/intro-ip/part/the-idea-expression-dichotomy/) · [scrapeops.io/websites/youtube](https://scrapeops.io/websites/youtube/) · [skipthewatch transcript-API guide](https://skipthewatch.com/blog/youtube-transcript-api-guide)

**Flagged unverified:** Jordan playlist total; awesome-scalability MIT license; some canonical post titles (Stripe rate limiter, Figma multiplayer, FB typeahead) cited from memory; component percentages are analytical estimates.
