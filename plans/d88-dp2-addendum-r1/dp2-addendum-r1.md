# DP2 addendum R1: the harness runner (`dp2_api.py`)

**Status:** **RATIFIED by the user, 2026-11-24 (D88)**: "Ratify and seal (Recommended)". Runner **v4.1**. Architect. Audit path (`dp2-addendum-runner-audit.md`): audit "revise and re-check" (3 BLOCKING, 8 MAJOR, 7 MINOR); re-check v2 (0, 4, 7); re-check v3 (0, 3, 3); re-check v4 (0, 1, 2); re-check v4.1 **"ready to seal"** (0, 0, 0). User decisions D86 and D87 are in §5.2. Earlier runners are kept as `dp2_api_v1_audited.py`, `dp2_api_v2_rechecked.py`, `dp2_api_v3_rechecked.py` and `dp2_api_v4_rechecked.py`. **Nothing runs before the API credits and a dry run (D60).**

## 1. What is pinned
- **Runner:** `phase6/dp2_api.py` v4.1, sha256 `247ab47a6ffe1320d24eaf35a017e461b98cd7b76b043b019bd2a17e86c81e8d`. Python 3.13. Self-test: `python3 -I dp2_api.py selftest` prints PASS.
- **Client:** `api/te_api.py`, sha256 `8807790d2fcb471a589ff0f68c77ad0ca2d320d28ddaed3d92e45f578ea169f3`, checked in full at prepare and at submit.
- **D57 gate:** `firewall.py` `827a2edff3fa1a7d3597068a8c1e142ff108bc8e3f8eaeadf55c610c2603ac1a`, through the teaching harness's gate module `teach_audit5_harness/gate.py` `b325eb190bcfefc565a84916f951a8ff25761824c79dc2df5a082f4271bc51b4` (harness 0.5.1). Both are checked in full at prepare and at submit, and the gate's exit code must agree with its hit count.
- **Model** `claude-opus-5-5`, **temperature 0**, **max_tokens 8000**, for every step. No tools.

## 2. Steps (DP2 §3.2(a), §4.2–§4.5)
| Step | DP2 § | Sees | Never sees | Reply |
|---|---|---|---|---|
| `recall_screen` | §3.2(a), N5 | title, authors | anything else | `knows_outcome`, `claimed_result` |
| `recall_post` | §4.4 | title, authors | anything else | same |
| `rescue` | §4.2 | the hypotheses; the scored test's "Supported if" line; **the direction of departure (above, below or none), computed by the runner from the sealed prediction against 0.44**; the link statement; what would count against; the protocol extract; the base-rate sentence; DP1's R1–R4 **verbatim**, with the DP2 addition to R2 | the probability | R1–R4, each pass or fail with a reason |
| `skeptic_blind` | §4.3 | the scored test, built from the Screener's "Supported if" line; one fixed resolution-rule sentence; the quoted decision rule; the base-rate sentence | the protocol, any Scientist output | `p_supported` |
| `skeptic_informed` | §4.3 | the same, plus the protocol extract | the Scientist's diagnosis and hypotheses | `p_supported` |
| `adjudicate` | §4.5 | the fixed resolution rule, the "Supported if" line, the quoted rule, the Stage 2 source and extract; **the complement rule** (not supported, inconclusive and partial all count as not supported); "unresolvable" limited to the §6 cases | every probability | `supported`, `not_supported` or `unresolvable`; a quote; `rule_conflict` |

**Provenance (B1, N3):** for `rescue`, both Skeptics and `adjudicate`, the "Supported if" line and the quoted decision rule must equal the **screen record** exactly, and the protocol extract must match its hash there. **The screen record's sha256 must equal the §3.3 commit.** So no Scientist text can reach the Skeptics or the adjudicator. Skeptic packets naming the Scientist or a forecaster are **flagged for review, never refused**, because their text is fixed by the screen record and DP2 §4 rules out dropping a target over its protocol text (N2).

**Predictions (N3, P3):** the Rescue Auditor's direction comes from the predictions record, whose sha256 must equal the hash written when the Scientist sessions closed. **The runner never sends the number.** If the Scientist's own text (hypotheses, link statement, what would count against) contains it in a common form, the packet is **flagged for review**, not refused; the screen-sourced line and the protocol extract are not checked.

**Review flags (p3):** Skeptic-term flags and Rescue number flags are listed in `MANIFEST.json`. **The user reviews them before the real submit of that step**, and the review (reviewer, date, decision, reason) is logged. **A flag never removes a target.**

**Order (minor 5, p2):** `recall_post` runs only for targets whose Scientist sessions are closed (a listed record, whose sha256 is recorded); `adjudicate` only once the seal-2 sha256 file exists, parsed, with its value recorded. The screen-commit and predictions closing hash values and each Stage 2 extract's sha256 are recorded in `MANIFEST.json`.

