# Phase 1: can an AI evidence auditor predict which findings replicate?

**Status:** complete (decisions D24–D26, 17 November 2026; D27, the first Phase 2 test, is included below for context).

**In one line:** on 79 published psychology and social-science findings, the auditor **beat prediction markets** and **matched expert panels**. It scored ahead of the panels, but not by enough to count as a win under the rule we set in advance.

## What was tested

The SCORE programme (DARPA and the Center for Open Science) sampled claims from about 3,900 social and behavioural science papers, repeated a subset of the studies, and published which findings held up. Before those outcomes were known, SCORE also collected forecasts from:

- **Replication Markets**, where people bet on which findings will replicate;
- **repliCATS expert panels**, structured group judgements by researchers;
- **machine forecasters** built by research teams.

Our evidence auditor, an AI model following a fixed written rubric, read a structured summary of each original study's claim and key test (SCORE's own coding, with the original sample size and p-value) and gave a probability that the finding would replicate. It was never shown the replication results.

**The blind procedure:**

- A separate curator agent handled the outcomes.
- The auditor's forecasts were logged and fingerprinted before any outcome was revealed.
- The outcomes were fingerprinted before scoring.

## The pre-registered question

Written before the final batch was built:

- **Primary:** does the auditor beat the expert panels on Brier score? Brier score measures how far probability forecasts are from what happened; lower is better, and always guessing 50% scores 0.25.
- **Rule:** superiority is claimed only if the 95% confidence interval of the difference lies entirely below zero. The interval comes from a paired bootstrap: 5,000 resamples, seed 20261117.
- **Data:** both SCORE batches pooled, 79 findings, of which 40 replicated.

## Results (79 findings)

| Forecaster | Brier (lower is better) | AUC (higher is better) |
|---|---|---|
| **AI evidence auditor** | **0.199** | **0.76** |
| repliCATS expert panels | 0.229 | 0.66 |
| Replication Markets | 0.235 | 0.67 |
| Simple baseline (sample size and p-value) | 0.241 | 0.64 |

| Comparison | Brier difference [95% CI] | Verdict |
|---|---|---|
| Auditor − expert panels (**primary**) | −0.030 [−0.064, +0.008] | **Not established**: the interval includes zero |
| Auditor − prediction markets | −0.036 [−0.068, −0.001] | Auditor better (narrowly) |
| Auditor − simple baseline | −0.042 [−0.079, −0.000] | Borderline |

AUC measures how well a forecaster ranks findings that replicate above those that don't (0.5 is chance).

**Honest summary:** the auditor beats the markets and matches the experts. If the true gap is the size we observed, settling the expert comparison would take roughly 110–150 findings.

**Machine forecasters:** only 13 of the findings had machine forecasts, too few to compare. On those 13 their Brier scores were 0.238–0.265, and one team's AUC (0.82) was higher than the auditor's on the larger batch 4 set (0.78).

## Things that cut against us

- **On the smaller second batch alone (19 findings), the prediction markets beat the auditor:** Brier 0.176 against 0.226 (AUC 0.83 against 0.71). That batch was too small to decide anything, but we report it.
- **A calibration fix failed.** After the early batches, we tried stretching the auditor's probabilities to make them less timid (D23). On fresh data the stretch made them slightly worse, so we dropped it (D24). The forecasts above are the raw, unstretched ones.
- **Different information.** The human forecasters gave their forecasts years earlier, and saw less structured descriptions of each claim than the auditor did. This compares what each forecaster had to work with; it is not a test of skill under identical conditions.
- **Possible contamination.** SCORE began publishing outcomes in April 2026, before the AI model's training cutoff (June 2026). For each of the 79 findings the auditor recorded whether it recalled anything about the outcome, and it recalled none, so the planned check that drops recognised items changed nothing. Still, an early public release is a risk to blindness, and we say so.
- **A gap in coverage.** 15 further SCORE claims belonged to multi-claim papers whose forecasts we could not match, because one SCORE data file was not downloaded. They were left out, and that gap is not random.
- **Choice of aggregation.** We used SCORE's first-listed aggregation of the human forecasts. We chose it before seeing any values, but it may not be each team's preferred method.

