# MA9: The Contest Type for Mathematical Disputes

**Status:** **RATIFIED by the user, 2026-11-24 ~12:40 PT (D80).** Text v4 by the Architect.
- v2 applied the first audit ("revise", 3 BLOCKING, 11 MAJOR, 7 MINOR).
- v3 applied the v2 re-audit ("revise, narrowly": 1 BLOCKING, 6 MAJOR, 7 MINOR).
- **v4 applies the v3 narrow check** ("ratify with listed amendments": 0 BLOCKING, 2 MAJOR, 4 MINOR, plus 2 MINOR leftovers; `claude/math-ma9-audit.md`, "Narrow check (v3)"). §8 maps every finding.
- **For:** a narrow re-audit, then the user's ratification.
- **Resolves:** MA9; the routing part of MA10 (M1); and N-M7, by the user's choice.
- **Urgency:** low. No mathematical contest is pending, and M1 does not need MA9.
- **Live effect:** nothing changes in live operation until §5's tests pass **and MA1, MA5 and MA7 are resolved** (M5, n1).
  - "MA5 resolved" means the final gap-coding rules (the paused D66 work) are ratified and validated. **In practice MA9 cannot go live before M1 v4.3** (n2). Until then the interim rule (profile §3.3: no mathematical contest records; disputes carried by gap coding with the L3 cap) stays in force.

## 1. The problem
- **Contest types (spec §5.3; sealed type-gate prompt):** there are four: empirical, methodological, definitional and value-laden.
- **The gate's "empirical"** is "what the world is like … some possible observation would settle it". A dispute over whether a proof step holds is settled by a proof or a counterexample, not by an observation, so it has no clean route.
- **Claim type is not contest type:** the claim object's `mathematical` type (§3.1) is a different field. A mathematical claim can be disputed in several contest types.

## 2. Options
| | Option | What it means | Cost, stated fully (m3, m4) |
|---|---|---|---|
| **A** | **Subtype of empirical: `empirical-mathematical`** (recommended) | Keeps the contest lifecycle: pre-registered criteria, coding and ruling by the Judge (D40), rebuttal windows, appeal. The four top-level types stay. | **Not "inherits everything":** most science coding fields (`sites_ok`, `preregistered`, `data_not_previously_reported`, D46 power) do not apply. A needs: new criterion kinds and Judge fields; a revised Judge prompt; `ruling_engine` and `score_gates.py` updates; a new gate JSON enum value; type-gate prompt v2; and every test in §5. |
| **B** | A fifth top-level type, `mathematical` | The same rules, specified anew. | Everything A needs, plus changes to spec §5.3 and the renderer. No gain found. |
| **C** | No mathematical contests: the interim rule becomes permanent | Disputes live only in gap coding. | **Benefit:** the L3 cap already prevents forced resolution, so C is safe, and it needs none of MA1, MA5 or MA7. **Loss:** no pre-registered "what would settle it" for live disputes, which is a spec §8 output. The §5.3 deviation becomes permanent. |
| **C′** | No contest record, but the renderer shows the crux and the candidate settlement routes as **unregistered description**. | Cheap; needs a named producer of the crux text and a scope audit of it. | It loses P9's pre-registration, so the routes are labelled "not registered; not binding". The §5.3 deviation becomes permanent. |

## 3. Routing (type-gate prompt v2; sealed before any new test case is built, m5)
**Definition added:** "**empirical-mathematical:** the sides agree on which statement and which definitions are meant, and disagree on whether a step or statement follows from stated axioms; a proof, a counterexample, or a proof of independence would settle it."

