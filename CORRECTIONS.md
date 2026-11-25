# Corrections log

Sealed files are never edited. When we find a mistake in a sealed file, a published result or our own account of it, it is logged here with the date, what was wrong, what is right, and whether it changes any result. Newest first.

| Date | What was wrong | What is right | Changes a result? |
|---|---|---|---|
| 2026-11-25 | Our decision log entry D90 (not a sealed file) said all four OpenTimestamps calendars confirmed PP-1 change 3 in Bitcoin block 977,516. | Two calendars (a.pool, alice) confirmed in block 977,516 and two (b.pool, bob) in block 977,519. Our confirmation record had both heights; the summary sentence was wrong. Proof files: [`examples/pp1-change3/`](examples/pp1-change3/). | No. Both blocks are below the selection block 977,596, so the rule was fixed in time either way. |
