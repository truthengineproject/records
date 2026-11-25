# The Truth Engine: public records

This repository is the public record of **The Truth Engine: Calibrated, Contestable Knowledge**, an independent project to build AI that states how confident it should be, shows its evidence, and can be proven wrong.

Project blog: https://truthengineproject.substack.com

## What this repository is for

The project makes three promises:

1. **Rules before results.** Every test gets a written plan before it runs. Since 23 November 2026, every new plan is also timestamped by two independent outside services, so anyone can check it existed first and hasn't changed. Earlier plans were sealed with fingerprints only.
2. **Every result is published**, including the ones that don't go our way.
3. **Corrections are public.** Sealed files are never edited. Mistakes are logged in [`CORRECTIONS.md`](CORRECTIONS.md).

This repository is where you can check those promises for yourself.

## What's here so far

| Folder | What it holds |
|---|---|
| [`examples/pp1-change3/`](examples/pp1-change3/) | A worked example of a sealed, timestamped rule, with its proof files and step-by-step instructions to verify it yourself. |
| [`results/phase1/`](results/phase1/) | The Phase 1 result reported in our first post: the evidence auditor against prediction markets and expert panels. |
| [`CORRECTIONS.md`](CORRECTIONS.md) | The public corrections log. |
| [`SHA256SUMS`](SHA256SUMS) | Fingerprints of every file in this repository. |

More records will be added as they are screened for release: the other sealed plans, their proofs, the decision log and further results.

## What is not here, and why

Some project files are deliberately held back:

- **Anything that would spoil a running test**, such as the papers selected for a test before its rules allow them to be named, or forecasts that have not yet been scored. These are released when the test's own rules allow.
- **Secrets**: answer keys for audits, access keys, encrypted custody files.
- **Personal details** of the people involved.

Where a sealed file is held back, we list its fingerprint (for example, the Phase 1 data files in [`results/phase1/`](results/phase1/)), so that when it is released you can check it is the same file.

## How the work is done

An AI model (Claude) drafts plans, code and analyses. Separate, freshly started AI auditors check the work. The project lead reviews and approves every rule and every publication, and each approval is numbered in the decision log (for example, "D90").

## Licence

Documents are released under [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0). Code is released under the [MIT licence](LICENSE-CODE). See [`LICENSE`](LICENSE).