**Order of precedence (M3), applied to each claim or part:**
1. **Value-laden first.** This includes judgements of **importance, depth, elegance or beauty**, which stay **value-laden and mapped**, as profile §1 and §5 say (B1). It also includes **norm questions about mathematics**: whether computer- or AI-assisted proofs should count, what standard of rigour is acceptable, which definition of a concept ought to be used (M1). If a claim cannot be split and any part is value-laden, the whole is value-laden, which is the sealed rule, unchanged.
2. **Definitional next:** the sides read the statement, a hypothesis or a definition differently. Dissolved by splitting into one node per reading. *Example:* "for all n" read over ℕ or over ℤ⁺.
3. **The axiom split:** "S is provable from axioms X" is routed as **empirical-mathematical**. "X should be accepted" is a norm question, so by rule 1 it is **value-laden (mapped)**, and it is **also** recorded as a **P7 stopping point** (shown, not graded; MA10, routing part) (N4).
   - *Definitional, by contrast,* is about which statement or definition is meant, for example which notion of "continuous" a lemma uses.
   - *The axiom split* is about which axioms the proof may use, for example whether the step needs choice (n5).
4. **Methodological:** whether a **method** is reliable, for an **unverified computer search or computation** only (M2).
   - A **kernel's or proof assistant's soundness** is a P7 stopping point with the TM3 reopening trigger, not a contest.
   - An informal "well known" step is a dispute about the step, so it is **empirical-mathematical**.
5. **Empirical-mathematical:** what remains of a proof-step dispute.

**Guard line, extended (M3, M6):** "Never classify a claim whose dispute depends on weighing values as empirical, empirical-mathematical or methodological, even if it contains numbers or theorems."

**Boundary line (M3):** a claim applying a theorem to the world, such as "Arrow's theorem shows no fair voting system exists" or "theorem T makes the protocol secure", is split under profile §1's boundary rule. The theorem is one part, the modelling assumption another (empirical or methodological), and any value term ("fair") a third (value-laden or definitional).

**Priority or novelty** ("who proved it first") is **provenance** (profile §5), not a dispute the gate types. If such a case is ever given to the gate, it returns a new enum value, **`provenance`** (renamed, n7), which is scored as correct only for priority and novelty cases (B1).
- **Zero tolerance covers it (N5):** 0 value-laden cases routed to anything other than value-laden, **and 0 non-provenance cases routed to `provenance`**.

## 4. Rules under A (profile §3.3 text, replacing the interim rule once §5 passes and MA1, MA5 and MA7 are resolved)
**Attachment (M4):** a mathematical contest attaches only to **one proof link and one registered step** of that proof. It never attaches to a statement node. The statement's status comes only from the ST rules (§3.1, D29).

