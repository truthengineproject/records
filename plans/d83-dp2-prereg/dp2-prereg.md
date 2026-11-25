# Discovery Pilot DP2: Many Concepts, One Pending Registered Report Each (pre-registration, v2)

**Status:** v2, 2026-11-24 ~14:00 PT. Architect. It applies the audit (`phase6/dp2-prereg-audit.md`: "ratify with listed amendments", 3 BLOCKING, 10 MAJOR, 8 MINOR), and its diff check (2 BLOCKING, 6 MAJOR, 5 MINOR; N1–N8, n1–n5). §10 maps the findings.
- **RATIFIED by the user, 2026-11-24 (D83)**: "Ratify now (Recommended)". Seal 1 below, with FreeTSA and OpenTimestamps, before any agent runs.
- **Decision basis:** D49 (Phase 6 pilot). The DP1 record: FAILED on form (attempt 1); attempt 2 not run for lack of targets (`claude/discovery-pilot-dp1-screen-results.md`).
- **Direction:** chosen by the user, 2026-11-24 ~13:25 PT: "1: many concepts (Recommended)".
- **Does not change:** DP1's 4 sealed predictions (`e2627f95…`, all on Warne & Larsen), which stay live and are scored separately. The Phase 6 scaling gate: full Phase 6 still waits until the store's statuses are shown to predict (D49).

## 1. Question
Under the D49 guardrails, can a Scientist Agent produce **sharpened, non-rescue hypotheses** that it turns into **advance predictions of pre-registered primary tests in pending Registered Reports**, across many different claims, and do those predictions beat simple baselines when they resolve?
- With about a dozen predictions, the pilot is mainly about **feasibility and form**. Predictive skill is reported **descriptively only**.

## 2. Why this design (DP1's finding)
- DP1 failed on form, and its second attempt found 0 usable targets. Under the strict screen (A1.3 (a)–(c)), **no general claim had 3 or more pending Stage 1 Registered Reports (RRs)**.
- But the same survey found **138 pending Stage 1 RRs** at PCI RR, which come with design tables stating each hypothesis and its decision rule.
- **DP2 keeps the strict screen** and drops the "one concept" constraint: **one pending RR per claim, many claims**.

## 3. Target pool, screen and assignment (fixed now)
**Two seals (m4):**
- **Seal 1** is this text, ratified and timestamped before any agent runs.
- **Seal 2** is the predictions record, hashed and timestamped with FreeTSA and OpenTimestamps before any resolution.

1. **Survey (M1):**
   - **Scope:** **PCI RR only.** Every Stage 1 recommendation with in-principle acceptance and no linked Stage 2. Other journals are excluded, to keep the list complete and checkable.
   - **Who:** a fresh agent, on the survey date.
   - **Output:** a list sorted by PCI RR record id, ascending, and hashed.
   - **Exclusions,** each pinned by a hashed list (m3):
     - P1 targets (waves 1–2);
     - Warne & Larsen (DP1);
     - any RR whose claim is the frozen claim of a live contest (CT-*);
     - RRs outside the domain. **In domain** means PCI RR's thematic field is psychology or its sub-fields, the behavioural, cognitive or social sciences, or cognitive neuroscience with a behavioural primary outcome (m2).
2. **Target Screener**, split across fresh sessions of at most 15 candidates each (m7). It writes no predictions and sees no probabilities. A candidate passes only if all of these hold:
   - **(a) Results not public (M2, N3):** a dated search.
     - **Required sources:** PCI RR, the journal, Crossref (Stage 2 or related DOIs) and `api.osf.io` (OSF and PsyArXiv). **A candidate whose required sources cannot be reached fails (a).**
     - **Best effort, logged:** Google Scholar, thesis repositories and news.
     - **Recall probe at the screen (N5):** a no-tools harness session, given title and authors only, is asked whether it knows the outcome. **Any specific claimed result fails (a).** This runs before the commit.
   - **(b)** The full Stage 1 protocol can be opened and is cited by section or page.
   - **(c) The scored test (B1):** the hypothesis the protocol names as **primary**. If none is named, the **first hypothesis in the study design table that is not an auxiliary check** (a positive control, manipulation check, outcome-neutral test or quality check).
     - The Screener quotes its decision rule verbatim and writes one line: "**Supported if …**".
     - A **second, independent agent** checks that line against the quote, **and checks which design-table row was picked** (n1). On disagreement, the candidate fails (c).
   - **(d)** An expected completion on or before 2029-12-31, or none stated. The number with none stated is reported (m8).