## Record fingerprints

These fingerprints (SHA-256) were recorded when each file was sealed, before scoring. External timestamping (Bitcoin and FreeTSA) began on 23 November 2026, so these Phase 1 seals are not externally timestamped. That is a weaker guarantee than the newer records in this repository have.

| File | SHA-256 |
|---|---|
| Batch 4 forecasts | `67a47ff96ae973cacef55fde12bf4b3eb6797854f49fe7ddf4b26fe1a3e2e1dc` |
| Batch 4 outcomes | `9a3aabee5b8e69ab58b14f2069c7a96d5a8f017c33df165f2a00986bc267465d` |
| Batch 4 baseline | `e35932e504466043ee3399b3fe7bdbfe7ccde8510705cf19f95b7c9fa0f2655e` |
| Batch 4 auditor log (159 lines, sealed 2026-11-17 21:59 UTC) | `50a4a18d30eb6ab82a8059774a39c7a76b5632b32e6062b1bc1e35d2df215caa` |
| Batch 5 forecasts | `418ea1580e263fb7e7c8dfeb1243035463601ff6588328caa0b134cc97923e70` |
| Batch 5 outcomes | `ee0c9650d92c41798105a4910d8119b3e3839e08ee218c7fcbb8ce27d54a3898` |
| Batch 5 baseline | `8ebfe23c1c6b72c8ac9b4f8c0855b393d3066ca5f9c030e0ce8397cb3cec025f` |
| Batch 5 auditor log (178 lines, sealed 2026-11-17 22:06 UTC) | `30039e8c504160e9f6d3828f630f57f5aca07234044505cfa997c8a68a32c7ad` |

The files themselves are not yet released. They include data from SCORE, and we are checking SCORE's terms for redistribution. When they are released, you will be able to check them against these fingerprints.

## Decision log entries

The entries below are copied from the project's decision log.

| # | Decision | Rationale |
|---|---|---|
| D24 | **Calibration stretch reverted (auditor v0.7):** default b = 1.0; raw forecasts are logged and used. | On batch 4 (60 SCORE claims, structured packets) raw forecasts were already well calibrated and the v0.6 stretch made them slightly worse. The earlier underconfidence came from weak-information batches. Lesson confirmed: corrections fitted on small samples must be tested prospectively before adoption, and this one failed. |
| D25 | **Phase 1 gate (market comparison) met.** On 60 SCORE claims the auditor (Brier 0.191, AUC 0.78) beat Replication Markets (0.254, 0.62) with CIs excluding zero, and led repliCATS expert panels (0.227, 0.66) with CIs including zero. | Expert-panel comparison needs more claims before claiming superiority. |
| D26 | **Pre-registered expert comparison: not established.** On 79 pooled SCORE claims the auditor led repliCATS expert panels (Brier 0.199 vs 0.229; AUC 0.76 vs 0.66), but the pre-registered 95% CI for the Brier difference included zero (−0.064 to +0.008). Superiority over prediction markets held (−0.068 to −0.001). | Reported as pre-registered. The honest claim is "matches expert panels, beats markets". Settling the expert comparison needs about 110–150 claims. |
| D27 | **Phase 2, Test 2A (pre-registered): author-overlap independence weighting did not improve on raw counts** (Brier +0.0065, CI +0.0015 to +0.0116, n = 103 originals). Lineage tracing will prioritize bias lineages (shared paradigm, measure, sample frame, publication bias) over author overlap; replication history becomes a first-class, first-aggregated evidence type. | Replication records are already ~90% independent teams, so team overlap had almost no variance to use. Earlier replication outcomes were strongly predictive (AUC 0.80). |