**Registration (B2, m2), at opening, before any new evidence (P9):**
- the informal step's **exact text, quoted**, with its premises and its place in the argument;
- the **axiom set**, fixed for this contest (for example "Lean 4 core plus Mathlib's standard axioms: propext, Quot.sound, Classical.choice"), and recorded as a stopping point. A side may register a second, smaller set (for example without choice) as a separate route;
- the **results the record treats as already established**, each with its own link status. **Only results whose own status is established may be used** (a refereed or formally verified link, not a disputed one) (n6);
- the **neutral reviewer**, named **at registration** (N7). Each side may object once, with a reason, at registration, and an objection names a replacement from a pre-agreed list (for example the relevant editor or a formalization community's reviewers). The neutral is **never an author of the formal statement under review** (§2.4).

**"For" (formal, B2, m7):** a **completed** formalization of the registered step that meets every condition:
- `formal_verified` conditions: no `sorry`, admitted lemmas or new axioms; axioms within the registered set; checked by the kernel;
- **every hypothesis of the formal statement** is either in the registered informal step or one of the registered established results. **The fidelity review checks each hypothesis by name**;
- the hypotheses are **jointly satisfiable in the intended application**, shown by an instance, a model, or a recorded argument that the review accepts;
- a partial formalization, or one with an open `sorry` anywhere in the step's dependency cone, settles nothing.

**"Against":** any one of:
- a **verified counterexample** to the step as registered. "Verified" means **formally checked**, or L8's threshold: two independent reproductions plus an expert check that it meets the registered definitions;
- a **formal proof that the step's statement is false**;
- a formal proof that the step needs a hypothesis that is false in its application;
- **independence (m1, N6):** a formal proof that **the step's implication** (registered premises ⇒ conclusion) is not provable from the registered axioms. It always rests on Con(X), which is recorded as a stopping point. It settles "provable from X" as false for that axiom set only.

**Every "against" route, formal or informal, goes through the same fidelity review** (N1). A counterexample's fit to the registered definitions is a fidelity question, so the Judge never decides it. If the reviewers disagree, the counterexample is credited to the variant reading's node, and the contest stays open.

**Fidelity review (B3, M5):**
- **It is the MA1 fidelity-check procedure**, not a second procedure. Since nothing goes live before MA1 is resolved, there is no "provisionally settled" state (n3).
- **Reviewers:** one reviewer from each side, within **60 days**. **An unanswered side review is replaced by the neutral named at registration,** who has its **own 30-day window** (N7). A `formal_independent` statement written by a third team (profile §2.1) also counts. **Silence never counts as passed, and absence never vetoes.** No author of the formal statement reviews it (§2.4).
- **If the reviewers disagree,** and neither the neutral nor an independent formal statement confirms fidelity within its window: **the result is credited to the variant's own node** (profile §2.4, A1), and the contest on the original **stays open**. That is a definitional split, not a ruling. **A reviewer's dissent can therefore keep a contest open** (n7). **The Judge never decides fidelity** (B3, N1).

**Windows (m6):** the fidelity review completes first. **Then** the 90-day rebuttal window (D40) starts.

**Where the contest sits (P1):** on the pair **(proof, step id)**. While it is open, **L2 applies to every link whose argument uses that step**, except a corrected link whose correction replaces the step. L1 and L4 still come first.

**Outcome mapping (M4, N2, N3), recorded on the (proof, step id) pair:**
- **settled for:** that step's gap is coded **repaired (formal)**. The link's status then follows `math_status.py` from all its gap codes. Settling one step does not formalize the whole proof, so L6/L7 do not follow;
- **settled against:** that step's gap is coded **unrepaired (L5)**, never fatal by itself. Whether the proof as a whole is fatally broken, statement-changing or repairable is left to **MA5 coding of the whole argument** (N2). The independence route gives **statement-changing if the step is provable from X plus an added axiom or hypothesis; otherwise unrepaired** (N6, q3);
- **methodological outcomes:** "for" leaves the step's gap code unchanged and records the method as validated; "against" codes the step **unrepaired** (N3);
- **while open:** **L2 (Contested proof)** for every link that uses the step (P1);
- **after closing:** the link falls back to its gap coding.

**Code changes needed in `math_status.py`, logged as a profile amendment in v1.6 (N3):**
- every gap gets an **id**, so that a (link, step) pair can be addressed;
- `contest_open` moves from the proof to the **(proof, step id)** pair. Every link using the step turns L2, except a correction that replaces it (P1);
- new gap-code fields **`repaired_formal`** (with the formalization's hash) and **`method_validated`** (with the validation's record id), each with self-test rows (q4);
- **L1 and L4 take precedence over L2**, so opening a contest can never improve how a statement displays (ST8 cannot become ST7);
- a second registered axiom set gets **its own statement node**, so one proof never carries two outcomes;
- self-tests for each change, and for every outcome row above (§5.2);
- **review condition:** the v1.6 code and self-test pass a review **before** any §5 protocol that depends on them is sealed. The status-rules document is updated together with the code.

**Methodological criterion kinds (M2), for computer searches:**
- **"for":** a formal check of the computation, or an independent reproduction that meets L8's rules, if the sealed criteria allow it;
- **"against":** a verified error in the code or its output;
- a route needing work not yet feasible is labelled `horizon: long_run` (D47).

**Splits (N-M7), for the user to choose. This changes what the D45 block shows for mathematics. (ii) and (iii) are both logged deviations from D45 and spec §8, because both show no number (n7):**
- **(i)** a numeric split labelled "ungraded". Its basis is **step-level evidence only** (partial formalizations of this argument), not special cases of the statement, with a named producer and an audit. **Conflicts with profile §3.2** (no numbers before a sealed test of the stratum), and risks being read as "P(the proof is right)" (E7).
- **(ii)** no number.
- **(iii) No number; the formal evidence is listed by side** (for example "for: sub-steps 1–3 formalized; against: none"). **Architect's recommendation**, also offered by the auditor. It keeps §8's purpose without an ungraded probability, and it is consistent with §3.2. Like (ii), it is a logged deviation from D45.

## 5. Tests before the first live mathematical ruling (each protocol sealed before it runs; none run until ratification)
1. **Gate (c) re-run with prompt v2** (M6, M7, m5):
   - the 32 sealed cases plus **at least 24 new mathematical cases**, built by an independent builder that reports counts only (D43);
   - **categories, at least 4 cases each:** empirical-mathematical; definitional (two readings of a hypothesis, disguised as a proof dispute); axiom-relative splits; methodological (computer search); importance/elegance; norm questions about proofs; plus near-misses: value terms inside theorems (Arrow / "fair"), unsplittable axiom-and-value mixes, applied-theorem boundary claims, an independence result, priority cases (`provenance`);
   - **near-misses:** at least 2 cases of each near-miss kind, and at least 4 priority cases. They count within the 24 new cases (N5, q2);
   - **scoring of axiom-set splits:** splitting a claim by axiom set counts as correct when the gold answer is that split (n3);
   - **pass, all of:**
     - **regression cases (P2):** at least 31/32, under **the sealed rule**: 0 value-laden cases routed to empirical, empirical-mathematical or methodological. A value-laden case routed to definitional and upheld by blind adjudication, the sealed R21 precedent, does not fail the run;
     - **new cases:** at least ⌈0.9 n⌉, with at most 1 miss per category, and **0 value-laden cases** (including importance, elegance and norm cases) **routed to anything other than value-laden**. This bar is **stricter than the sealed rule**, on purpose, for the new line only;
     - **0 non-provenance cases routed to `provenance`**, on all cases (N5);
   - blind X/Y adjudication of disputed routes, as in D41;
   - **only the new cases count as evidence for the new line.** The 32 regression cases' key was seen by the main session (D41), so they serve as regression only.
2. **Definitions v1.6, `ruling_engine` and `math_status.py`:** the criterion kinds and Judge fields of §4, the outcome mapping, and the new enum value, all covered by self-tests.
3. **Judge recertification (D43 protocol), at least 16 dossiers, at least half of them traps that must not settle** (M8):
   - a formal statement of a variant (fidelity failed);
   - hypotheses as black boxes, and vacuous (jointly unsatisfiable) premises (B2);
   - `sorry`, an extra axiom or a non-standard axiom;
   - "compiles" with no checked statement;
   - a counterexample to a misreading of the step;
   - a counterexample checked against the wrong definitions;
   - an unanswered review and a split review (B3);
   - a proof of the conclusion by a different route;
   - criteria met on both sides (a conflict);
   - two methodological contests;
   - **more new-rule traps (n5):** an absent side whose review is done by the named neutral; a second axiom-set route, which needs its own statement node;
   - **new-rule traps (n6):** an informal counterexample with no fidelity review (must not settle); an external result used as a hypothesis whose own status is disputed; a "settled against" step that must be coded unrepaired, not fatal; an independence result for the conclusion alone, not the implication.

   **Pass:** every ruling correct and 0 forced resolutions. **D43 rule, stated:** any change to the definitions, the Judge prompt or the gate prompt after a run means a fresh builder, a fresh Judge or gate, and a rerun. Failed runs stay in the record.
4. **Renderer (M9):** a fresh scope audit under the D45 protocol (60 answers, 10 planted), with planted mathematical errors:
   - a link status shown as the statement's status;
   - an open contest shown as settled (E6);
   - the evidence list or split shown as a verdict or a probability (E7);
   - the contest applied to a different statement or a variant node.
5. **Criteria audit (M10):** the first mathematical registration gets an audited, sealed criteria record, with Contesting Agents for both sides and an independent criteria auditor, using updated prompts (gate (b) practice, D42 and D48), before any ruling on it.

## 6. Unchanged
- The interim rule, until §5 passes and MA1, MA5 and MA7 are resolved.
- The disputed-fatal cap (L3).
- D47's accepted asymmetry (a missing proof cannot be shown formally), stated in every record.
- Definitional disputes dissolve by splitting.

## 7. On ratification (D74)
- **The log entry:** a D-number in spec §13 in the same turn, with the user's words, the options shown (A/B/C/C′; N-M7 i/ii/iii), the choices and the user's reason (or "no reason given").
- **Profile v1.6** records it. MA10's routing part is marked as settled in the open-issue table.
- **Before any test is run,** each §5 protocol is drafted and audited.

## 8. Audit findings → where applied
**v3 narrow check:**
- **MAJOR:** P1, the contest on (proof, step id) and the code list; P2, the §5.1 pass bars.
- **MINOR:** q1, stale text fixed; q2, near-miss counts; q3, the independence outcome; q4, the new fields.
- **Leftovers:** n3, the axiom-split scoring rule; n5, the two traps.
- The minor findings are numbered as in the audit.

**v2 re-audit:**
- **BLOCKING:** N1 "against" (every route goes through the fidelity review).
- **MAJOR:**
  - N2 and N3: the outcome mapping and the `math_status.py` changes;
  - N4: §3 rule 3;
  - N5: the `provenance` value and §5.1;
  - N6: the independence route;
  - N7: the neutral named at registration.
- **MINOR:**
  - n1, n2: the live condition;
  - n3: the fidelity review;
  - n4: §5.1;
  - n5: the rule 2 and rule 3 examples;
  - n6: Registration and §5.3;
  - n7: the N-M7 heading, the `provenance` name, the dissent sentence.
- **Partly fixed earlier, now completed:** B3 (N1, N7); M1 (N4); M4 (N2, N3); M5 (n1, n2); m1 (N6).
- **Options framing:** C and C′ rows (permanent deviation; C′ producer and audit; A's dependencies stated in the header).

**First audit:**
- **BLOCKING:** B1 §3 rule 1, the enum value and §5.1; B2 §4 Registration, "for" and §5.3; B3 §4 Fidelity review and §5.3.
- **MAJOR:**
  - M1 §3 rules 1 and 3;
  - M2 §3 rule 4 and the methodological kinds;
  - M3 the precedence order, the guard and boundary lines;
  - M4 Attachment and the outcome mapping;
  - M5 the header and the fidelity review;
  - M6 §5.1;
  - M7 §5.1;
  - M8 §5.3;
  - M9 §5.4;
  - M10 §5.5;
  - M11 the N-M7 options.
- **MINOR:** m1 "against" (independence); m2 the axiom set; m3 and m4 §2; m5 §3 header and §5.1; m6 Windows; m7 "for".

## 9. Ratification (user, 2026-11-24, ~12:40 PT; D80)
- **The user's words (question window):**
  - "A: empirical subtype (Recommended)";
  - "(iii) Evidence listed by side (Recommended)";
  - "Ratify now (Recommended)".
- **Options shown:** A, B, C or C′; N-M7 (iii), (ii) or (i); ratify now or not yet. Recommendation: A; (iii); ratify.
- **Reason:** no reason given. The user chose the recommended options.
- **What is now decided:**
  - **contest type:** `empirical-mathematical`, a subtype of empirical;
  - **split:** no number; the formal evidence is listed by side. This is **a logged deviation from D45 and spec §8**;
  - **the routing rules, criteria, fidelity review and outcome mapping** in §3 and §4;
  - **the status-order amendment** (L1 and L4 before L2; contest on (proof, step id)), to be recorded in profile v1.6 together with the code.
- **Not live:** the interim rule stays in force until §5 passes and MA1, MA5 and MA7 are resolved, so not before M1 v4.3. Each §5 protocol is drafted, audited and sealed before it runs.
- **Audit path:**
  - draft: "revise" (3 BLOCKING, 11 MAJOR, 7 MINOR);
  - v2: "revise, narrowly" (1 BLOCKING, 6 MAJOR, 7 MINOR);
  - v3: "ratify with listed amendments" (0 BLOCKING, 2 MAJOR, 6 MINOR);
  - all applied in v4.

**SEAL:** `4db921051af173bfd31507f6b0fed5f8b6c0491e01358c89c6e1830048bdc73b` · 2026-11-24
