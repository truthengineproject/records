# AG1: Auditor-Agreement Study, Pre-registration v1.3

**Drafted:** 2026-11-23 (v1); **v1.1:** 2026-11-24, Architect, aligning the text with the built runner and with ratified A2 (D76). **v1.2:** the same day, applying the Profile Auditor's audit of v1.1 ("revise": 2 BLOCKING, 4 MAJOR, 8 MINOR; `ag1-prereg-v1.1-audit.md`). **v1.3:** applies the v1.2 re-audit ("ratify with listed amendments": N1 BLOCKING, the wrong rule number; N2–N6). · **Status:** **RATIFIED by the user, 2026-11-24 ~11:55 PT (D79); sealed below, before any API call.** · **Runs under:** D60 (API harness) and D76 (A2) · **Feeds:** run 5's flag rule (A2 item 5).

## Changes from v1
- **Prompt edits:** v1 said "two mechanical edits". The runner applies the **same four edits as run 5** (`prompts.auditor_system`, `a68ba399…`):
  1. the batch is given inline;
  2. the file-read and tool line is removed;
  3. a JSON array is returned in the reply;
  4. the "done" line is removed.

  The self-test checks that each edit applies exactly once.
- **D57 gate hits:** a hit stops a real request until it is reviewed, which is A2 decision 3. The dry run found 21 hits, 7 distinct (see §Gate review).
- **Defined now, where v1 was silent:**
  - invalid replies;
  - the order of precedence in the decision table;
  - how κ and its interval are computed;
  - how "under its intended type" is scored;
  - where the run happens.
- The runner and scorer `ag1/ag1.py` (`5c66e99d…`) are named and pinned.

## Question
How often do independent auditor passes agree on whether a teaching lesson contains an error? And does a single auditor pass catch planted errors reliably enough for a majority-of-3 rule to be safe?

## Data (fixed; no new lessons)
- **Packets:** the exact rendered packets of teaching runs 3 and 4 (`teach_audit3/audit/batch1–5.txt`, `teach_audit4/audit/batch1–5.txt`). That is 10 batches and 100 lessons: 80 real and 20 planted, 10 lessons per batch. They are used as audited, including run 3's known table defects.
- **Prompts:** each run's sealed `AUDITOR_PROMPT.md` (run 3 `67198faa…`, run 4 `9d95714c…`), with the four edits above.

## Procedure
1. **Requests:** 30 in all (2 runs × 5 batches × 3 passes), sent as **one Message Batches request** through `te_api.py` (`8807790d…`).
   - **Model** `claude-opus-5-5`, pinned; a response from another model stops scoring.
   - **Temperature 1.0,** a deliberate deviation from D60's default of 0. **max_tokens 8000.**
   - **The key is given explicitly** (`--key-file`), with no default path or environment fallback, as in run 5. **The run happens where the key file is.**
2. **Order:** for each run and each pass p, a fresh `random.Random(20261130 + p)` reshuffles batches 1–5 in turn. So run 3 and run 4 batches get the same shuffle pattern in a given pass, as in `run5.py`. Lesson blocks are split at their `=== LESSON` headers. Their content is unchanged; blank lines between lessons are normalised, and each block ends with one newline.
3. **D57 gate:** it runs on every request, on a mirror with the system prompt plus one text field per lesson. The re-rendered gated text must equal the text sent.
   - **Real mode:** a request with any hit not listed in the reviewed-hits file is not sent.
   - **Every reviewed entry must carry** the signature, the reviewer, the date (YYYY-MM-DD) and the reason, or the file is refused. The file's sha256 goes into the manifest.
   - **At submit,** the rebuilt requests must equal the prepared request hashes, and the runner must equal the hash recorded at preparation. Otherwise nothing is sent.
4. **Dry run first:** `prepare` (dry) plus `submit --dry` check the request count and the token estimate. Measured on 2026-11-24: 30 requests, about **859k input tokens** (chars/4).
5. **Invalid replies:**
   - **Valid** means a JSON array covering every lesson of its batch exactly once, each with a boolean `error` and a `types` list. Types are matched after removing spaces and upper-casing, so " t2 " counts as T2.
   - **An invalid reply is re-requested once.** Only the marked pass-batches are re-sent (`submit --rerequests`). Attempts are counted per answer received, and collecting twice changes nothing.
   - **A second invalid reply** makes that pass of that batch **missing**.
   - **API failures** (errored, expired or cancelled results) are re-sent up to twice and do not use the re-request. A third failure is missing.
   - **AG1 runs once.** A second full submit is refused. A repeat happens only under **rule 1** of the decision table (κ < 0.4), or after a void.
   - **Void:** if any reply carries a model string other than exactly `claude-opus-5-5`, or none, AG1 stops and is void, and scoring is refused. It is repeated once in full, **in a new run directory**, and the void is logged. **A second void makes AG1 inconclusive**, so run 5 uses any of 3.
   - **Scoring is refused** while any pass-batch is still waiting to be re-sent. A re-send is refused while an earlier batch is uncollected. At submit, `te_api.py`, `prompts.py`, `gate.py`, the firewall and the runner must all equal their prepared hashes.
6. **Logs:** every request and response is logged under D60 and archived with hashes. **Planted keys are read only at scoring,** by the key holder.

