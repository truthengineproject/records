# Evidence Profile: Mathematics, v1.6 (RATIFIED)

**Status:** **v1.6 RATIFIED by the user, 2026-11-24 ~13:25 PT (D82).** It records D80 (MA9) and supersedes v1.5 (`066925de…`). `math_status_v16.py` is now the status authority. **v1.5 status:** **RATIFIED** as Mathematics Evidence Profile v1 by the user, 2026-11-21 (**D56**). **Amended as v1.1 on 2026-11-23 (D61): amendment M-A1 and clarification C-1, §4a. Amended as v1.2 on 2026-11-23 (D62): GC-1, §3.1a. Amended as v1.3 on 2026-11-23 (D63): C-2, the stratum-C fidelity exception, and the TM1 primary-stratum change, §4b. Amended as v1.4 on 2026-11-23 (D65): GC-1 validation failed; GC-1.2 ratified, pending re-validation, §3.1b**. Ratified on the recommendation of Profile Audit v3 ("ratify with listed amendments"; 0 BLOCKING items; amendments applied). It was drafted as v0–v3.1 through four audit passes. Changes only by logged amendment (D52). **No mathematics claims enter the store** until MA1, MA5, MA7 and MA9 are resolved and a sealed test exists (§4). · **Drafted by:** Profile Architect (main session acting in that role). It will not audit, judge or forecast in this domain. · **Audits:** `claude/math-profile-audit-v0.md` ("revise first"; N-M1, N-M2) and `claude/math-profile-reaudit-v1.md` ("revise again"; R1, R2), `claude/math-profile-audit-v2.md` ("revise again, narrowly"; B1), and `claude/math-profile-audit-v3.md` ("ratify with listed amendments"; 0 blocking). · **Status rules as code:** `claude/math-profile-v3-status-rules.md` (`math_status.py` v3.1). · **Builds on:** spec §3.1 (claim type `mathematical`), §5.3, §13a (domain 2), D29, D40–D48, D51–D52; Science Evidence Profile v1, as amended by v1.1–v1.2.

**What changed from v0** (details in §10):
- **Statements and proofs are now separate objects** (N-M1), and the status rules are rewritten so that every combination of evidence gets exactly one status (N-M2).
- **"Unproven as stated" is no longer folded into Disproved,** and "fidelity unchecked" is kept apart from "fidelity failed".
- **New:** failure-trigger options (§7a), a horizon on conjecture credence, leakage and censoring controls, and an interim no-contest rule.
- **Source misuses corrected** (S3, S6, S7, S8, S10).

---

## 0. Why the §13a sketch needed changing (summary)

The sketch assumed mathematics would be mostly "settled where verified", with a slow outcome signal. Developments in 2026 change that, though less cleanly than v0 said:

1. **Formal verification has become fast and common.**
   - Fermat's Last Theorem was formalized in Lean in about 11 days. That run built on earlier human formalization work, and its top-level statement was checked against Mathlib's (S1).
   - AI-assisted formalizations of **published** work turn up repairable gaps: wrong intermediate bounds, missing hypotheses, edge cases. Examples are the MIP\* = RE component (S2) and EconCS papers (S4). Formalization of the authors' own preprints catches gaps before publication too (S3).
   - So **"does a refereed proof survive formalization as stated?"** is now a frequent outcome, the closest analogue of a replication. Because repairable gaps are near-universal (S2–S4), the useful outcome is not "any gap" but whether the **statement** survives (§4).
2. **Long-held conjectures are being resolved, often against expectation.**
   - The Jacobian conjecture has been disproved in dimension ≥ 3 by an explicit map, independently verified in Isabelle (S5).
   - Erdős's unit-distance conjecture has a claimed disproof. It is an existence proof checked by external experts but not yet refereed or formalized, so under this profile it is a Claimed proof of the negation, not Disproved (S6).
   - One website lists hundreds of AI-assisted results (S7). Its inclusion rules are unknown, so it is not a base rate.
3. **"Open" is a claim about the literature.** In one systematic screen, most of the problems an AI resolved had earlier solutions in the literature (S8). That sample was selected for being solvable, so it shows the label can be wrong, not how often it is.

**Judgment (unsourced; see MA11):** mathematics may resolve faster than psychology. The main new risk is **statement fidelity** (S9).

## 1. Scope

- **Covers:** claims of type `mathematical`: theorems, lemmas, conjectures, counterexamples and computational facts.
- **Excludes:**
  - models of the world (science profile);
  - empirical claims about mathematicians or journals. These are science claims, used here only as grading data.
- **Boundary rule:** a claim like "theorem T implies the protocol is secure" splits into the theorem (mathematics) and the modelling assumption (science or engineering), joined by a `depends` edge. The Architect proposes the split; the Auditor checks it.
- **Mode:**
  - **Graded** for statements whose proofs are not formally verified, and for conjectures, once the §4 tests exist;
  - **Settled, with stopping points,** for formally verified statements;
  - **Mapped** for questions of value or importance, which the type gate routes as value-laden.

## 2. Evidence types

Every proof node carries at least `claimed_proof` (the default code).

### 2.1 Proof evidence (attached to a **proof** node, §3.1)
| Code | Evidence | Notes |
|---|---|---|
| `formal_verified` | Machine-checked in a proof assistant (Lean, Isabelle, Coq/Rocq, HOL, Metamath), with no unproven placeholders. | Record:<br>• the system, library and versions;<br>• the **axioms used**;<br>• the fidelity result (§2.4): passed, unchecked or failed. |
| `formal_independent` | A second formal check whose statement was written independently, in a different system or by a different team. | It adds a lineage for display only (P6 is an open hypothesis; D35). Two checks by agents of the same model family do not count as independent (MA8). Its independently written statement counts as **one independent fidelity review** toward MA1's pass rule. |
| `refereed_proof` | Published after peer review in a recognized venue. | Evidence, not verification. |
| `expert_checked` | A documented check by named experts outside refereeing, such as a seminar or a written digest. | Shown as a qualifier on Claimed proof (§3.1). The sign-off is a human intervention and is logged (P8). |
| `claimed_proof` | A preprint or announcement, unrefereed and unverified, including AI-generated proofs. | Scored on content (P5). AI provenance is neither a penalty nor a credential. |
| `computational` | A computer-assisted step without a verified checker. | Upgraded to `formal_verified` if the computation is itself formally checked. |
| `computational_reproduced` | Public code, independently re-run, with the same result. | Counts toward a proof (Computer-assisted proof) **only if the human-checked remainder is refereed or `expert_checked`**. Otherwise the proof stays a Claimed proof. |
| `gap_report` | A documented gap in the proof: from a formalization, an erratum, a referee or a published comment. | Coded as **repaired** (statement unchanged), **statement-changing** (needs a stronger hypothesis or a weaker conclusion; v1.2: includes a printed statement that is false where an established corrected version of the same result exists, G-1(c), §3.1a), **unrepaired** (open), **fatal-confirmed** (v1.2, GC-1 G-1, see §3.1a: within its scope, the argument cannot establish the printed statement or any established corrected version of the same result; accepted by the authors or independently confirmed, formally or by a second coder), or **fatal-disputed** (coded fatal, but contested by the authors and not independently confirmed). Coding rule and inter-rater check: MA5. A formalization of the same argument either repairs a gap or records it as statement-changing; it never leaves it unrepaired. |