3. **Commit before the seed (B3):**
   - **Commit:** the screened list, sorted by PCI RR record id, is hashed and **timestamped with FreeTSA and OpenTimestamps before block h is mined.**
   - **h:** fixed at ratification at least 300 blocks (about 2 days) ahead: **h = 977740** (tip 977420 at ratification, mempool.space, 2026-11-24). If the list is not committed and timestamped by then, **h moves to the first block at least 300 blocks after the commit.** This is a mechanical rule, logged.
   - **Reading h:** the block hash is read from two explorers at 6 or more confirmations, and both must agree.
4. **Assignment (B3, M10):** the pinned script `dp2_draw.py`, **sha256 `408f142227806c6df9583a01f1a31fa1cba34cd18787800ac107ffc805fd398b`**, Python 3.13 (N2). It checks the list hash and that the commit's attestation is below h, prints the full shuffled order, and has a self-test:
   - **seed:** `int.from_bytes(sha256(bytes.fromhex(block_hash)), "big")`;
   - **draw:** `random.Random(seed).shuffle()` over the committed list;
   - **assigned:** the first 12;
   - **replacement order:** the rest, in that order.
   - **Replacement:** a target is replaced from that list **only if it is dropped before its Scientist session starts**, for example because its results became public. After that, a dropped target counts as a failure.
   - **If fewer than 8 targets pass the screen,** DP2 stops and reports. There is no switch to a relaxed screen.
   - The Scientist must address every assigned target.

## 4. Roles (fresh agents; none has seen P1 or DP1 forecasts)
1. **Scientist Agent**, one session per target, so that targets cannot influence each other.
   - **Inputs:**
     - the target's Stage 1 protocol (cited);
     - the claim it tests, as stated in its design table;
     - if that claim or its concept is in the store, the concept-level evidence (read-only).
   - **Tools:** WebSearch and WebFetch for published literature and the protocol. **Never** for this target's results, data, preprint or talks.
   - **Logging (M2, N4):** the session **lists every query and URL in its hand-back**. This is self-reported (§9) and archived. At the end, it states `saw_results: yes/no` with a reason. **A target where results were seen, or where a listed URL is a results source, counts as a failure.** The Scientist's prompt does **not** say how a "yes" is scored, so the self-report has no reason to under-report (n3).
   - **Model (M3):** each role's model id and stated knowledge cutoff are recorded.
   - **Tasks:**
     - (i) **Diagnose**, in at most 300 words, why the existing evidence on the claim conflicts or is thin.
     - (ii) **Propose 1–2 hypotheses**, each a sharpened sub-claim or boundary condition with a defining feature measured independently of the outcome (D20).
     - (iii) **Give one advance prediction:** **P(supported)**, the probability that the scored test (§3.2c) is **supported under the protocol's own decision rule** (B2). "Not supported", "inconclusive" and partial support are all the complement.
       - It states which hypothesis the prediction follows from, and **how** that hypothesis moves the probability away from the base rate (M6).
       - **Base-rate sentence (N7):** the Scientist and both Skeptics are given the same sentence: "In a published sample of 71 Registered Reports (Scheel, Schijen & Lakens, 2021), 44% supported their first hypothesis."
       - An optional second prediction on another pre-registered test is logged but **not counted**.
     - (iv) **State what would count against each hypothesis.**
   - A target where the Scientist finds no sharpened hypothesis is logged "no hypothesis". **It counts against form.**
2. **Rescue Auditor** (independent), with DP1's R1–R4 unchanged:
   - R1: an independent measure;
   - R2: a prediction that can fail;
   - R3: more than relabelling;
   - R4: the target pre-registers the analysis the prediction needs.
   - **Added under R2 (M6):** the prediction must follow from the stated hypothesis, and the stated direction of departure from the base rate must be what the hypothesis implies. A prediction with no such link fails R2.
