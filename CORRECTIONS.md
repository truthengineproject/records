# Corrections log

Sealed files are never edited. When we find a mistake in a sealed file, a published result or our own account of it, it is logged here with the date, what was wrong, what is right, and whether it changes any result. Newest first.

| Date | What was wrong | What is right | Changes a result? |
|---|---|---|---|
| 2026-11-25 | Labels inside five sealed plans published in `plans/`: D79, D80 and D82 give ratification times (~11:55, ~12:40, ~13:25 PT) later than their own FreeTSA timestamps; D83's status line says "~14:00 PT"; D89's title says "(draft v2)". | The FreeTSA time is the authority: D79 before 11:48:16, D80 before 12:11:15, D82 before 13:16:32, D83 sealed at 13:47:12 PT (all 24 Nov 2026); D89 was ratified as v2.1. The times were estimates written after the fact; our internal records audit caught them on 24 Nov. Listed in [`plans/README.md`](plans/README.md). | No. The texts themselves are unchanged and their timestamps are correct. |
| 2026-11-25 | Our decision log entry D90 (not a sealed file) said all four OpenTimestamps calendars confirmed PP-1 change 3 in Bitcoin block 977,516. | Two calendars (a.pool, alice) confirmed in block 977,516 and two (b.pool, bob) in block 977,519. Our confirmation record had both heights; the summary sentence was wrong. Proof files: [`examples/pp1-change3/`](examples/pp1-change3/). | No. Both blocks are below the selection block 977,596, so the rule was fixed in time either way. |