**Other refusals:** a field name carrying a probability or a score; Scientist fields in the Informed Skeptic's packet; a direction that is not the one computed from the sealed prediction; a repeated target.

## 3. Run rules
- **One step per run directory, run once.** `prepare` requires an empty directory and copies the inputs, the screen record and the predictions record into it. **One ledger** across run directories (`phase6/dp2_runs/LEDGER.jsonl`; real mode uses only this one), **keyed by step and target** (P1), so later adjudication batches run as results arrive. **Before the first real call, `submit` writes an "attempted" line for each target to the ledger, so the ledger alone records what was run, wherever run directories are later moved; a ledger run directory that is missing is refused, never read as unattempted (Q1).** A target already prepared for real in a step may be prepared again only (a) if that earlier run made **no real attempt** for it, logged as abandoned, or (b) as **the one repeat of the latest real run** for it, of the same step and covering only its targets, when that run is **void** (P2). The one-repeat limit is **per target** (q2). **A void repeat stops the step for those targets**, which is reported.
- **D57:** each request's system prompt, **user-prompt template** and packet are gated. In a dry prepare, hits are listed for review. In a real prepare, a hit not in the reviewed file stops preparation. **Each reviewed hit must carry a reviewer, an ISO date and a reason** (DP2 §4). Hits in protocol or Stage 2 text go to the user and are never grounds to drop a target after the draw.
- **D60:** a real submit refuses unless a dry submit of the same directory exists. It also refuses if the runner, client, gate or firewall changed, or if the requests **rebuilt from the copied inputs** differ from `REQUESTS.json`. The key file is given on the command line only.
- **Failures:** each call is logged as **attempting**, with its attempt number, **before** it is made, and replies are paired with attempts by tag (N1). An interrupted run is recoverable: an attempt with no reply counts as an API error, and requests never sent are resent. Any exception from a call, including network errors, is recorded as an API error. An invalid reply is re-sent **once**; API errors **up to twice**; **never more than 4 attempts** per request. A resend needs a `collect` made after the last submit. After that the request is **missing**, and it is reported. **A missing recall request counts as flagged** (D87).
- **Model mismatch:** a reply from any model other than the pinned one **voids the step**. It is repeated once in a new directory.
- **Recall flag:** any reply with `knows_outcome: true`, or with any non-empty `claimed_result`, is flagged, and the full reply text is kept. What a flag means is §5.2.
- **Dry replies** (for testing the parser) are written to `STATUS_dry.json` and `RESULTS_dry.json` only, and are refused on a directory with a real submit.
- **Logs:** `api_log.jsonl` (full requests and responses, D60) and `api_log_dry.jsonl` are archived with their sha256, with `MANIFEST.json`, `INPUTS.json`, `SCREEN.json`, `PREDICTIONS.json`, `REQUESTS.json`, `SENT.jsonl`, `STATUS.json` and `RESULTS.json`.

## 4. Self-test (offline, no key, no network)
`python3 -I dp2_api.py selftest` prints PASS and lists what it covers: the gate on every step's prompt, template and packet; the pins; the prompt content (base rate, adjudicator rules, DP1 R3, a neutral recall prompt); every input and order refusal above; the provenance and commit ties; flagged-not-refused Skeptic terms; the run rules (empty directory, dry before real, run once, ledger and repeats, key, rebuilt requests, fresh collect, bounded resends, interrupted runs, network errors); the recall flags (any claimed result, missing = flagged); the model-mismatch void; dry replies kept apart; the parsers.

**Recall prompt wording (minor 7):** "If you are unsure, say what you think you know and that you are unsure", in place of "do not guess", so partial knowledge is reported and can be flagged.

## 5. Choices made here
1. **The Rescue Auditor sees the direction, not the number.** The direction is computed mechanically: above if P(supported) > 0.44, below if < 0.44, none if equal, from the sealed predictions record. (Audit: acceptable under DP2.)
2. **What a recall flag means (user decisions D86, D87, 2026-11-24):**
   - **Every flagged reply fails (a) at the screen, or counts as a form failure at the second run, unless the user clears it** with a logged reason.
   - **Deadlines (D87):** screen flags are cleared **before the commit**; second-run flags **before seal 2**.
   - **A recall request that ends missing counts as flagged** (D87).
   - **The user reads every recall reply's full text** (`RESULTS.json` keeps it) before the deadline, and may **add** a flag where the prose states a result (D87).
   - The main session does not judge flags.
3. **Messages API, one call per request**, not Batches. Steps have up to about 140 requests (the screen's recall probe runs on every candidate); others at most about 40. (Audit: acceptable.)

**SEAL:** sha256 of everything above this line: `de5db5ba715ccb1da7af3629fe02a76de9d90d7212ea38383827c78f537e9114` · 2026-11-24