## Measures
- **A1. Agreement on real lessons:** Fleiss' κ across the 3 passes on the binary flag, computed on the real lessons that have all 3 passes. **The decision uses the point estimate.**
  - The interval is a 95% percentile bootstrap over lessons: 2,000 resamples, seed 20261201.
  - The number of resamples where κ is undefined is reported. If more than 5% are undefined, no interval is given.
  - The proportion of specific positive agreement is also reported.
- **A2. Single-pass sensitivity:** the share of planted lesson passes in which the auditor flags an error **and lists the intended type** (the plan's `planted-T*` stratum), out of 60, less any missing.
  - **Also reported:** sensitivity by type, and how many of the 20 planted lessons are caught by any of 3 and by majority. A2's validity rule names T1, T2 and T6.
  - **Disclosed:** requiring the intended type is stricter than run 5's validity count.
  - **Key check:** before sealing, the key holder runs `ag1.py keycheck`. It asserts that every packet lesson is in the key, that each run has exactly 10 planted lessons, and that every planted label is `planted-T1`…`T6`. It prints only PASS or FAIL. **Result: PASS (2026-11-24).**
- **A3:** the flag rate on real lessons, per pass.
- **A4, descriptive only:** agreement between the API majority and the original sealed single-auditor verdict. **Sealed verdicts of runs 3 and 4 do not change.**

## Pre-registered decisions for run 5's flag rule (applied in this order of precedence)
0. **More than 3 of the 30 pass-batches missing, or no planted pass scored, or a planted lesson with no scored pass:** **AG1 is inconclusive**, and run 5 uses **any of 3** (sensitivity first).
1. **κ < 0.4, or κ undefined:** **the auditor is unreliable.** Run 5 is not sealed until the auditor prompt is revised and AG1 is repeated. κ is undefined when every scored lesson gets the same flag on all passes; then agreement on errors cannot be measured.
2. **A2 < 90%:** **any of 3.**
3. **A2 ≥ 90% and κ ≥ 0.6:** **majority of 3.**
4. **A2 ≥ 90% and 0.4 ≤ κ < 0.6:** majority of 3, with any of 3 reported alongside. Run 5 is **judged** under majority of 3.

The mechanical check counts as a flag under every rule, as in runs 1–4.

## Gate review (before the real run)
The dry run listed **21 hits, 7 distinct**, all in learner and teacher text or in cited study fields, by lesson:
- run 3 N08: a dictator-game payoff (cited study);
- run 3 N23: a meta-analysis title;
- run 3 N23: a methods limit;
- run 3 N15: experimental incentives (teacher);
- run 4 P17: a learner's remark about lab games;
- run 4 P46: a finding sentence;
- run 4 P33: the same dictator-game payoff as N08.

The list is in `ag1/dry/gate_hits_for_review.json`. **Reviewed:** the user approved all 7 on 2026-11-24 (`ag1/gate_reviewed.json`, `24e1529b…`). `prepare --real` accepts the file.

## Limits (stated in advance)
- **κ with few flags:** about 4–10 of 80 real lessons are expected to be flagged, so κ has a wide interval. It is reported with its interval.
- **Same model family:** all passes use one model, so this measures **within-model consistency, not independence**.
- **A2's denominator** is 20 planted lessons × 3 passes, so 54/60 is the 90% bar. Missing passes lower the denominator, and the bar stays 90% of what exists.
- **Disclosed difference from run 5:** `run5.py` does not re-request an invalid auditor reply. It counts that pass as not flagging, and it does not check that `error` is a boolean. AG1 re-requests once. The difference only affects how failures are handled, and it is reported with run 5.
- **Gate review gap, shared with run 5:** run 5's `gate.py` also accepts bare signatures. Run 5's reviewed-hits file is held to the same fields by the operator checklist.
- **Known limit (changed-lines check C1):** the self-test does not cover the re-send guard or the helper-hash guard at submit. Both were checked by hand by the auditor (refused as specified). Adding the tests would change the runner hash, so the gap is recorded instead.

## Ratification (user, 2026-11-24, ~11:55 PT; D79)
- **The user's words (question window):** "Ratify AG1 (Recommended)".
- **Options shown:** ratify, or leave as a draft until the credits arrive. Recommendation: ratify.
- **Reason:** no reason given. The user chose the recommended option.
- **Audit path:** v1.1 "revise" (2 BLOCKING, 4 MAJOR, 8 MINOR); v1.2 re-audit "ratify with listed amendments" (N1 BLOCKING); v1.3 changed-lines check "ratify" (0, 0, 1 MINOR).
- **Pinned at sealing:**

  | Item | sha256 |
  |---|---|
  | runner `ag1/ag1.py` | `5c66e99d…` |
  | reviewed hits `ag1/gate_reviewed.json` | `24e1529b…` |
  | `te_api.py` | `8807790d…` |
  | `prompts.py` | `a68ba399…` |
  | `gate.py` | `b325eb19…` |
  | run 3 auditor prompt | `67198faa…` |
  | run 4 auditor prompt | `9d95714c…` |

- **Before the first API call:**
  - API credits;
  - `prepare <run> --gate-reviewed gate_reviewed.json --real`;
  - `submit --key-file` on the machine that holds the key.

**SEAL:** `fa00646bcf3c1782eb9faafbb2384d96e739b1ecfa13cfe2f2766c5e0e8ff8da` · 2026-11-24