### 2.2 Conjecture evidence (attached to the **statement** node)
- **`numerical`:** checked up to a bound, with method recorded. It is never proof-like (the Mertens, Pólya and Skewes cases are the warning).
- **`heuristic`:** a probabilistic or density model. When it is used as a prior, the prior audit applies (P10).
- **`partial`:** special cases, weaker forms, or conditional results, each recorded.
- **`analogue`:** the analogous statement holds in a related setting.
- **`expert_belief`:** shown, never weighted alone (the mathematical "Commonly held").

### 2.3 Disproof evidence
- **`counterexample_verified`:** an **explicit** counterexample checked **formally**, or by **two independent computations with public code with the statement's definitions checked**. A formal check also needs the fidelity review (§2.4). Until that review passes, the statement shows **Disproved (fidelity unchecked)**.
- **Disproof by proof**, an existence argument with no explicit object (S6): a proof of the negation is its own **proof node**, attached to the statement with direction "against" and handled by the same proof statuses. The statement becomes Disproved only when that proof's link is established: Formally verified (ST1), Formally verified with fidelity unchecked (ST2), or Refereed or Computer-assisted (ST3).
- **`counterexample_claimed`:** announced, not yet checked. It is a Claimed proof of the negation.

### 2.4 Statement fidelity
A formal proof verifies the **formal statement**, which can drift from the intended one in several ways (S9; the evidence there is from benchmarks, but the risk carries over):
- missing hypotheses;
- the wrong number system;
- wrong quantifier scope;
- a dropped part of a multi-part statement;
- mismatched definitions.

**Rule:**
- **The check is done by a reviewer who did not write the formal statement.** The reviewer compares the formal statement and its dependent definitions with the published statement. An agent may review, and a named domain expert signs off when the statement is shown to users as Formally verified. That sign-off is logged (P8). What counts as a pass: MA1.
- **Three outcomes:**
  - **passed:** the proof supports the statement;
  - **unchecked:** the proof supports the statement only with the qualifier "fidelity unchecked";
  - **failed:** the formal proof verifies a **different statement**. That statement gets its own node, linked by `formalizes-variant`. The original gains nothing from it.

## 3. Status vocabulary

### 3.1 Two kinds of node, and the status rules (fixes N-M1, R1, R2)

**A statement node** is the mathematical claim itself, for example "the abc conjecture" or "FLT". It carries the statement's status, its conjecture evidence and its literature-search record.

**A proof node** is one argument for or against a statement, for example "Wiles–Taylor 1995", "Mochizuki IUT 2012" or "OpenAI 2026 construction". It carries proof evidence (§2.1).

**A link** joins a proof to a statement. It has a direction (**for** or **against**) and a relation: **direct**, or **corrected** (the link to a corrected statement that a statement-changing gap produced). **A proof's status is computed per link (R1).** Three things are link attributes:
- **Fidelity (A1):** the check of the formal statement against *this link's* statement. So a formalization of a variant or corrected statement is credited on that statement's own node.
- **Coverage (B1):** each statement-changing gap has an ID, and a corrected link records which gaps its corrected statement covers. Any gap it does not cover keeps the link at "Does not establish this statement as stated".
- **The correction's own check (B1):** none, expert, refereed, or formal. **The original proof's refereeing or expert check never transfers to a corrected statement.** An unchecked correction is a Claimed proof.

**MA1 interim, enforced in code (A4):** until the fidelity check design is ratified and audited, a "passed" fidelity is treated as "unchecked". So nothing yet shows as Formally verified without the qualifier.

**Scope rule (D29):** a link status is never shown as a statement's status. "The IUT proof is contested" is about a link. The abc conjecture's status comes from the statement rules.

**The rules are stated as code.** Under v1.5 the code is `math_status.py`, sha256 `25cad02aa9acd15553e6d92df608a60e445e984ff51cf3fdaf42b79f14fcc74d`, reproduced in `claude/math-profile-v3-status-rules.md`.
- **From v1.6's ratification,** the authority is `math_status_v16.py`, sha256 `4b72509ee3a32c7b0c52a51b23a55af5fb671231e0f13129f41ec014095517d8`, reproduced in `claude/math-profile-v1.6-status-rules.md` (§3.3a). It applies the v1.5 rules unchanged to every record without step records or contests.
- **The code is the authority; the tables below summarize it.**
- **Coverage:** an exhaustive enumeration of every valid input covers the proof-link cases (under both MA1 settings) and 4,600 statement cases, with 0 invariant violations, plus the named audit cases.
- **Totality:** the rules are total by construction, because the last rule has no condition.

#### Link status (first match wins; v1.6 order)
In v1.6 the rows are checked in the order **L1, L3, L4, L2, L5**, then L6 onward. A contest therefore never lifts a link above L3 or L4. With no contest open, this order gives exactly the v1.5 results (§3.3a).
| # | Condition (on this link) | Link status |
|---|---|---|
| L1 | Withdrawn by its authors, or a **fatal-confirmed** gap | Failed proof |
| L2 | An open empirical-mathematical contest on a step **this link uses and does not replace** (v1.6, D80). None can exist until MA9 goes live (§3.3a) | Contested proof |
| L3 | A **fatal-disputed** gap | Proof with a disputed fatal gap. **A disputed fatal coding never forces Failed**, so there is no forced resolution (P3). |
| L4 | A **statement-changing** gap that this link does not cover (on a direct link, every one) | Does not establish this statement as stated |
| L5 | An **unrepaired** gap | Proof with an open gap |
| L6 | Formalization of this link's statement, fidelity **passed** (after MA1; on a corrected link, only if the correction's check is formal) | Formally verified proof |
| L7 | The same with fidelity **unchecked**, or **passed** before MA1 | Formally verified proof (fidelity unchecked) |
| L8 | Direct links only. **For:** computation reproduced at least once independently, with the human remainder refereed or expert-checked. **Against** (a counterexample): two independent reproductions plus an expert check of the definitions (A5). | Computer-assisted proof |
| L9 | Refereed (on a corrected link: **the correction** is refereed) | Refereed proof |
| L10 | Expert-checked (on a corrected link: **the correction** is expert-checked) | Claimed proof (expert-checked) |
| L11 | otherwise | Claimed proof |

