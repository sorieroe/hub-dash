# Open Questions & Next Decisions

Running list of things we (human + Claude) still need to decide. Ordered roughly by how soon they block progress.

1. **Name.** "Loadbound", "Uptime", "Bottleneck", something .io? Needs a domain check before attachment forms.
2. **Old repo.** You mentioned a possible prior attempt on your GitHub — couldn't scan the account from this session (tool needed interactive approval). If it exists, worth mining for ideas before we harden this draft.
3. **New repo creation.** Session integration lacked permission to create repos. Create `sorieroe/system-design-simulator` (or the chosen name) on GitHub, then we lift the `system-design-simulator/` folder from hub-dash into it wholesale.
4. **Vertical-slice level.** Which single level proves the fun fastest? Draft answer: URL shortener launch-day (schema in `levels/`), because it teaches DB-absence, caching, and SPOF in one 10-minute session with a 5-item palette.
5. **Fidelity dial.** How "sim-y" is too sim-y? Playtests must answer whether the queueing physics reads as fun or as homework. Fallback posture: simplify numbers, keep consequences.
6. **AI interviewer: v1 or v2?** It's the differentiator vs. every static course, but it adds per-user inference cost and prompt-engineering scope. Draft position: ship deterministic sim first, add AI interviewer as the flagship paid feature in v2.
7. **Scoring calibration.** Do stars map to interview leveling (junior/senior/staff difficulty tiers)? Tempting for marketing ("staff-level design badge"), risky for credibility.
8. **Content licensing.** Interview questions themselves aren't copyrightable, but course-alike phrasing is a lane to stay out of — our levels should read as original scenarios (fictional companies) that *map* to the classics.
9. **Community levels.** Levels-as-JSON makes a level editor nearly free engineering-wise — but moderation/quality isn't. v2+.
10. **Marketing beachhead.** Daily-challenge share cards vs. YouTube/Shorts of spectacular failures vs. SEO codex pages — pick one to do properly at launch.
11. **Firsthand competitive teardown.** Actually play SysSimulator (syssimulator.com), LeetDesign (leetdesign.com), and Hello Interview's guided practice before building — cheap intel on what a sim feels like *without* game design, and what rubric-grading feels like without a sim. (Research summaries in doc 04 are secondhand.)
12. **Name collision check.** "System design simulator" is already used by at least three sites (.org/.in/.com). The product name must not be descriptive-generic — reinforces question 1.
