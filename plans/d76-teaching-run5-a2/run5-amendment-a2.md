# Teaching Audit Amendment A2: Run 5, a Larger Confirmation Run Through the API

**Drafted:** 2026-11-23 · **Status:** **RATIFIED by the user, 2026-11-24 ~10:56 PT (D76); sealed below.** No run-5 lesson has been run. · **Amends:** design §6 (`315c6769…`) and A1.3 (`241e61bf…`), for run 5 onward · **Depends on:** D60 (API harness) and AG1 (auditor agreement; sets the flag rule)

## Why
Run 4 passed (2 of 40 flagged), but the 40-lesson gate cannot tell a good teacher from a poor one:

| Lessons | Pass rule | P(pass) at a 3% true error rate | at 5% | at 10% |
|---|---|---|---|---|
| 40 | ≤ 2 flagged | 0.88 | 0.68 | **0.22** |
| **80** | **≤ 4 flagged** | **0.91** | **0.63** | **0.09** |

At 80 lessons, a teacher with a 3% error rate passes slightly more often, and a teacher with a 10% rate passes about 9% of the time instead of 22%.

## Changes from runs 1–4
1. **Sample: 80 real lessons.** The concept pools are small (4 Contested, 8 other concepts, 17 Disfavored or Commonly held), so the strata cannot simply double. Proposed:

   | Stratum | Real lessons |
   |---|---|
   | Contested concepts | 4 (the whole pool) |
   | Other concepts | 8 (the whole pool) |
   | Disfavored / Commonly held concepts | 12 |
   | Effects | 24 |
   | Papers | 16 |
   | Unsplit records | 16 |
   | **Total** | **80** |

   **Disclosed:**
   - Concepts make up 30% of the sample here, against 40% in runs 1–4.
   - All 12 Contested and other-concept nodes were taught in earlier runs. The teacher has no memory, but v0.4's rules were partly written against errors on such nodes.
2. **Planted controls: 10,** the same types as before (two each of T1, T2, T3, T5 and T6), on nodes outside the real 80.
   - Both T2 controls come from the Disfavored / Commonly held pool, since the other-concept pool is used up.
   - That pool then holds 12 real + 2 T3 + 2 T2 = 16 of its 17 nodes.
3. **Everything runs through the API (D60).**
   - **Teacher:** `claude-opus-5-5` at temperature 0. One request per turn, with the prompt, context, card and transcript. The harness then runs the checker and allows **one** repair request, which mirrors the chat procedure's "fix and re-run once".
   - **Learner:** the same model at temperature 1.0, with the persona rotation unchanged.
   - **Auditors:** 3 passes per batch at temperature 1.0, with lesson order reshuffled per pass (as in AG1).
4. **Harness fixes, all covered by `selftest.py` before sealing:**
   - no field in the auditor table is ever truncated (the run-4 P30 defect);
   - visuals carry `cites`, and the cited items appear in the auditor table (the run-4 P36 gap);
   - a self-test that every figure in a visual can be traced to a shown item.
5. **Flag rule:** fixed by AG1's pre-registered table (majority of 3, or any of 3), plus the mechanical check.
6. **Decision rule:**
   - **Validity:** ≥ 9 of 10 planted errors detected under the chosen flag rule, including T1, T2 and T6.
   - **Pass:** ≤ 4 of 80 real lessons flagged.
   - **On failure:** as before. Fix, rerun on a new seed, and keep the failed run in the record.

## Unchanged
- Error definitions E1–E8 and T1–T6.
- The auditor prompt, apart from D60's mechanical edits.
- The D57 gate before every auditor request.
- Sealing before running, with keys withheld from agents.
- Personas.
- The replacement rule for blocked lessons.

## Cost
Measured by a dry run before sealing. It is not estimated here.

## Harness decisions to ratify with A2 (added 2026-11-23, after the offline harness build)
The harness is in `teach_audit5_harness/`. Its self-test passes 30/30, and the offline dry run ran end to end. The README and `HARNESS.sha256` are in the project under `claude/api/run5/`.

**Items the user decides at ratification:**
1. **Teacher prompt addition.** A short section asking for `cites` on visuals and a JSON reply format. The rest of teacher v0.4 is byte-identical, which is tested. This changes what the teacher is asked to do, so it is logged as a teacher change.
2. **Visual defects count as mechanical flags.** These are a visual with no `cites`, a cite outside the lesson's context, or an untraced figure. They count, rather than being reported only.
3. **Gate hits are pre-approved** through a `--gate-reviewed` file, instead of being reviewed after the fact as in runs 2–4.
4. **Edge rules:**
   - an unparseable reply uses the one repair;
   - two safety refusals block the lesson, and the replacement rule applies;
   - a lesson still open after 7 learner turns is marked incomplete.

**Known limits, disclosed:**
- The figure-trace check cannot catch numbers written as words, or a number that happens to appear in a cited item. A planted "99" was missed in the dry run.
- Correct derived figures, such as sums, can raise false alarms.
- The closing card's 60-character citation cut lives in sealed teacher v0.4 and is left unchanged. The full text appears in the auditor table.
- Requests are sent one at a time, not through the Batches API.
- Cost is unmeasured until a real dry run.

