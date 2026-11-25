# Sealed plans

Each folder holds one approved plan or rule, exactly as sealed. Each also has its proofs: four OpenTimestamps files (Bitcoin) and a FreeTSA signed timestamp. The step-by-step guide is in [`examples/pp1-change3/`](../examples/pp1-change3/README.md), which also holds the FreeTSA certificates. For example, from inside `plans/d83-dp2-prereg/`:

```
sha256sum dp2-prereg.md
openssl ts -verify -data dp2-prereg.md -in freetsa/dp2-prereg.md.tsr -CAfile ../../examples/pp1-change3/freetsa/cacert.pem -untrusted ../../examples/pp1-change3/freetsa/tsa.crt
ots info opentimestamps/dp2-prereg.md.alice.btc.calendar.ots
ots verify -f dp2-prereg.md opentimestamps/dp2-prereg.md.alice.btc.calendar.ots
```

**None of these tests has produced results yet,** and the three mathematics rules are not yet fully in use. The tests wait for API credits (expected around 30 November 2026). They are published now so that you can later check that results were scored by the rules written in advance.

**Known labels in these sealed texts.** Sealed files can't be edited, so here are the corrections to labels inside them. Each plan's FreeTSA time is the authority for when it was fixed.

| Plan | The sealed text says | The correct reading |
|---|---|---|
| D79 | ratified "~11:55 PT" (twice) | ratified before 11:48:16 PT, the FreeTSA time; "~11:55" was an estimate written after the fact |
| D80 | ratified "~12:40 PT" (twice) | ratified before 12:11:15 PT, the FreeTSA time |
| D82 | ratified "~13:25 PT" (twice) | ratified before 13:16:32 PT, the FreeTSA time |
| D83 | status "v2, 2026-11-24 ~14:00 PT" | the drafting estimate; sealed at 13:47:12 PT |
| D89 | title says "(draft v2)" | ratified as v2.1, as its own status line says |

