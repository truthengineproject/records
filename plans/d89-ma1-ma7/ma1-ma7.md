# MA1 and MA7: the fidelity check and the literature search (draft v2)

**Status:** **RATIFIED by the user, 2026-11-24 (D89)**, as v2.1: "Ratify and seal (Recommended)", with decisions 1–8 as in §5. Profile Architect. Audit path (`ma1-ma7-audit.md`): review "revise" (3 BLOCKING, 11 MAJOR, 12 MINOR); re-check v2 "ratify with listed amendments" (0, 2, 7), all applied. **MA7 is resolved. MA1 is resolved in text but not live:** go-live needs the planted-fault audit (§1.8) to pass and the code amendment (§1.7) in profile v1.7. **Nothing displayed changes before then.**

**What changes on display:** nothing until the planted-fault audit (§1.5) passes **and** the go-live amendment (§1.7) is ratified. After that, the only changes are the ones MA1 always intended: L6 and ST4 ("Formally verified") become reachable, for items reviewed under the audited configuration and signed off by an expert. No new status label is added. The literature marker gains its depth and, if chosen, a staleness note (free text in the existing `search_note`).

---

## Part 1. MA1: the fidelity check

### 1.1 What is checked, and what counts as a pass (M1)
The formal statement, with every definition in its dependency cone unfolded to the library, is compared with **the statement of the link it is credited to** (profile §2.4; A1): the printed statement for an ordinary link, or the corrected statement for a corrected link.

**Pass means equivalence:** the formal statement and its unfolded definitions say **the same thing** as that statement, up to the permitted differences in §1.2. A formal statement that is **strictly stronger** and still implies the target statement is recorded as a separate, non-failing note ("formal statement stronger than printed"), and it passes only if the reviewers confirm the implication; **an implication the reviewers do not confirm is unresolved** (R1).

### 1.2 Permitted differences, by context (B1)
- **M1 stratum C items:** exactly profile §4b: the M1 v4.2 §10.1–§10.2 well-definedness conventions and cited-result hypotheses, with their mechanics. Links then mean "established relative to the listed results" (§4b, unchanged).
- **Misprint readings (GC-1a):** only if and when GC-1 or a successor takes effect (it is not in effect: §3.1b, D66).
- **Every other item: none.** Extending conventions or cited hypotheses beyond stratum C is **decision 5** (§5).

**Permitted differences are themselves reviewed** (M3): a convention is needed only for an expression to be defined and never for truth; a cited hypothesis matches its source's printed statement (M1 §10.2); a misprint reading meets GC-1a.

