# Content Pipeline: Ground Truth for Every Level (researched draft)

Researched 2026-07. Licenses verified against LICENSE files; full source list in the research report. This is how we guarantee "we never ship an incorrect solution."

## The source hierarchy

**Tier A — openly licensed, legally adaptable (the backbone):**
- **The System Design Primer** (donnemartin, ~360k stars) — **CC BY 4.0 verified**: 8 fully solved questions with capacity math, plus the entire concepts curriculum. Commercial adaptation allowed with attribution.
- **DesignGurus' open Grokking companion** (design-gurus/grokking-system-design) — **CC BY 4.0 verified**: approach-level walkthroughs of 40+ questions and the 12 building-block patterns, published by DesignGurus themselves.

**Tier B — free web, copyrighted (corroborate + cite, never copy):** HelloInterview's ~16 free problem breakdowns (the deepest free written solutions anywhere: requirements → capacity → API → data model → deep dives with difficulty tags), codeKarle's ~15 written solutions, NeetCode's ~10, Exponent's guides.

**Tier C — paid canon (fact-check references):** Alex Xu Vol 1+2 (26 designs — the de facto interview canon), Grokking paid courses. We own copies, verify against them, copy nothing.

**Tier D — video walkthroughs (watch, take factual notes; never republish transcripts):** Jordan has no life (~35 ex-Google deep dives — the deepest free video canon), Gaurav Sen, codeKarle.

**Primary sources (flavor + authority):** the real engineering-blog posts per question — Discord's trillions-of-messages migration, Dropbox Magic Pocket, Meta TAO, Slack Flannel, Uber H3, WhatsApp-on-Erlang, Scaling Memcache at Facebook. Facts from these ground the chaos events ("in 2022 a major ticketing platform's presale collapsed…") and fill the codex's "how it really happened" panels.

**Licensing traps confirmed:** ByteByteGo's system-design-101 repo and karanpratapsingh/system-design are **CC BY-NC-ND** — no commercial use, no derivatives. Facts inside them remain usable; their text/diagrams do not.

## Legal posture (researched, load-bearing)

- **Architectures are facts/ideas — not copyrightable** (17 U.S.C. §102(b); *Feist v. Rural*). "Fan-out-on-write breaks at celebrity scale, go hybrid" is unprotectable knowledge wherever you learned it. Every prep competitor already sells solutions to the same ~30 questions on this basis.
- **Protected**: solution prose, close paraphrase, distinctive diagrams (Alex Xu's illustrations), paywalled content (never scrape), YouTube transcripts (ToS + copyright — factual notes only).
- **Trademarks**: levels use **original fictional brands that transparently map to the classics** — "StubCrush" ≈ Ticketmaster, "BoxDrop" ≈ Dropbox — with a "based on the classic 'Design X' interview question" citation line (safe *and* credibility-signaling). Real company names appear only in factual flavor text.

## The canonical question list (level backlog, ranked by cross-source frequency)

Corroboration counted across 12 prep canons (~90 solved designs). Top of the table:

| Rank | Question | Freq | Difficulty | Core lessons |
|---|---|---|---|---|
| 1 | URL shortener / Pastebin | 11/12 | Easy | hashing, KV store, cache, read-heavy scaling |
| 2 | News feed (Twitter/Instagram) | 11/12 | Medium | fan-out write vs read, celebrity problem, ranking |
| 3 | Chat (WhatsApp/Discord) | 10/12 | Medium | WebSockets, ordering, presence, wide-column store |
| 4 | Video (YouTube/Netflix) | 10/12 | Med-Hard | blob storage, CDN, transcoding pipeline |
| 5 | File sync (Dropbox/Drive) | 9/12 | Easy-Med | chunking, dedup, presigned URLs, metadata DB |
| 6 | Ride sharing (Uber) | 8/12 | Hard | geospatial index, realtime ingest, matching |
| 7 | Rate limiter | 7/12 | Medium | token bucket, distributed counters |
| 8 | Web crawler | 6/12 | Hard | frontier queue, politeness, Bloom filters |
| 9 | Ticket booking (Ticketmaster) | 5/12 | Medium | contention, locking, virtual queue, ACID |
| 10 | Typeahead / autocomplete | 5/12 | Medium | trie, top-k, prefix caching |

…continuing through ~35 (notification system, KV store, proximity/Yelp, message queue, Maps, payments, distributed cache, Google Docs, top-k/leaderboard, ad aggregator, job scheduler, stock exchange, live comments, S3-clone, LLM serving — the 2024-26 emerging question — etc.). Full ranked table with per-question component lists lives in the research report; it becomes the `levels/` backlog with fictional skins.

## Component frequency → palette & unlock order

Across ~90 solved designs (analytical estimates): LB and app servers ~100%, cache ~85%, sharding discussion ~70%, SQL ~65%, replication ~60%, message queue ~60%, API gateway ~60%, KV/document store ~50%, object storage + CDN + rate limiter ~40% each, consistent hashing ~35%, search index + WebSocket gateway ~30% each, stream processing ~20%, geo index ~15%, locks/coordination ~15%, probabilistic structures ~10%, CRDT/matching engines <5%. (Notable trap the sources agree on: graph DBs are almost never the answer — that's a *lesson*, and validates doc 03's palette almost exactly.)

**Unlock order** (frequency × pedagogy — and it matches HelloInterview's published pattern set): LB + app server + SQL → cache → replication → sharding → object storage + CDN → queue + workers → NoSQL variants → WebSocket + consistent hashing → search index → locks → stream processing → geo → consensus → specialists.

## The authoring workflow (per level)

1. **Draft** from ≥3 independent sources (e.g., Xu chapter + HelloInterview breakdown + Jordan notes) + the primary engineering blog for flavor.
2. **Normalize**: store only our own structured representation — components, edges, constraints, traffic profile, rubric, twist events. Facts, not expression.
3. **Skin**: fictional brand, original scenario text, citation line + "learn more" out-links (linking is unrestricted and reads as credibility).
4. **Validate**: headless sim proves the reference architecture passes its own level and that documented anti-patterns fail it the *documented* way (fixtures per doc 11 §8).
5. **Judge**: pedagogy judge checks every debrief claim against provenance (doc 14); provenance JSON ships in the level record (doc 10 `levels.provenance`).
6. **Attribute**: CC BY sources credited in-app.

## PoC level picks (E6)

1. **"Shortly"** — URL shortener launch day (drafted; the #1 most universal question).
2. **"PasteHub"** — Pastebin with expiring pastes and a hot-link spike (Primer has this fully solved under CC BY; introduces object-vs-DB storage and TTL cleanup).
3. **"BoxDrop"** — Dropbox-lite file upload/sync (HelloInterview rates it Easy; introduces object storage, presigned uploads, metadata split — and its "upload blobs through your DB" failure is the game's single best teaching moment).

Together they cover chapter 1–2 concepts (storage, caching, SPOF, blobs) with three distinct traffic shapes — the minimum honest proof that the engine, the curriculum, and the fun coexist.