| Decision | Folder | What it is, in plain words | FreeTSA time (UTC) | Bitcoin blocks |
|---|---|---|---|---|
| D76 | [`d76-teaching-run5-a2/`](d76-teaching-run5-a2/) | **Teaching test, run 5.** Can the system's Teaching Agent explain claims while reporting their status and uncertainty honestly? 80 real lessons and 10 deliberately flawed control lessons, checked by separate, freshly started AI auditor passes (the same model as the teacher) through a logged, pinned interface. | 2026-11-24 18:56:43 | 977408, 977410 |
| D79 | [`d79-ag1-auditor-agreement/`](d79-ag1-auditor-agreement/) | **How much do the auditors agree?** It re-audits 100 earlier lessons three times. The result fixes, in advance, how many auditor passes must flag a lesson before it counts as flagged in run 5. If agreement is too low, run 5 cannot go ahead until the auditor instructions are revised and this study is repeated. | 2026-11-24 19:48:16 | 977408, 977410 |
| D80 | [`d80-ma9-math-disputes/`](d80-ma9-math-disputes/) | **Disputes about mathematical proofs.** How a disagreement over one step of a proof is classified, what evidence each side may bring, and how it is settled. A formal machine-checked proof counts only if a separate review confirms it proves the same thing. *Not yet in force.* | 2026-11-24 20:11:15 | 977410 |
| D82 | [`d82-math-profile-v1.6/`](d82-math-profile-v1.6/) | **The mathematics rulebook, v1.6.** What counts as evidence for a mathematical claim, what each status means, and the order in which problems with a proof decide its status. The status rules are fixed in code, which is the authority (the code will be published in a later release). | 2026-11-24 21:16:32 | 977417, 977418 |
| D83 | [`d83-dp2-prereg/`](d83-dp2-prereg/) | **Discovery pilot 2.** An AI "Scientist" predicts whether 12 registered studies, accepted before their data exist, will support their main hypothesis. These are studies accepted in principle before their results are known. Its predictions are compared with two sceptical AI forecasters, a coin flip and the historical base rate (44%). Scored by Brier score only; nothing it predicts changes any status in the system. | 2026-11-24 21:47:12 | 977425, 977426 |
| D88 | [`d88-dp2-addendum-r1/`](d88-dp2-addendum-r1/) | **The program that runs discovery pilot 2's AI roles.** Its safeguards include: the sceptics see exactly the committed study texts; a dry run comes first; every call is logged; and a flag from the check for already-known outcomes is a failure unless the project lead clears it with a logged reason. | 2026-11-24 22:49:06 | 977427, 977431 |
| D89 | [`d89-ma1-ma7/`](d89-ma1-ma7/) | **Checking formal proofs and searching the literature.** When does a machine-checked proof really prove the original claim (two reviews, both must pass, at least one by an audited setup, plus a named expert's sign-off)? And which sources must be searched before a result is called new? The literature-search rules (MA7) are adopted. *The proof check (MA1) is not yet in force:* it must first pass a test with deliberately planted faults, and the rulebook's code must be amended (v1.7). | 2026-11-24 22:58:51 | 977428, 977431 |

## Held back for now: the M1 mathematics pilot

The M1 pilot's procedures (how papers are selected, searched and screened) are not published yet, because the pilot is running. Its design controls appear in the mathematics rulebook (D82). Its papers are chosen at random using the hash of Bitcoin block **977,596**, which had not been mined when the rules were sealed.

What you **can** check now: [`held-back/m1-pilot/`](held-back/m1-pilot/) holds the Bitcoin and FreeTSA proofs for every M1 file that was timestamped directly before that block. Other M1 files (later script versions, the procedures text, the fingerprint lists) are fixed indirectly: their fingerprints are listed inside these timestamped files or in the record chain. Each proof is tied to the file's fingerprint, listed below. The files themselves come later. When they do, you will be able to confirm they are the same files and were fixed before the selection block existed. (One M1 rule, PP-1 change 3, is already published in full in [`examples/pp1-change3/`](../examples/pp1-change3/).)

| Decision | Folder | What it is | SHA-256 fingerprint | FreeTSA time (UTC) | Bitcoin blocks |
|---|---|---|---|---|---|
| D71 | `d71-frame-seal` | The frame seal: the document that fixes the 23,869-paper sampling frame (by its files' fingerprints) and the draw rule | `7d6ff05acc480d0a405f59a98110c4c9637c168b27a8bb42b7826a764e06a8d2` | 2026-11-24 17:40:18 | 977398, 977399 |
| — | `rehearsal-draw.py` | An earlier version of the draw script, used in a rehearsal and superseded by D75's | `caa48065497e283f1bd238c49e9e5d63472f91c44a422040a9c5c2304e2e3867` | 2026-11-24 18:34:50 | 977408, 977410 |
| D75 | `d75-draw.py` | The draw script: turns the block hash into the order of papers | `a34d54e20761bb66ab523b456bb9ddea419b6bdef4fd8cbb2a29aeb1354d5c2a` | 2026-11-24 18:51:48 | 977408, 977410 |
| D75 | `d75-regsearch.py` | The script that plans and logs the registration searches | `8a1d4af20a5b214f7f8826f636f184c5aeaa86f0c7a327a20202d0647ebf4aa5` | 2026-11-24 18:51:54 | 977408, 977410 |
| D75 | `d75-targets_meta.py` | The script that gathers each drawn paper's details (the D75 version; replaced at D77 by a version fixed in the procedures fingerprint list) | `90b4ff8eccdb5acf0ed30f4bb9e9ef5b96adbdd589597a8f8b65085d6d601f94` | 2026-11-24 18:51:59 | 977408, 977410 |
| D75 | `d75-pp1-v2-with-scripts` | The audited procedures amendment PP-1 (v2), stamped with its scripts | `654585b3b2742a8c164c79e27d34bda56d32b715c8b01f1e36d6505ef6473325` | 2026-11-24 18:52:05 | 977408, 977410 |
| D75 | `d75-pp1` | PP-1 as ratified, with its seal line | `1d8935cd7079bca8f18109efa306c6ccf3f3c48bbd76a539734b540d21991f72` | 2026-11-24 18:56:09 | 977408, 977410 |
| D77 | `d77-pp1-change1` | PP-1 change 1: how freely available paper copies are obtained | `e249865886f79b84f6da22a758c0357e26849e8776adfeba8cb874be59c1fedf` | 2026-11-24 19:39:54 | 977408, 977410 |
| D81 | `d81-pp1-change2` | PP-1 change 2: what happens when a search stalls | `a3712c1ee39fcf3ba90e3bd9788fd73312ea7116ec11379abd4411dffe504646` | 2026-11-24 20:51:32 | 977416 |
| D90 | `d90-afp_index_search.py` | The search script named in PP-1 change 3 | `694a227edc0dd72a60a7cd2d8c24d3c10eee2cb8d3bdac6dc89e71d22bc1aa8f` | 2026-11-25 11:51:01 | 977516, 977519 |
| — | `chain-head-e3c0d7c2b65a1a9f` | The pilot's append-only record chain, as it stood at each seal. Each later head contains every earlier one, so the latest (`5b372cb4…`) covers them all. (One head between these, `64ac989e…`, is not shown because its Bitcoin proofs were still pending; it is contained in `5b372cb4…`.) | `e3c0d7c2b65a1a9f36782a8d4c3da0f6e57811e3b3b6c69137fdc8768f74c433` | 2026-11-24 18:56:15 | 977408, 977410 |
| — | `chain-head-0d63e5a7401376ba` | (chain head) | `0d63e5a7401376bae71d0060d367caac9954e86bea887772fd1b6bc8b7ddc9da` | 2026-11-24 18:37:07 | 977408, 977410 |
| — | `chain-head-63f6c87adbc32e0c` | (chain head) | `63f6c87adbc32e0c2a4731ba39ea0686ffa11be90f653e6d969d9781289eb048` | 2026-11-24 18:52:11 | 977408, 977410 |
| — | `chain-head-9acb2341fab47595` | (chain head) | `9acb2341fab4759516cc8d3604d93bc91204cb3ec90f1558b11c4839bec6ee46` | 2026-11-24 19:39:59 | 977408, 977410 |
| — | `chain-head-9deb4dd389094241` | (chain head) | `9deb4dd38909424175e5128e958d081ba118ae5484f50ff2eabb1a9824e3ad9c` | 2026-11-24 20:51:39 | 977416 |
| — | `chain-head-5b372cb43708d6ba` | (chain head, after D90) | `5b372cb43708d6ba3797c56e78754c8fa0d204fdd824a617b01f9b5e77ce46d2` | 2026-11-25 11:51:08 | 977516, 977519 |

Each held-back folder holds the four `.ots` proofs, the FreeTSA reply (`.tsr`) and the request (`.tsq`). Without the file you can still read the FreeTSA reply with `openssl ts -reply -in <file>.tsr -text`, which shows the fingerprint and time, and inspect each `.ots` proof with `ots info`. To check that a proof belongs to the file, you need the file itself, once it's released.