A fidelity-failed formal proof is ignored on this link: it belongs to the variant statement's node (§2.4). **Established** link statuses are L6–L9. **Weak** ones are L2, L3, L5, L10 and L11. **Dead** ones are L1 and L4.

#### Statement status (first match wins)
| # | Condition | Statement status |
|---|---|---|
| ST0 | Any established link **for** and any established link **against** | **Inconsistent: reopened** (P7 trigger). Both are listed. |
| ST1 | An established **against** link at Formally verified proof | **Disproved** |
| ST2 | An established **against** link at Formally verified proof (fidelity unchecked) | **Disproved (fidelity unchecked)** |
| ST3 | An established **against** link at Refereed or Computer-assisted proof | **Disproved (refereed or computer-assisted)** |
| ST4 | An established **for** link at Formally verified proof | **Formally verified** |
| ST5 | An established **for** link at Formally verified proof (fidelity unchecked) | **Formally verified (fidelity unchecked)** |
| ST6 | An established **for** link at Refereed or Computer-assisted proof | **Proved (refereed or computer-assisted)** |
| ST7 | **Any** weak link, for or against | **Proof claimed, not established.** Every link is listed with its status. |
| ST8 | Links exist, and all are dead | **Open (earlier proofs failed or did not establish it as stated)** |
| ST9 | No links | **Open (conjecture)** |

**Shown with S1–S6:** any other links, such as a claimed disproof next to a refereed proof, are listed as "also on record".

**Literature marker:** for ST7–ST9, the literature-search marker is **appended** to the status, as *searched (date, sources)* or *literature not checked*. It never replaces the status (MA7). ST0 also lists any non-established links as "also on record".

**Coding rules (MA5; they cannot be enforced in code):**
- a formalization of an argument either repairs a gap or records it as statement-changing; it never leaves it unrepaired (A4);
- a fidelity failure caused by the **proof** needing an extra hypothesis is recorded as a statement-changing gap, not only as fidelity failed (A2);
- **every documented objection is coded before any status is displayed (A3).** Example: if the Scholze–Stix objection to IUT were left uncoded, the refereed IUT papers alone would show abc as "Proved (refereed)", which shows why A3 is required.