3. **Skeptic Forecasters (M7), two kinds,** each independent and each giving P(supported):
   - **Blind Skeptic:** sees only the prediction statement, the resolution rule and the quoted decision rule;
   - **Informed Skeptic:** also sees the full protocol, but not the Scientist's diagnosis or hypotheses.
4. **Leak checker** (independent). Before seal 2, it re-runs check (a) for every target. It confirms, from the **logged queries and URLs**, not only from the outputs, that no session reached a results source (M2).
   - **Recall probe, second run (M3, N5):** after the Scientist sessions, a no-tools harness session is asked again, title and authors only. **A target it answers with a specific result is not counted, and counts as a form failure.** Results are also reported **split by IPA date before or after the models' stated cutoff**.
5. **Blind adjudicator**, at resolution. It sees each resolution rule, the "Supported if …" line and the results source, never the probabilities.
   - **Which version counts (m1):** the first **recommended Stage 2** at PCI RR, or failing that the published journal version.
   - **What counts as supported:** the authors' own conclusion under the pre-registered rule, unless it contradicts the quoted rule, in which case the rule decides and this is logged.

**Harness (M4):** see §9. **D57 (M5, n2):** every packet passes `firewall.py gate` (code `827a2edf…`).
- **Hits in protocol text** go to the user for review, with the reviewer, date and reason logged. **They are never grounds to drop a target after the draw.**
- **Prompts carry nothing the D57 patterns screen for** (about what depends on an outcome).


## 5. Pass criteria (fixed now)
1. **Form (M10):** at least **6 distinct targets** each have a counted prediction that passes R1–R4, whatever the number of targets assigned. At most 1 is counted per target. "No hypothesis", "results seen" and a dropped target count as failures for that target.
2. **Scope:** no Scientist output reaches the store, a status, a credence or a contest (checked by the main session).
3. **Resolution plan:** every counted prediction names its target, its quoted decision rule and a findable results source (the Stage 2 at PCI RR or the journal).

## 6. Scoring (descriptive only)
- **When (M9):** at the first 10 resolved counted predictions **or 2029-12-31, whichever comes first**, and again at 2029-12-31. **If fewer than 6 have resolved by then, the result is "not evaluable".**
- **Comparisons:**
  - Brier(Scientist) against Brier(Blind Skeptic) and against Brier(Informed Skeptic);
  - Brier(Scientist) against Brier(0.5);
  - Brier(Scientist) against Brier(**base-rate forecaster, P(supported) = 0.44**) (M8). The source is Scheel, Schijen & Lakens (2021), *Advances in Methods and Practices in Psychological Science*: 44% of 71 published Registered Reports supported their first hypothesis. Partial support counts as not supported, as in our scored event. **Disclosed:** that sample is RRs published by November 2018 and codes only the first hypothesis.
- **Unresolvable:** a target with no **recommended Stage 2 at PCI RR or published journal version** (§4.5, n4) by 2029-12-31, or whose primary test changed after IPA without the change being pre-registered, is logged as unresolvable.
- **Inference:** none. A formal test needs a later, larger pre-registered batch.

**Phase 6 scaling** still needs all of (m6):
- the DP2 descriptive comparisons favouring the Scientist over **all four comparators** (Blind Skeptic, Informed Skeptic, 0.5 and the base rate) **at the evaluation point in §6** (N8);
- the D49 gate: the store's statuses shown to predict;
- **a larger pre-registered batch with a formal test**, before any scaling claim.

## 7. Guardrails (D49, unchanged)
- Outputs are **Pending** items in this pilot's log only. Nothing touches any status, credence, contest record or settlement criterion.
- Predictions are sealed by hash before any resolution.
- The Scientist is not scored on how many or how plausible its hypotheses are, only on how its predictions resolve.
- The Philosopher role is not used in DP2.
- **Budget (D49 guardrail 6; m5):** DP2 is one pilot. It uses no funds (D59), and its only cost is session time. Any scale-up needs its own budget line under the value function and the exploration share.