### 1.3 The checklist (M3; profile §2.4 plus two added types)
For each item, *no drift*, *drift* (with the exact clause), or *unsure* (with the reason):
- **D1** an added hypothesis (including one needed for truth, which is never a convention), or a missing one **where the formal statement then does not imply the target** (R1);
- **D2** a wrong number system or type (ℕ for ℤ, truncated subtraction, coercions, division by zero, `Real.sqrt` of negatives);
- **D3** quantifier scope or order;
- **D4** a dropped or weakened part of a multi-part statement (including "iff" as one direction);
- **D5** mismatched definitions (conventions on 0, open or closed, normalization);
- **D6** vacuity: contradictory or unsatisfiable hypotheses, or a junk value that makes the goal trivial *(new; not in the profile's §2.4 list)*;
- **D7** an altered conclusion: a constant or bound, strict versus non-strict, an index or range off by one, "eventually" versus "for all", effective versus ineffective, existence versus an explicit object *(new)*;
- **D8** presentation and elaboration traps: local notation or instances, shadowed names, `axiom`, `opaque` or `sorry` in the dependency cone, coercions or implicit arguments hidden by pretty-printing. Checked from the **elaborated** statement, with an automated `#print axioms` and a check for local instances and notation, logged *(new)*.
- **For contest items only (MA9 §4):** each hypothesis is in the registered step or a registered established result, checked **by name**; and the hypotheses are **jointly satisfiable** in the intended application, by an instance, a model or a recorded argument the review accepts.

### 1.4 The procedure and its three outcomes (M2)
1. **Reviewer:** a fresh session that **did not write** the formal statement and has seen **no proof** of it (profile §2.4). It sees the target statement (with page or section), the formal statement, the unfolded definitions, the automated D8 output, and the recorded permitted differences. It has **no web and no library search access** while reviewing (M4).
2. **Outcomes:**
   - **pass:** every required review finds no drift on every item;
   - **failed:** a drift **with its exact clause**, confirmed by a second fresh reviewer or by exhibiting the differing reading (an input on which the two statements differ). The formal proof then verifies a **different statement**: it gets its own node, linked by `formalizes-variant`, and the original gains nothing (profile §2.4, unchanged). **Whether the difference comes from the proof** (A2: a statement-changing gap) is decided by an **MA5 coder who can see the proof**, not by the reviewer;
   - **unresolved:** any *unsure*, or a drift claim that is not confirmed. The link stays **`unchecked`** (displayed "fidelity unchecked"). An unresolved review never fails a link.
3. **Pass rule: decision 1** (§5): (a) two independent reviews, both pass; or (b) one. **Every pass needs at least one review under an audited configuration** (R2). An independently written `formal_independent` statement counts as **one** review (profile §2.1), so it can only be the **second** review under (a). Same-family reviews are correlated (MA8); this is disclosed on every passed item, and the planted-fault audit measures the configuration actually used.
4. **Expert sign-off (B3, m2):** a named domain expert sees **the same inputs** as the reviewers and signs off; the sign-off is logged (P8; profile §2.4). **In code, `passed` means "MA1 pass under an audited configuration, and expert sign-off".** An MA1 pass **without** a sign-off is stored as `unchecked`, so it displays as ST5 ("Formally verified (fidelity unchecked)"), and the review result appears only in the fidelity record and the detail view. No new label exists. *Decision 2 (§5) offers the alternative, which would amend profile §2.4 and P8.*
5. **Records:** reviewer roles and models, the configuration hash (§1.6), the library commit and toolchain (m3), every input's hash, each item's finding, and the outcome. The record is the link's fidelity attribute. **MA2:** if a definition in the cone changes in a later library version, the fidelity reopens (unchecked) until re-reviewed.

### 1.5 Contests (B2; MA9 §4 governs)
For an `empirical-mathematical` contest, **MA9 §4 governs** reviewer selection, the 60-day and 30-day windows and the handling of disagreement: one reviewer from each side; an unanswered side replaced by the neutral named at registration; the neutral or an independent formal statement can confirm fidelity; otherwise the result is credited to the variant node and the contest stays open; the Judge never decides fidelity. **MA1 supplies** the checklist (§1.3, including the contest items), the records and the outcome semantics. **Disclosed:** the planted-fault audit certifies only the agent configuration, not side or neutral reviewers. Whether a contest panel must also include one audited-configuration review is **decision 6** (§5); it would amend MA9.

### 1.6 Configuration lock (M7)
What the audit certifies is a **configuration**: the reviewer model(s) and versions, the prompts, tool access (none), the input format, the D8 automated checks and the pass rule. Its sha256 goes into the audit's seal and into every item's fidelity record. **Any change needs a fresh audit.**

### 1.7 Go-live (M8)
`MA1_RESOLVED` is not flipped globally. Instead:
- the code gains a per-record field `ma1_config`; a stored `passed` counts as passed **only if at least one of its reviews has an audited `ma1_config` and an expert sign-off is recorded** (R2). Every other `passed`, including every pass recorded before MA1 and M1's own fidelity reviews, is treated as `unchecked`;
- this change is a **code amendment**: `math_status_v17.py`, re-sealed, with the self-test extended to show that, with no audited configuration recorded, every status equals v1.6 exactly, **and to test the audited cases** (audited pass with and without sign-off; a `formal_independent`-only pass; a pre-MA1 pass) (r7);
- **before go-live, a confirmed `failed` from this procedure is recorded but not applied to any status** (r6). Today this is moot, because the store holds no mathematics claims;
- the change is ratified with the planted-fault audit's result, as a logged profile amendment.

### 1.8 The planted-fault audit (gate for go-live; M4, M5, M6)
**Sealed before any material exists:** this protocol, the bars, the counts, the configuration (§1.6), the key-checker's brief and the scoring script, with FreeTSA and OpenTimestamps (the gate (a) and recert practice).

**Builder** (an independent session that never reviews):
- takes real formal statements of published results from public formal libraries, at a named toolchain and commit, and their printed statements;
- produces **faulted items**, one planted drift each, at least **10 per type D1–D8 after any key-checker drops, so it builds a surplus** (r2), some placed in an **unfolded definition** rather than the top line, each type-checking (the check is logged with toolchain and commit, m4);
- produces **clean items** (count: decision 3), **rewritten like the faulted ones** (normalized names, hypothesis order, formatting) so that verbatim library text does not mark an item clean;
- uses **no base result in both classes**, and recorded permitted differences at similar rates in both classes.

**Custody:** the key and the builder's source map (item to library theorem, which is key-equivalent) are **encrypted** with the M1 custody method (`custody.py`, procedures v2.1) before any review, held where no reviewer can read them; the builder reports counts only. **Scoring is by the sealed script; the main session does not read the key before scoring.**

**Key-checker** (an independent session that never reviews): **before any review**, it sorts every item as **equivalent**, **stronger and implies**, or **does not imply** the target (R1), and confirms that every faulted item's planted drift is real. Items it rejects are dropped or replaced, and **replacements are key-checked too** (r2). Its result is recorded in the seal.

**Decryption key holder (r2):** as M1 procedures v2.1 key custody (D68): a Custodian session, with the roster, the written attestations and the disclosed same-disk weakness. The key is used only by the sealed scoring script.

**Reviews:** the full §1.4 procedure under the pass rule chosen, one fresh session per item. Reviewers are **not told it is an audit**, and the class ratio is not disclosed. Every reviewer packet passes the D57 gate.

**Scoring:** a faulted item is **missed** if it ends **pass** and the key-checker sorted it **does not imply** (R1); a pass on a stronger-and-implies item is scored as correct, and its note is checked. A clean item is a **false alarm** if it ends **failed**. *Unresolved* items are reported separately (they leave links unchecked, so they are not misses). **The unresolved rate on clean items must be at most the ceiling set in decision 3** (r1), so that a procedure that is often unsure cannot pass while rarely passing anything. **Per-type results are reported.**

**Bars (decision 3):** all one-sided 95% exact (Clopper–Pearson) upper bounds:
| Option | Faulted items | Misses allowed | Upper bound on the miss rate | P(pass) if the true miss rate is 1% / 2% |
|---|---|---|---|---|
| (a) | ≥ 80 (10 per type) | 0 | 3.7% | 0.45 / 0.20 |
| (b) | ≥ 95 | ≤ 1, in any type | 4.9% | 0.75 / 0.43 |

*(With 8 types, the minimum is 80 faulted items, so (a)'s bound is tighter than v1's 0 of 60.)* Clean items: **(i)** ≥ 60 with ≤ 3 false alarms (upper bound 12.4%), or **(ii)** ≥ 30 with ≤ 3 (upper bound 23.9%; a point bar of 10%).

**Disputes (M5):** after scoring, a disputed item goes to the Phase 3 blind X/Y adjudication, pre-specified. Both scores, with and without adjudication, are reported.

**If it fails:** go-live does not happen; the procedure is revised by logged amendment and re-audited on a **fresh** set. Items are never reused. **At most three attempts**, all disclosed (m5).

**Relation to TM2 (M6):** the audit bounds the **miss rate**. TM2 counts the share of passed items that later fail, p·m / (p·m + (1−p)(1−f)) for drift prevalence p. At realistic prevalence the audit's bound implies TM2's bar; it is not the same quantity.

### 1.9 The ongoing check (M9; TM2 is the user's trigger, so decision 7)
Proposed: each quarter, a random 10% of passed items, **at least 5**, is re-reviewed by fresh sessions under the audited configuration, with **fresh planted canary items** mixed in, built and held under §1.8's custody; **a canary that passes counts as a confirmed fail for TM2 and is reported** (r3). A failure is a **confirmed named-clause fail**. Counts are pooled across quarters; **TM2's ratified response applies when the pooled confirmed-fail rate exceeds 5% with at least 20 items re-reviewed**, or at once on two confirmed fails before 20. A re-reviewed item that fails is set to `failed`; one that ends unresolved is set to `unchecked`.

---

## Part 2. MA7: what counts as "searched"

### 2.1 Purpose and scope
The literature marker (profile §3.1, ST7–ST9) says whether a statement shown as open or claimed has been checked for an existing solution, refutation or formalization. It is appended, never replaces a status, and is graded descriptively (profile §4, m1). **It does not run on M1 wave targets before H**; there, PP-1's registration search is the instrument and its blinding governs (m11).

### 2.2 A search counts as done when all of these are logged
**"Source paper"** means the earliest paper that poses the statement and, for ST7, each paper with a claimed proof (m6).

**Required sources** (a required source that cannot be reached means **not searched**, never a pass, as D81; a transient failure is retried once at least an hour later, as PP-1 §4; not-searched items are re-queued):
1. **zbMATH Open API:** each source paper by id, and its citing documents (`rf:`), paginated to the end (the PP-1 method).
2. **OpenAlex:** citing works (`cites:`) of each source paper, paginated; and works matching the statement's name. *(Crossref is used for DOI metadata and title matches only, because its citing-works list is not public, M10.)*
3. **arXiv:** the statement's name and key terms, in the relevant `math.XX` categories (plus the MSC field where present), since the earliest source paper's year (m6).
4. **Formal libraries:** Mathlib at the commit current on the search date (`git grep` by name and key terms, with PP-1 #4's disclosed weakness that TeX names rarely match), the Archive of Formal Proofs, and public formalization platforms such as Prove2Me, as user-run queries where the workspace is refused (D81's 7-day rule).
5. **Problem lists:** the Erdős-problem database for combinatorics and number theory statements, and OEIS for sequence statements, **always**, by key terms (m7).
6. **MathOverflow and Math StackExchange,** through the StackExchange API (m8).

**Best effort, logged:** Google Scholar, Semantic Scholar, PubPeer, Retraction Watch, Lean Zulip and blueprints.

**Search terms:** fixed **before** searching: the statement's name, each source paper's DOI and title, and up to five key terms chosen from the printed statement and logged.

**Screening rule (M11):** every hit's title, and its abstract or zbMATH review where available, is read and coded **candidate** or **not relevant**. A candidate is any work that states, in its title, abstract or review, a proof, disproof, counterexample, formalization, or correction of the statement. Candidates are opened and confirmed or rejected by a **second session**, with the reason logged. A confirmed item is coded under MA5 before any status changes (A3, m10), and while it awaits coding the marker shows *found, being coded*.

**Disclosed limit (r5):** screening by title, abstract and review misses solutions that appear only as corollaries in other work; this is stated next to "depth 1" in the marker's detail view.

**Rehearsal before sealing (r4):** OpenAlex `cites:`, the Erdős-problem database, OEIS and the StackExchange API are each tried in a logged rehearsal before MA7 is sealed; a source that cannot be reached there is reported to the user before ratification.

**Record:** every query, source, date and hit list (hashed), each screening code, and a verdict: **nothing found**; **found** (with the item); or **not searched** (a required source failed).

### 2.3 Second look (M11)
- **Sample:** 20% of each batch, **at least one item**, drawn with a seed fixed **before the batch's first search** (a beacon block hash or a sealed value, as D81's `rs2_seed`).
- **Method:** a fresh second searcher chooses **its own** key terms, blind to the first searcher's, and runs §2.2.
- **A hit the first search missed** sets that item to *found* and is reported. **What a miss leads to is decision 8** (§5).

### 2.4 What is shown
- *searched (date, sources; citing works, depth 1)* when §2.2 holds, using the profile's punctuation (m12);
- *literature not checked* otherwise; *found, being coded* while a confirmed item awaits MA5 coding.
- **Staleness: decision 4.**

### 2.5 Who
A Literature Searcher: a fresh session per batch, with web tools and the shell for the scripts, and user-run queries where the workspace is refused. No searcher sees any status, credence or forecast.

---

## 5. Decisions (user, 2026-11-24, D89)
1. **Pass rule:** two independent reviews, both pass; at least one under an audited configuration ("Two, both pass").
2. **Display:** the named-expert sign-off stays required for "Formally verified" ("Keep expert sign-off").
3. **Planted-fault bars:** faulted **≤ 1 miss of ≥ 95** (one-sided 95% bound 4.9%); clean **≤ 3 false alarms of ≥ 60** (bound 12.4%); unresolved on clean items **≤ 20%** ("(b) ≤1 of 95").
4. **MA7 staleness:** a marker older than 12 months is shown as *searched (date, over 12 months ago)* until re-searched ("Flag after 12 months").
5. **Permitted differences:** none outside M1 stratum C, as the profile is now ("No, M1 stratum only").
6. **Contest panels:** MA9 §4 unchanged; the audit's scope is disclosed ("Keep MA9 as is").
7. **TM2 ongoing check:** as §1.9 ("Adopt as proposed").
8. **MA7 second-look misses:** any miss sends the whole batch to a re-search by a fresh searcher; a pooled miss rate above 10% after at least 20 second looks revises MA7 by logged amendment ("Batch re-search").

Options shown and the recommendations are in v2.1 (`ma1-ma7-v2-rechecked.md` and the audit). Every recommendation was chosen. **Reason: no reason given.**

## 6. Audit findings → where applied
- **BLOCKING:** B1 §1.2, decision 5; B2 §1.5, decision 6; B3 §1.4.4, §1.7.
- **MAJOR:** M1 §1.1; M2 §1.4.2; M3 §1.2, §1.3; M4 §1.4.1, §1.8; M5 §1.8; M6 §1.8 (table, relation to TM2), decision 3; M7 §1.6; M8 §1.7; M9 §1.9, decision 7; M10 §2.2 item 2; M11 §2.2 screening, §2.3, decision 8.
- **Re-check (v2):** R1 §1.1, §1.3 D1, §1.8 key-checker and scoring; R2 §1.4.3, §1.7; r1 §1.8, decision 3; r2 §1.8; r3 §1.9; r4, r5 §2.2; r6, r7 §1.7.
- **MINOR:** m1 references corrected throughout (§2.4, §4) and D6 marked new; m2 §1.4.4, decision 2; m3 §1.4.5; m4 §1.8; m5 §1.8; m6 §2.2; m7 §2.2 item 5; m8 §2.2 items 4 and 6; m9 §2.2; m10 §2.2; m11 §2.1; m12 the wording (no gate hit), the marker punctuation, and the unsupported M1 comparison removed.

## 7. What this does not change
The status rules (until the §1.7 amendment, whose self-test shows equality with v1.6 when no audited configuration is recorded); the A4 interim rule until go-live; MA5; MA9 §4; M1's own fidelity review and its stratum-C exception; the scope rules.

**SEAL:** sha256 of everything above this line: `0b013ea4792181b3d241fab4c651ef3a2e95665f2311f00b4b42a7d4155a049a` · 2026-11-24