### User decisions on the four harness items (2026-11-23 18:00 PT); to be sealed with A2
1. **Teacher prompt addition: APPROVED.** It is logged as **teacher v0.4.1**: v0.4 plus the visual `cites` and JSON reply section only. Both hashes are recorded at sealing.
2. **Visual defects count as flags: APPROVED.** These are a missing cite, a cite outside the lesson, or an untraced figure. Flags count toward the ≤ 4/80 pass rule.
   - **False-alarm review, fixed now:** an untraced-figure flag is cleared **only** if a script shows the figure to be an arithmetic derivation of traced figures in the same visual (a sum, difference, product, ratio or rounding of them).
   - Every clearance is logged and reported. No other ground for clearing is allowed.
3. **Gate hits: STOP until reviewed: APPROVED.** A D57 hit blocks that auditor request until the hit is listed in the `--gate-reviewed` file, with the reviewer, the date and the reason. Pauses are reported.
4. **Edge rules (a)–(c): APPROVED as drafted.**

### Store review folded in (user decision, 2026-11-24)
- **Run 5 uses teacher tools v0.5.** This is v0.4.1 plus the card claim-line fix from `store-review/issue3_teach_tools.diff`:
  - unsplit records are labelled with the paper's title, and their scope is limited to the listed replications;
  - FReD effects show the tested finding as the claim, with the authors' wording on a separate labelled line;
  - concept and paper cards are unchanged.

  63 of 187 cards change. The card version stamp is bumped to v0.5.
- **Run 5 reads store v0.3** (`phase2/store_v0.3/`; evidence-ID fix, and papers carry no status). So run 5 is the fresh teaching audit that the card change requires.
- **Harness:** before sealing, the harness is pointed at teacher tools v0.5 and store v0.3, and `selftest.py` is re-run.

### Harness pointed at teacher tools v0.5 and store v0.3 (2026-11-24, ~19:00 UTC; offline, no API call)
- **Teacher tools v0.5** installed as `teacher/v05/teach_tools.py`, `c3413bf8…`. Its diff from v0.4 (`6ab3566f…`) is byte-identical to the reviewed `store-review/issue3_teach_tools.diff`. The teacher prompt file is unchanged (`5532bc95…`). The harness still adds the v0.4.1 section in memory, as before.
- **Store v0.3**, `phase2/store_v0.3/store.db`, `883f44bf…`.
- **Harness 0.5.1:** `run5.py` defaults and the two `selftest.py` paths are changed. Nothing else is. `HARNESS.sha256` is `348db5fa…`. The pre-change file is kept as `HARNESS_2026-11-23_prepoint.sha256`.
- **Self-test: 30/30 PASS. Offline dry run (fake transport, the same 6 run-4 rows): completed.** 6 of 6 lessons done, 0 mechanical failures and 0 gate hits. The run manifest records the v0.5 and v0.3 input hashes.
- **What changed in the 6 dry-run contexts, checked by hand:**
  - the unsplit-record card now reads "recorded under the original paper's title; not itself a tested finding", with scope limited to its listed replication;
  - the FReD effect card separates "the tested finding" from the authors' wording;
  - under store v0.3's evidence-ID fix, the effect's second replication is restored, which clears its old tally-mismatch note;
  - concept and paper card bodies are unchanged.
- **Newly visible field (disclosed):** v0.5 puts the store's `credence_basis` text into every lesson context. For example: "paper node: an index of its findings (D29); no status or credence of its own". The teacher and the auditor table now show it. The store text is the same in v0.2 and v0.3; only its visibility is new.
- **Correction to the 2026-11-24 fold-in text above:** "The card version stamp is bumped to v0.5" is wrong. Cards carry no tools-version label. The card's `version` is a hash of the lesson context, so it changes on the nodes whose context changed. The tools version is traced by the teach-tools sha256 that every run manifest records.

## Ratification (user, 2026-11-24, ~10:56 PT; D76)
- **The user's words (question window):** "Ratify A2 (Recommended)".
- **Ratified:** this text, including the four harness decisions (2026-11-23 18:00 PT), the store-review fold-in (teacher tools v0.5, store v0.3), the repoint record and its correction.
- **Options shown:** ratify, or leave as a draft. Recommendation: ratify.
- **Reason:** no reason given. The user chose the recommended option.
- **Pinned at sealing:**

  | Item | sha256 |
  |---|---|
  | harness 0.5.1 `HARNESS.sha256` | `348db5fa52fc3a6ed819005ecbc416331fb7b57555d16705b7e25d2168071466` |
  | teacher tools v0.5 | `c3413bf840e4c0557f358562a9d02b38be500777f05f582791d0f73a9cdc7d52` |
  | teacher prompt v0.4 (file) | `5532bc9565aaf3c7073fa6a0b6da6e4f481134783912f025b572cbe315d481e9` |
  | teacher v0.4.1 (system prompt as sent, rendered by `prompts.teacher_system`; saved as `teacher/v05/teacher_prompt_v0.4.1_rendered.md`) | `89304b73ee9da35c7eadc03df3d39106336d203b086e112bba6c0e3a44cf4289` |
  | store v0.3 | `883f44bf4c0715b08d2142aa75cb37ee9e76993515f4909eb38e08c665af5c34` |

- **Still required before any run-5 lesson (unchanged by ratification):**
  - API credits;
  - the dry run with the real plan and seeds;
  - AG1 ratified and run, since it sets the flag rule (item 5);
  - the sealed sample and planted key held by the key holder;
  - `selftest.py` re-run on the pinned files.

**SEAL:** `30b9559f901400d558d67285f7a990e7f01cff45f337575bf90e8a4d73b3707c` · 2026-11-24