## 8. Timeline and cost
- **Before seal 2:** the survey, screen, commit, draw and per-target sessions take several sessions over about a week.
- **The commit waits for the API credits (f3):** the first recall probe (§3.2(a)) is a harness step and runs before the commit. So the commit, and with it h under §3.3, comes after the credits arrive and the dry run passes.
- **Draw records (f2):** the commit's `.ots` file and a block-explorer printout of block h, showing its hash and at least 6 confirmations, are archived with `DRAW.json`.
- **Resolution:** the monthly check, from 2027 onward; most Stage 2s are expected in 2027–2029.

## 9. Logged choices
- **Harness (M4; user, 2026-11-24, "API harness, after credits (Recommended)"):**
  - **Through the D60 harness (N1),** with no tools: the **Rescue Auditor, both Skeptics, the recall probe (both runs) and the blind adjudicator**. They run once the API credits arrive.
    - **Model** `claude-opus-5-5`, **temperature 0** for every harness role, and max_tokens 8000.
    - **Harness:** `te_api.py` sha256 `8807790d…`.
    - **Runner:** a DP2 runner `dp2_api.py` using that client, sealed with its sha256 in an addendum **before the first harness step**.
    - **A dry run of every harness step comes first** (D60).
    - **Their packets (N6):** they carry the protocol extract and, at adjudication, the Stage 2 extract.
      - The **Target Screener** extracts the protocol text.
      - A **fresh Extraction session** extracts the Stage 2 text, **only after seal 2**.
      - Each extract is hashed and passes the D57 gate.
  - **Chat agent sessions** (Claude Opus 5.5, model recorded per session): the **Survey agent, the Target Screener, the second-check agent, the Scientist sessions, the leak checker and the Extraction session**, because they need web tools, which the harness does not offer.
    - Their every query and URL is **listed in the session's own hand-back and archived**.
    - The leak checker compares those lists against results sources.
    - **Disclosed:** that list is self-reported, a weaker record than a harness log.
- **Browser (M1; user, 2026-11-24, "Yes, PCI RR pages only (Recommended)"):** the Survey agent may use the user's built-in browser read-only on `rr.peercommunityin.org` only, with no sign-in and no forms, for pages web tools cannot render.
- **Block h:** fixed at ratification as the tip + 300 or more blocks (§3.3).
- **Direction 1** (user, 2026-11-24).
- **12 targets** and a **form bar of 6**:
  - 12 makes "≥ 6 distinct studies" a real test, while keeping the session load small;
  - 6 keeps DP1's 3-hypothesis bar in proportion to the target count, with stricter spread (one per target).
- **The base-rate baseline is new.** It guards against a Scientist that only predicts "supported" or "not supported" in line with the field's base rate.

## 10. Audit findings → where applied
- **BLOCKING:** B1 §3.2(c); B2 §4.1(iii); B3 §3.3–§3.4. **Diff check:** N1 §9 harness; N2 §3.4 (script hash).
- **Diff-check MAJOR and MINOR:** N3 §3.2(a); N4 §4.1; N5 §3.2(a), §4.4; N6 §9; N7 §4.1(iii); N8 §6; n1 §3.2(c); n2 §4; n3 §4.1; n4 §6; n5 the status line.
- **MAJOR:**
  - M1 §3.1, plus the user's browser decision (§9);
  - M2 §3.2(a), §4.1 logging, §4.4;
  - M3 §4.1 and §4.4;
  - M4 §9 (user decision);
  - M5 §4, the gate;
  - M6 §4.1(iii) and R2;
  - M7 §4.3;
  - M8 §6;
  - M9 §6;
  - M10 §3.4 and §5.
- **MINOR:** m1 §4.5; m2 and m3 §3.1; m4 §3 header; m5 §7; m6 §6; m7 §3.2; m8 §3.2(d).

**SEAL 1:** sha256 of everything above this line: `397091f603b1398234b121b39ce6598c873a8455c1192a46a32b0eb1b8587db9` · 2026-11-24
