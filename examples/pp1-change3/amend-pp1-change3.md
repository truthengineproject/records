# M1 pilot procedures, PP-1 change 3: the AFP query in T2

**Status:** **RATIFIED by the user, 2026-11-25 (D90)**: "Ratify and seal now (Recommended)". Architect. Amends PP-1 (D75) for T2 only. v2 applied the check (`amend-pp1-change3-check.md`: "ratify with listed amendments", 0 BLOCKING, 4 MAJOR, 4 MINOR; no further audit needed once applied). RS brief v5.2 carries the step; `BRIEFS.sha256` `6334589c…`.

## 1. The problem
The MA7 source rehearsal (2026-11-25, `mathaudit/ma7_rehearsal/REHEARSAL.md`) found that the AFP search page T2 lists (`https://www.isa-afp.org/search/?s=<title>`, in `regsearch.py`) builds its results in the browser with JavaScript. A session that fetches it gets the page shell with **no results**, whatever the query. A T2 AFP check could therefore record "no touch" without searching. PP-1 recorded AFP as reachable ("answer"), which is true of the page, not of its results.

## 2. The change
1. **The T2 AFP query is run against the AFP entry index** (`https://www.isa-afp.org/entries/index.json`). That is the same data the search page searches: every entry's title, abstract, authors and topics (1,061 entries on 2026-11-25).
2. **Procedure:** by the Registration Searcher, with `procedures_pp1c3/afp_index_search.py`, **sha256 `694a227edc0dd72a60a7cd2d8c24d3c10eee2cb8d3bdac6dc89e71d22bc1aa8f`** (listed in `procedures_pp1c3/PP1C3.sha256`; self-test PASS):
   - `fetch` the index once per screening session and once per posting session; its file and sha256 are logged with the session;
   - `search` it for each candidate with the same keys as T1: the title, the DOI and the arXiv id if any. An entry is listed if the DOI or arXiv id appears in any field (including `related`), or if **at least half of the title's distinctive tokens** appear among the entry's title and abstract tokens; a title with fewer than two distinctive tokens uses its non-function words, **all** of which must appear. Markup is stripped and accents and special letters are folded on both sides. The rule is fixed in the script;
   - **the RS reads every listed entry** and decides TOUCH or NO TOUCH, as for any T2 hit. The script never judges.
3. **Logging (so `regsearch.py evaluate` counts the query as done):**
   - the fetched index goes in `custody/regsearch/afp/`, with the `fetch` output line (path, sha256, entries, final URL, HTTP status);
   - the search output (JSON) goes in `custody/regsearch/afp/<candidate_id>_<phase>.json`, hashed;
   - the T2 log entry has **`planned` = the AFP URL exactly as `regsearch.py plan` prints it for that candidate**, `executed` = `T2:AFP via entry index, PP-1 change 3`, the index sha256, the search output's sha256, and **`touch_found` = true if any listed entry is judged a touch**, otherwise false;
   - the same applies at the screen and at posting.

   The planned query in `regsearch.py` stays as written; this procedure executes it.
4. **If the index cannot be fetched:** the AFP query has not been run. It is retried once at least an hour later (PP-1 change 1 §2.4), then becomes a user-run query under rules already ratified (RS brief step 4; D81: 7 days, then SKIP at step 3). The user saves the **rendered** results page (after the results appear), not the bare page.

**If this change is void** (attested at or above h\*), the same already-ratified route applies to every AFP query: one user-run AFP search per candidate at the screen, and one per mission at posting.

## 3. What does not change
T1, T3, the other T2 queries, every other step, the scripts already sealed (`PROCEDURES.sha256` `027f9f82…` is unchanged), and the touch rule itself.

## 4. Disclosed
- **Compared with the search page:** the page needs every title word in one field and shows at most 16 entries; this rule needs half of the distinctive words (all of them for short titles) and has no cap, and its DOI match also covers the `related` field. It is looser on long titles; every listed entry is read, so that costs only reading time.
- **No stemming or spelling variants** beyond prefix matching ("graph" matches "graphs"; "-isation" does not match "-ization"). The page behaves the same way.
- AFP is Isabelle, not Lean. A touch there is still a touch under C-2 ("formalized elsewhere").

**SEAL (convention b):** sha256 of everything above this line: `2431aabf825091be11b7f3bf7223c4c0f25455ad1a8b91270f7436c8c53c5eff` · 2026-11-25