**Repairs in practice:**
- When a formalization finds gaps but the statement survives, the gaps are recorded as repaired. The link reaches L6 or L7.
- When the statement needed a stronger hypothesis (S2's edge case):
  - the original statement's link is L4, so the original shows ST8, or ST7 if other weak proofs exist;
  - the **corrected** statement's link reaches L7 (or L6 after MA1) **only if the correction itself was formally checked and covers every statement-changing gap**. In that case it shows ST5 (or ST4). An unchecked correction shows as Claimed proof (ST7).
- **Disproved always needs established evidence against (ST1–ST3)**, never just a failed or corrected proof.

### 3.1a Gap-coding amendment GC-1 (v1.2; D62, ratified by the user 2026-11-23)
Full amendment file (`claude/math-gc1-coding-amendment.md`, including the §6 GC-1.1 completions) sha256 `e868acae58a228b9b982c0e056a74116adfb2472f7c2a6ace6b41b1a65d68396` (v1.3 correction: the v1.2 inner hash `ef933c8e…` was taken before the GC-1.1 completions were added and did not match the text). The full text and audit trail are in `claude/math-gc1-coding-amendment.md`. **It takes effect for coding only after its validation on 20 new reports passes** (GC-1 §4). `math_status.py` is unchanged.

**G-1, the fatal-confirmed definition (replaces the §2.1 text within its scope; includes the GC-1.1 completions):**

> **Scope (N3):** G-1 applies **only when the argument cannot establish the printed statement** (for example, the printed statement is false or cannot be proved as printed). An ordinary statement-changing gap stays statement-changing under the existing definition, **whether or not its correction has been checked** (§3.1, "Repairs in practice": an unchecked correction only limits the corrected link's status).
>
> **fatal-confirmed:** within that scope, the argument cannot be made to establish the printed statement **or any corrected version of the same result established under G-1(b)**; and this is accepted by the authors or independently confirmed (**formally or by a second coder**, as in the current text; N4).
>
> **(a) Same result (audit #6):** a corrected version is the *same result* only if it keeps the printed conclusion's type and objects, and differs only by:
> - added or strengthened hypotheses; or
> - a weakened constant, bound, range or rate in the conclusion.
>
> A version that drops or replaces the main conclusion is not the same result. **Quantifier weakenings** ("all" to "almost all" or to "sufficiently large") count as a weakened range. **The reason field records exactly what was weakened**, so that a sensitivity analysis coding these cases as F remains possible (GC-1.1).
>
> **(b) Established (audit #4):** the corrected version has been checked at the **refereed** or **formal** level, using the profile's levels for a correction's own check (§3.1). An unchecked authors' erratum, or a verification that is *conditional* on undischarged assumptions, does not count. Evidence is judged **as of the coding date** in retrospective (MA5-style) coding, and **as of H** for M1 resolutions (N11). The date is recorded.
>
> **(c)** If (a) and (b) hold, the gap is **statement-changing**, even if the printed statement is false.
>
> **(d) Interactions (audit #5):**
> - A withdrawn result is **F** regardless of any correction (L1 is unchanged).
> - If a corrected version exists but whether the original argument is fatal is contested, the gap stays **fatal-disputed** (L3). **(d) takes precedence over (c)** (GC-1.1).
> - A correction that is itself contested is not "established" under (b) (GC-1.1).
>
> **(e)** Statement falsity is recorded separately, as an against link (§3.1). It is never inferred from a gap code.

**GC-1a, misprints (added after A2–A4):**

> **Rule A2 takes precedence.** If the proof as printed needs a hypothesis or reading that the statement lacks, that is a statement-changing gap under A2. It is never a misprint.
>
> A **misprint** is a **typographical substitution**: a wrong symbol, index, subscript or letter, or a transposed term. It counts only if the intended reading is fixed by the paper's **own printed text other than the proof**, meaning its other statements or definitions, or by the authors' own published erratum. What the proof uses, on its own, is never enough to establish a misprint.
>
> A misprint is coded **repaired**, with "misprint" in the reason. Otherwise it is **statement-changing**.

**Primary and sensitivity analyses (N2):**
- **Primary:** misprint cases coded repaired. Outcome R means "survives as printed, reading recorded misprints as corrected".
- **Sensitivity:** the same cases coded statement-changing. **They are never dropped,** since dropping them would be an exclusion after the freeze.

**Fidelity (N2):** for a recorded misprint, the §2.4 fidelity review compares the formal statement with the published statement **read with the recorded misprint correction**. A formal statement that matches that reading is **not** a variant, so L6 or L7 can apply. This is an explicit, logged exception to §2.4, limited to misprints that meet the rule above. The exception and the misprint reading are recorded in the fidelity record, so a reader can see them.

**GC-1b, unrepaired versus not yet formalized:**

> - **A gap in the paper:** the report states that the paper's argument is wrong, unjustified or incomplete at that step.
> - **Not yet reached:** the report says only that the formalization has not covered the step, without claiming a defect in the paper. **This is not a gap.** It is recorded in the summary row as "not yet formalized".
> - **If the report is silent** about a defect, there is no gap.
>
> **Packet rule 1 is rewritten** (audit #14): "**Unrepaired** is used only for a documented gap in the paper's argument that the report leaves open. A completed formalization of the argument never leaves a gap unrepaired: each gap it found is repaired or statement-changing. Steps the formalization has not reached are not gaps."

**G-4, outcome derivation:**

> - **Outcome R requires that the main target is formally established.** That means sorry-free and completed, as the report documents it. In M1 this is the same as link L6 or L7 (`math_status.py`, unchanged).
> - **If the main target is not formally established,** and no F, D or S gap applies, the outcome is:
>   - **U** in retrospective (MA5-style) coding;
>   - **N** ("not finished by H") in M1. U maps to N.
> - **A report with no gaps is R only if** the main target is established. Otherwise it is U or N.
>
> M1 design §3 already derives R only from L6 or L7. This rule makes the same requirement explicit for gap-based derivation, and for the MA5-style derived outcome.

### 3.1b GC-1 validation result and GC-1.2 (v1.4; D65, ratified by the user 2026-11-23)
- **GC-1 (§3.1a) FAILED its held-out validation** (`claude/math-gc1-validation-results.md`): outcome agreement 15/19 against a bar of 18. **§3.1a is therefore not in effect.** Coding uses packet v1.0 (MA5, κ 0.81).
- **GC-1.2** (`claude/math-gc1.2.md`, ratified-text sha256 `6391cb00b8de5240cf7217345f16e2f36261a3b0dea378f9063ec80a2dfbb5c5`) revises G-1, G-2 and G-4, adds GC-1c, amends A2 and packet rule 3, and pins documents (packet v1.2). **It takes effect only if its re-validation on 20 new reports passes** (GC-1.2 §3). If it does, §2.1, §3.1 and §3.1a are replaced by the GC-1 text as amended by GC-1.2, in a logged v1.5 entry. **If it fails, the pre-committed default is to withdraw GC-1** and keep packet v1.0, disclosing v1.0's known weaknesses.
- `math_status.py` is unchanged.

- **v1.5 note (D66, 2026-11-23; recorded 2026-11-24):** the rule work is **paused**. GC-1.2 is ratified text but is **not in effect**, and its literature re-validation is **held, not run**. GC-1.3 (the outcome measures the paper's own argument) is a draft. The final rules are to be designed once, using pilot evidence, and validated on ≥ 20 other captains' Prove2Me reports, sealed unread before M1 v4.3 (A1 §5). Pilot coding is descriptive, under packet v1.0. `math_status.py` is unchanged.

### 3.2 Conjecture credence
- **Credence is defined only for statements at ST7–ST9** (a statement already proved or disproved has none). Each credence is stated as **P(statement is true)**, never as "P(true | resolved)".
- **Where a test grades it,** the forecast is P(resolved true by horizon H) and P(resolved false by horizon H), both registered, because counterexamples tend to arrive faster than proofs (N-M4).
- **When numbers may be shown:** only after a sealed test of that stratum exists (§4). Until then, the evidence profile is shown and no number.

### 3.3 Contests
**Interim rule (until MA9's go-live conditions in §3.3a are met; MA9 itself was decided by D80):** no mathematical contest record is created. Disputes are handled by gap coding, with the **disputed-fatal cap** (L3), so no single coder's call decides a dispute.

**Logged deviation:** spec §5.3 step 1 opens a contest whenever credible evidence exists on both sides. The interim rule delays that for mathematics until MA9, routing and Judge recertification are done.

**Proposal for MA9 (the auditor's recommendation):** mathematical disputes become a **subtype of empirical**, called `empirical-mathematical`. That keeps the four contest types (empirical, methodological, definitional, value-laden) and gate (c)'s protection for value-laden claims. Before the first live ruling:
- new criterion kinds need Judge recertification on planted cases;
- the type gate needs new routing cases (as D41 and D43 required).

**Settlement criteria and splits:** superseded by the ratified MA9 text (D80, `claude/math-ma9.md` §3–§4).
- One fidelity review applies to every route, with a neutral reviewer named at registration.
- There is no Judge tie-break; silence never passes.
- Disagreement credits the result to the variant node and leaves the contest open.
- **No numeric split:** the formal evidence is listed by side (N-M7 option (iii)), a logged deviation from D45 and spec §8.

The earlier sketch, which let an unanswered review count as passed and allowed a Judge tie-break, is withdrawn. **Kept from it, unchanged (D47, D80 §6):**
- **Null outcomes:** "not yet formalized" settles nothing. Only a completed formalization meeting all conditions settles.
- **Asymmetry, stated openly:** "against" settles only by refuting the step (or by a formal proof that the step needs a hypothesis that is false in the application). "For" settles by proving the step. This follows from what can be proved, because a missing proof cannot be shown formally. D47 accepted the same kind of claim-driven asymmetry.

**Worked example (not a registration).** Mochizuki's IUT proof of abc is a **proof node**. Its link to abc depends on how the Scholze–Stix objection is coded under MA5:
- if coded **fatal-disputed** (the authors contest it), the link is L3, Proof with a disputed fatal gap;
- if coded unrepaired, it is L5;
- once MA9 is live, a contest on the step at issue makes the link L2. But L1, L3 or L4 take precedence. While the objection is coded fatal-disputed, the link stays L3, and the renderer shows the open contest separately (§3.3a).
- **Statement status:** under ST7, the abc conjecture itself is **Proof claimed, not established**, and the link is listed with its status.
- **The crux, as Project LANA states it:** deriving Corollary 3.12 from Theorem 3.11. LANA "has not made a final judgment" (report of 2026-07-16).
- **Mochizuki's own Lean effort:** it so far gives skeletal code for the last sub-step of his decomposition ("3.11.5 ⇒ 3.12"), with many black boxes (S10).

**Definitional disputes** dissolve by splitting, as before.

### 3.3a MA9 amendment (v1.6 draft; D80)
**The §3.3 interim rule stays in force.** It is replaced by the MA9 rules (`claude/math-ma9.md` §3–§4) only when **all** of these hold:
- the MA9 §5 tests pass: the gate (c) re-run, definitions v1.6 with the engine, the Judge recertification, the renderer scope audit and the criteria audit;
- **MA1, MA5 and MA7** are resolved.

**Status-rule amendment, in code (`math_status_v16.py`, `4b72509e…`):**
- **Contest location:** a contest sits on a **(proof, step id)** pair. `contested` holds the contested step ids, replacing the proof-level `contest_open`, which is kept only as a read-only property.
  - A link uses every step of its proof unless `uses` names a subset.
  - A corrected link's `replaces` removes the steps its correction replaces.
  - A link shows Contested proof (L2) only if a contested step is among the steps it uses and does not replace.
- **New precedence:** Failed (L1), then disputed fatal gap (L3), then does not establish (L4), then Contested (L2), then open gap (L5).
  - **Opening a contest can never lift a link above "does not establish"**, so a statement can never move from ST8 to ST7 by opening a contest. It can move from ST7 to ST8, which is the intended, cautious direction.
  - **L3 moving above L2** is a by-product, not a D80 requirement. It is the only order that puts L1 and L4 before L2 and leaves every no-contest status unchanged.
  - **When L1, L3 or L4 applies, the link status does not show an open contest.** The renderer must show the open contest separately.
  - **With no step records and no contests, every status is identical to the ratified v1.5 rule.** The self-test checks this against `math_status.py` itself, under both MA1 settings (147,456 cases).
  - **Disclosed:** under the earlier draft's enumeration, per MA1 setting, 4,864 of 73,728 contested cases show "does not establish" and 9,216 "disputed fatal gap" instead of "Contested proof". None rises.
- **Step records:** `step_gaps` holds (step id, code, ref) records, one per step, with codes `repaired`, `repaired_formal` (ref = the formalization's hash), `unrepaired` and `fatal_disputed`.
  - **A record affects only links that use the step and do not replace it.**
  - Proof-level codes from v1.5 records still apply to every link.
  - `locate()` puts a proof-level `unrepaired` or `fatal_disputed` code on a named step. **The proof-level code is removed only when a logged MA5 coding record says it covered just that step (R1).** Without one, the step record is added and nothing displayed changes (tested).
  - **Under the interim rule,** step records are created only by MA5 coding, each with its coding record. No contest can exist.
  - **A contest can open only on a step with a record.**
- **Links:** `uses` (a non-empty set of step ids, or all) and `replaces` (corrected links only, within `uses`).
- **Settlement:** `settle(proof, step, outcome, ref)`:
  - **for:** `repaired_formal`, with the formalization's hash;
  - **against:** `unrepaired`, never Failed by itself, and **never below the step's existing gap rank**: a `fatal_disputed` step stays `fatal_disputed` (R2; **to be confirmed by the user at ratification**, since it narrows D80 §4's wording). A fatal coding of the whole argument is left to MA5;
  - **methodological for:** the step's code is **unchanged**, and (step, validation record id) is added to `validated`;
  - **methodological against:** `unrepaired`;
  - **independence:** `unrepaired`;
  - **independence with an added axiom or hypothesis:** a new statement-changing gap is added, with its id checked for clashes, and the step is marked resolved into it (`repaired`, ref `sc:<id>`), so it can be reopened. There is no unrepaired code as well. **Disclosed (R5):** statement-changing gaps are proof-level, as in v1.5, so the new gap applies to every link, including links that do not use the step.
- **Reopening:** `reopen()` reopens a contest and keeps the step's current code. Earlier outcomes and their history live in the contest record.
- **Validation:** refuses two records for one step, `replaces` outside `uses`, an empty `uses`, a contested step with no record, a clash between a step id and a statement-changing gap id, and a `validated` entry with no step record or record id (R8).
- **A second registered axiom set gets its own statement node.** That is enforced by the record schema, not by this module. The MA9 §5.3 recertification plants a second-axiom-set route (F13).
- **Self-test:** `python3 -I math_status_v16.py` reports PROBLEMS: 0. It first checks the ratified `math_status.py`'s hash (`25cad02a…`). It covers:
  - (0) the ratified module's own invariants and named cases (S2, B1, A1), its 4,600 statement cases, the statement rule's identity with the ratified one, the v1.5 link invariants over the new step cases, and "`locate()` without a coding record changes no status" (R3, R1);
  - (1) equivalence with the **ratified module itself** on 147,456 legacy cases, under both MA1 settings;
  - (2) 5,508 cases over the new fields: two steps, every step code, contests on any recorded subset, and links with every `uses` and `replaces`. The invariants checked are: a contest never lifts a non-establishing link; unused or replaced steps never affect a link; opening a contest never raises a link; "settled against" is never Failed; a methodological "for" never raises a link;
  - (3) named cases for F1–F4 and P1;
  - (4) the malformed-input refusals.
- **Review condition (MA9 §4):** this code and its self-test pass a Profile Auditor review **before** any MA9 §5 protocol that depends on them is sealed.

## 4. Grading signals

| Object | Signal | Required controls before any test is sealed |
|---|---|---|
| **Proof status: does Refereed proof survive formalization?** (test M1) | The proof node's status after a completed formalization:<br>• statement survives (link L6 or L7);<br>• statement corrected (L4 on the direct link, with a corrected statement);<br>• fatal (L1); disputed fatal (L3) is reported separately. | • **Cutoff:** registration before the first public blueprint commit, Zulip thread or gap note touching the target. *(v1.1: what "touching" means for an announcement is set by clarification C-1; a separate secondary stratum is allowed by amendment M-A1. See §4a.)*<br>• **Errata:** exclude papers with errata, corrigenda or public error reports from before the forecaster's training cutoff.<br>• **Horizon:** pre-register it, and score "not finished by H" as its own outcome, not as missing data.<br>• **Model family:** state when the formalizer and the forecaster share one.<br>• **Base rate:** state the expected rate of "statement corrected" and run a power calculation (MA3, N-M5). |
| **Statement status** (status-level test, N-M9) | Does a statement's status predict how it resolves? For example, do Proof-claimed statements resolve true more often than Open ones? Is a corrected statement's original later disproved? A status-to-probability map, as with F_status in science. | The same controls; registered alongside M1. |
| **Conjecture credence** | P(true by H) and P(false by H) against outcomes. | • Strata by tractability (MA4).<br>• Exclude, or put in a separate stratum, literature-found resolutions from before the forecaster's cutoff (leakage, N-M5).<br>• Truth-dependent timing (N-M4). |
| **Claimed proof** | **Primary:** reaches Formally verified proof, or is refuted by a verified counterexample or formal disproof. **Secondary:** refereeing (N-M10: acceptance is source-confounded). | A pre-registered horizon. |
| **Literature-search marker** | Rate at which "searched, open" items are later found already solved. | Descriptive at first. |
| **Fidelity** | Rate at which a later review finds a fidelity failure in items marked passed. | A planted-fault audit first (MA1). |

**Credence rule (D51) as applied here:** numbers appear only after a sealed test of that kind exists. That is **stricter** than science practice, where finding credence was shown before gate (a) resolved (N-M13). Lineage counts are for display only (D35).

**Who forecasts:** an Evidence Auditor. Not the Profile Auditor (who audits the profile) and not the Architect (D52).

## 4a. Amendments to the M1 controls (v1.1; D61, ratified by the user 2026-11-23)
Both items were drafted by the Architect, audited by the Profile Auditor (`claude/math-test-m1-design-audit-v1.md`, `-check-v2.md`, `-check-v3.md`) and ratified by the user. The M1 test itself is specified in `claude/math-test-m1-design-v3.1.md`.

**Clarification C-1** (ratified text, sha256 `6d153fcc42539446058bc56ae3f75f98435805feff0e9a5d18dab84d0e869dac`):

> **Clarification C-1 (§4 M1 row, primary cutoff).** A public post announcing a formalization project does **not** "touch" a target under the cutoff if:
> - it identifies the paper, and may identify statements only by **label or number** (for example "Theorems 1.2 and 4.1");
> - it contains **no** formal statement, and no discussion of any statement's proof or hypotheses;
> - it contains **no claim or suspicion about the correctness** of the paper or of any of its results.
>
> **Any later post** in the same thread, or elsewhere, that carries such content **does** touch the targets it concerns, so registration must come before it. The content of the announcement and the thread state are archived at registration.

**Amendment M-A1** (secondary stratum):

> **Secondary stratum only (M-A1).** A target may also be registered from an existing blueprint if, at a recorded snapshot commit, all of these hold:
> - (a) it corresponds to a numbered statement printed in the refereed version;
> - (b) its node is **statement-only**: the proof is a bare placeholder, with no partial proof;
> - (c) its formal statement is unchanged since its first commit;
> - (d) a reviewer who did not write it has recorded it as textually faithful to the printed statement;
> - (e) no issue, PR, gap note or **thread** references it. A thread is any public forum post: Zulip, GitHub discussions or issues, or a mailing list.
>
> **All other M1-row controls apply to this stratum.** Such targets form a separate stratum. They are reported separately, are never pooled without saying so, and **cannot fire TM1.** Stratum C keeps the C-2 cutoff. *(v1.3 wording; v1.1 read "The primary stratum keeps the original cutoff.")*

**Interpretation of TM1 (logged here, H9):**
- **First limb:** M1 reads "no better predicted" as "**not shown better, and shown not meaningfully better**". At a confirmatory look, the 97.5% BCa interval for the Brier skill score must have its lower bound ≤ 0 and its upper bound < δ = 0.15.
- **Second limb:** "statuses carry no ordering" fires on "no ordering shown" (an equivalence test within ±0.15) or on a significant reversal. The details are in M1 v3.1 §5–§6.

## 4b. Stratum C as the primary M1 stratum (v1.3; D63, ratified by the user 2026-11-23)
Drafted by the Architect, audited by the Profile Auditor (`claude/math-test-m1-design-audit-v4.md`, "revise narrowly", 3 BLOCKING; `claude/math-test-m1-design-check-v4.1.md`, "ratify with listed amendments", 0 BLOCKING) and ratified by the user with M1 design v4.2 (`claude/math-test-m1-design-v4.2.md`). **The user's reversal** of the v1.1 rule that only the announcement stratum (P) can fire TM1 is logged here as a dated decision.

**Condition C-2 (§4 M1 row, stratum C):**
> For stratum C, registration and external timestamping precede the project's posting of the target to the formalization platform, **and** the registration search finds no earlier touch (§4 cutoff, as clarified by C-1) and no public error report.

**Primary stratum:** the primary M1 stratum is **C**. Only C can fire TM1's first limb. Strata P and B are secondary and can neither rescue nor veto a C result.

**Stratum-C fidelity exception (logged exception to §2.4, parallel to GC-1a's misprint exception):**
> For stratum C, a formal statement that matches the printed statement **plus** the listed cited-result hypotheses and well-definedness conventions defined in M1 v4.2 §10.1–§10.2 is **not** a variant. Links L6 and L7 then mean "established relative to the listed results", and the fidelity record lists every such hypothesis and convention, each checked against its source. A hypothesis needed for the statement to be true is never a convention; it is a statement-changing gap under A2.

## 5. What this profile does not grade
- **Importance, depth or beauty:** mapped and value-laden.
- **Priority or novelty:** recorded as provenance only.
- **A formally verified result relative to its formal statement:** kernel trust and axioms are stopping points (§6).
- **Expert belief as such.**

## 6. Interfaces with the shared core
- **Stopping points (P7):** the kernel and the axioms used, recorded for each proof assistant. **Reopening triggers:** a kernel or library soundness bug, an inconsistency in axioms or definitions, or rule S0.
- **Firewall (P4):** it applies unchanged. As in science §7, Phase 4 is not built, so today it holds because no stakes data exist.
- **Scope (D29):**
  - proof statuses never stand in for statement statuses (§3.1);
  - a corrected statement's status is never shown as the original's;
  - a programme or field gets no status.
- **P5:** AI-generated and human proofs are scored on content only.
- **P6/D35:** lineage counts are shown, and never weighted into credence.
- **P8:** fidelity sign-offs and expert checks are logged as human interventions.
- **P10:** heuristic models used as priors are audited as priors.

## 7. Open issues
| # | Issue | Class (audit v0) |
|---|---|---|
| MA1 | Design and planted-fault audit of the fidelity check | RESOLVE-BEFORE-USE (before anything is shown as Formally verified with fidelity passed; until then, "fidelity unchecked") |
| MA2 | Library drift between versions | CAN-WAIT (interim: record versions; reopen statements whose definitions changed) |
| MA3 | M1 target list: selection, staged leakage, errata, censoring, shared model family | RESOLVE-BEFORE-USE (before M1) |
| MA4 | Conjecture strata by tractability | RESOLVE-BEFORE-USE (before conjecture credence) |
| MA5 | Gap coding (repaired / statement-changing / unrepaired / fatal): rule plus inter-rater check | RESOLVE-BEFORE-USE |
| MA6 | Claimed-proof volume: intake triage and horizon | CAN-WAIT (no intake yet) |
| MA7 | Literature-search depth: what counts as searched | RESOLVE-BEFORE-USE |
| MA8 | Independence of formal checks (same model family is not independent) | CAN-WAIT |
| MA9 | Mathematical contest type | **RESOLVED (D80):** `empirical-mathematical`, a subtype of empirical; see `claude/math-ma9.md`. Not live until §3.3a's conditions are met |
| MA10 | Constructive vs classical axioms: show, do not grade | CAN-WAIT. **Routing part settled (D80):** "provable from X" is contestable; "X should be accepted" is value-laden (mapped) and a P7 stopping point |
| MA11 | The §0 judgment on resolution speed is unsourced | CAN-WAIT (measure it once M1 runs) |
| N-M4 | Horizon and truth-dependent resolution for conjecture credence | RESOLVE-BEFORE-USE (handled in §3.2 and §4; to be sealed in the test) |
| N-M5 | Leakage controls | RESOLVE-BEFORE-USE (§4) |
| N-M7 | Numeric split for math contests | **RESOLVED (D80):** no number; the formal evidence is listed by side. A logged deviation from D45 and spec §8 |
| N-M8 | Even-handed fidelity review in contest criteria | **RESOLVED in text (D80):** one review for every route, a neutral named at registration, no Judge tie-break, the variant credit. It is the MA1 procedure, so it becomes operative with MA1 |
| N-M9 | Status-level test | RESOLVE-BEFORE-USE (before numeric display) |
| N-M10 | Claimed-proof outcome is source-confounded | Resolved in §4 (formal outcome primary) |
| N-M13 | Credence rules differ between profiles | CAN-WAIT (noted in §4; note to add to the science profile) |

## 7a. What would count against this profile (set by the user at ratification)
| Trigger | Consequence | Set by human |
|---|---|---|
| TM1: Test M1 shows Refereed proof's survival no better predicted than the naive repair-rate baseline, or the status-level test shows statuses carry no ordering. *(v1.3: only stratum C, the primary M1 stratum, can fire the first limb; see §4b. Interpretation logged in §4a.)* | Revise §2.1 and §3.1 | User, 2026-11-21 |
| TM2: the fidelity-audit failure rate among items marked passed exceeds **5%** | Revise MA1, and suspend the "passed" label for affected items | User, 2026-11-21 |
| TM3: a kernel or library soundness bug | P7 reopening of affected items (automatic) | User, 2026-11-21 |
| TM4: conjecture calibration fails in any stratum | Suspend numeric credence for that stratum | User, 2026-11-21 |
| TM5: a scope or fairness audit fails | Treated like any trigger | User, 2026-11-21 |

**For any trigger:** the Architect drafts an amendment, the Auditor reviews it and the user ratifies it.

## 8. Proposed first tests (not registered)
**Test M1 (formalization survival) plus the status-level test.**
- **Targets:** freeze about 40 statements with refereed proofs that are targets of announced formalization efforts, under the §4 controls.
- **Forecasts:** an Evidence Auditor forecasts P(statement survives as stated) within a horizon. Baselines: the naive repair-rate model and the status map.
- **Prerequisites:** MA3 and MA5. The expected base rate and power are stated before sealing.

## 9. Sources (current-state facts; checked 2026-11-20 and confirmed by the Profile Auditor)
- **S1:** Anthropic, "Formalizing Fermat's Last Theorem" (2026-09-04), and the project README:
  - an end-to-end Lean proof in about 11 days, using only Lean's three standard axioms;
  - the top-level statement checked against Mathlib's;
  - parts of the Imperial FLT project and flt-regular reused.
- **S2:** arXiv 2609.19814 (Lu, Deng, Zhu, Ji), the LIDT core of MIP\* = RE:
  - 25 gap notes, all resolved;
  - the hypothesis 0 < k added at an edge case;
  - a side condition tightened from k ≥ md to k ≥ 400md;
  - the final conclusion unchanged.

  This shows the printed statement was **not established as stated**, not that it was false.
- **S3:** arXiv 2608.18024 (Enomoto): formalization of the author's own AI-assisted preprint revealed gaps that AI reviews had missed. This is pre-publication, so it is not evidence about refereed work.
- **S4:** arXiv 2606.13306 (Garg, EconCSLib): minor bugs, an unstated assumption, and correct statements with incorrect proofs; no fatal errors so far.
- **S5:** Alpöge's explicit counterexample to the Jacobian conjecture in dimension 3 (announced 2026-07-20; the announcement credits the model "Fable", reported as Fable 5). AFP entry *Jacobian_Counterexample* (2026-07-20): an independent Isabelle/HOL verification, extended to every dimension ≥ 3, and itself using AI for proof engineering. The two-dimensional case remains open.
- **S6:** OpenAI (2026-05-20): Erdős's unit-distance upper-bound conjecture disproved by an existence construction, checked by external mathematicians with a companion paper. Not refereed or formalized as of the audit, so under this profile it is a Claimed proof (expert-checked) of the negation.
- **S7:** arXiv 2608.29595 (Sra), which cites the AIMath website's list (506 entries as of 2026-08-25; inclusion criteria unknown). Not a base rate or sampling frame.
- **S8:** arXiv 2601.22401 (Feng, Luong et al.):
  - 700 "open" Erdős problems screened;
  - 13 addressed, 8 of them with earlier literature solutions;
  - the database tracked 1,179 problems, 41% of them solved.

  The 8-of-13 figure is conditioned on a solution being found.
- **S9:** arXiv 2606.29493: a taxonomy of fidelity faults, based on benchmark datasets.
- **S10:**
  - Project LANA interim report on IUT (last updated 2026-07-16): no final judgment; the crux is Theorem 3.11 ⇒ Corollary 3.12.
  - Mochizuki, "On the formalization of IUT: a preliminary progress report" (April 2026): skeletal Lean code for "3.11.5 ⇒ 3.12", with many black boxes.
  - New Scientist coverage (date not verified by the auditor).

## 10. Changes

### v1.4 → v1.5 (2026-11-24; recording D66, no rule change)
- §3.1b records the rule pause and the A1 validation route.
- **Re-seal** replaces `ad8fbf77…`.

### v1.3 → v1.4 (2026-11-23, D65; user ratification)
- GC-1 validation FAILED; §3.1a is not in effect. GC-1.2 ratified, effective only after re-validation (§3.1b).
- **Re-seal** replaces `af2acac4e8db4e89e95ac1230be558e6545ca07343df2dc61f5727efb17ded9b`.

### v1.2 → v1.3 (2026-11-23, D63; user ratification)
- **C-2** cutoff condition for stratum C; **stratum C is primary** and the only stratum that can fire TM1's first limb (user's reversal of the v1.1 rule, logged); **§2.4 fidelity exception** for stratum C (§4b).
- M-A1 wording updated to name C-2; §7a TM1 note updated.
- **Correction:** the §3.1a inner GC-1 hash replaced (the v1.2 value predated GC-1.1).
- `math_status.py` unchanged.
- **Re-seal** replaces `e40b67109d59487518a31d1ec8ded3992bc2f03b4adfcbde43d7683f6f03356b`.

### v1.1 → v1.2 (2026-11-23, D62; user ratification)
- **GC-1:** the §2.1 fatal-confirmed definition changed (G-1), plus coding rules GC-1a (misprints) and GC-1b (unrepaired), and the outcome-derivation rule G-4 (§3.1a).
- **GC-1.1 completions** included (precedence of (d) over (c); contested corrections; quantifier weakenings; the §2.1 statement-changing text; unrepaired stays unvalidated).
- **Effective only after the held-out validation passes.**
- **`math_status.py` unchanged.**
- **Re-seal** replaces `6698e6cfd902c50d4033d8ad81634ea2cba0f0ddbb114bf55e45d176afff3807`.

### v1 → v1.1 (2026-11-23, D61; user ratification)
- **C-1:** clarifies what "touching" a target means for an announcement post (§4a). Ratified text hash `6d153fcc…`.
- **M-A1:** adds a secondary M1 stratum from untouched blueprint nodes; that stratum cannot fire TM1 (§4a; §7a note).
- **TM1 interpretation logged** (§4a).
- **The primary cutoff text is unchanged.**
- **One combined re-seal** replaces `e224d07237239a43f0b3d652d96183b933fc278bbaabe3f2f8e9df050be37a15`.

### v3 → v3.1 (audit v3)
- Statement rules renamed ST0–ST9, so they no longer collide with sources S1–S10.
- Test M1 uses the v3 link labels.
- The rules doc is renamed.
- The code validates inputs: meaningless records are rejected.

### v2 → v3 (audit v2)
- **B1:** per-link coverage and the correction's own check; original evidence never transfers.
- **A1:** fidelity is a link attribute.
- **A2, A3:** coding rules.
- **A4:** the MA1 interim rule enforced in code.
- **A5:** separate computation thresholds for proofs and counterexamples; ST0 "also on record"; literature-marker wording.

### v1 → v2 (re-audit v1)
- **R1:** proof status computed per link, with a direct or corrected relation.
- **R2:**
  - complete rules, now stated as code and checked by enumeration;
  - the disputed-fatal cap (L3);
  - the gap exclusion applies to fidelity-unchecked formal proofs (L4 comes before L6 and L7);
  - S7 uses "any" and covers the against side;
  - Disproved (fidelity unchecked);
  - S0 widened to any established pair.
- **Non-blocking items:**
  - IUT example corrected;
  - `computational_reproduced` added, with its remainder rule;
  - default `claimed_proof` code;
  - `formal_independent` counts as a fidelity review;
  - counterexample fidelity;
  - counter-evidence listed alongside established statuses;
  - the literature marker is appended, not substituted;
  - contest tie-break;
  - "compiles" replaced by the `formal_verified` conditions;
  - the §5.3 deviation logged;
  - S5 attribution.

### v0 → v1 (audit item → change)
- **N-M1:** statement and proof nodes (§3.1).
- **N-M2:** complete ordered rule sets for proof and statement status. "Unproven as stated" is separated from Disproved. Fidelity has three outcomes. The literature marker now applies to all open statuses. A route for disproof by proof (§2.3, rule S2). A rule for inconsistency (S0). `computational` and `formal_independent` now have effects.
- **N-M4, N-M5, N-M9, N-M10:** the §3.2 horizon; §4 controls; the status-level test; the Claimed proof outcome.
- **N-M7, N-M8, MA9:** §3.3 interim rule and sketch.
- **N-M3:** §7a trigger options, to be filled in by the user.
- **N-M11:**
  - S3, S6, S7, S8 and S10 corrected;
  - "fourth type" and "unchanged" removed;
  - "same discipline" replaced with an explicit difference;
  - v1.2 naming fixed;
  - the §0 judgment labelled.
- **N-M12:** P6/D35, P8 and P10 lines in §6; the forecaster named.

---

## Change log v1.5 → v1.6 (draft)
- **§3.3a added** (D80, the MA9 amendment).
- **Status code:** `math_status_v16.py` will replace `math_status.py` as the authority on ratification.
- **L-table:** a precedence note added, and the L2 row reworded.
- **§3.3:** the interim-rule heading updated; the settlement sketch and the splits paragraph replaced by a pointer to D80; the worked example's L2 line corrected.
- **Open-issue table:** MA9 and N-M7 resolved; N-M8 resolved in text; MA10's routing part settled.
- **Review:** `math-profile-v1.6-audit.md` ("revise": 2 BLOCKING, 6 MAJOR, 5 MINOR; F1–F13 applied).
- **Known D57 gate exceptions** in unchanged v1.5 text: one word in the firewall bullet in §6, and one table header in §7a (G2). Neither describes what depends on a verdict. Only the gated review packet goes to firewalled reviewers (R7).
- **Re-check (v1.6 rev):** "revise narrowly" (R1 BLOCKING; R2–R8), applied. R2 was confirmed by the user at ratification.
- **Final check:** "ratify with listed amendments" (0 BLOCKING, 0 MAJOR; G1, G2 applied: the R2 invariant and the full `locate()` check added to the self-test).
- **Ratified:** 2026-11-24 (D82).

## Ratification (user, 2026-11-24, ~13:25 PT; D82)
- **The user's words (question window):** "Keep the higher rank (Recommended)" (R2); "Ratify now (Recommended)".
- **Options shown:** for R2, keep the higher rank, or D80's literal "coded unrepaired"; ratify now, or not yet. Recommendation: the first of each.
- **Reason:** no reason given. The user chose the recommended options.
- **Status code:** `math_status_v16.py` sha256 `4b72509ee3a32c7b0c52a51b23a55af5fb671231e0f13129f41ec014095517d8`, reproduced in `claude/math-profile-v1.6-status-rules.md`. The ratified v1.5 file `math_status.py` (`25cad02a…`) is kept, and the v1.6 self-test imports it for the equivalence checks.

**SEAL (v1.6):** sha256 of everything above this line: `3e6f129fd58817e9b366c60babfdeddb853d3e099e35077c138f52148573b052` · 2026-11-24. It replaces the v1.5 seal.
