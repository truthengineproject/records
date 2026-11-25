# Worked example: checking a sealed, timestamped rule

This folder lets you check, with free tools, that one of our rules existed at a stated time and has not changed since.

The rule is **PP-1 change 3** (decision D90, 25 November 2026), a small amendment to how the M1 mathematics pilot searches one database. What it says matters less here than what you can prove about it: it was fixed before a deadline it needed to beat.

## Why the timing matters

The M1 pilot selects its papers using a public random source: the hash of Bitcoin block **977,596**, which did not exist when the rules were written. Any rule that affects selection has to be provably fixed **before** that block, or we could have tuned it after seeing which papers came up. So this rule had to be timestamped in a block lower than 977,596.

## The files

| File | What it is |
|---|---|
| `amend-pp1-change3.md` | The rule, exactly as sealed. |
| `opentimestamps/*.ots` | Four OpenTimestamps proof files, collected through four calendar servers (a.pool, b.pool, alice, bob), linking the file's fingerprint to the Bitcoin blockchain. a.pool forwards to alice and b.pool to bob, so they rest on **two** independent Bitcoin commitments: alice's in block 977,516 and bob's in block 977,519. |
| `freetsa/amend-pp1-change3.md.tsr` | A signed timestamp from FreeTSA, an independent timestamping authority. |
| `freetsa/amend-pp1-change3.md.tsq` | The timestamp request that was sent. |
| `freetsa/cacert.pem`, `freetsa/tsa.crt` | FreeTSA's public certificates, as downloaded from freetsa.org. Check them against freetsa.org yourself rather than trusting our copy. |

## Two fingerprints, two layers

The rule file ends with a line beginning `**SEAL (convention b):**`. That is a fingerprint of everything above that line, written into the document when it was approved: `2431aabf…`. (To reproduce it: take the file's text up to the seal line, remove the trailing blank line, end it with a single newline, and hash that. This inner seal is our own convention; the external timestamps below are the part anyone can check without trusting us.)

The timestamps cover the **whole file**, seal line included. Its fingerprint is:

```
3c11dc7ba6492b7f19e61baf7534209fe303f5f1cc26c63779033bd5e7929d8e
```

## Check it yourself

### 1. The fingerprint

On macOS:

```
shasum -a 256 amend-pp1-change3.md
```

On Linux:

```
sha256sum amend-pp1-change3.md
```

On Windows (PowerShell):

```
Get-FileHash amend-pp1-change3.md -Algorithm SHA256
```

The result should be `3c11dc7b…9d8e` as above (PowerShell shows it in capital letters, which is the same value). If even one character of the file had changed, it would be completely different.

### 2. The Bitcoin timestamps (OpenTimestamps)

The simplest way: go to https://opentimestamps.org, open the "Stamp & Verify" area, drop in one of the `.ots` files, then drop in `amend-pp1-change3.md`. It will report the Bitcoin block and date.

With the command-line client (`pip install opentimestamps-client`):

```
ots info opentimestamps/amend-pp1-change3.md.alice.btc.calendar.ots
ots verify -f amend-pp1-change3.md opentimestamps/amend-pp1-change3.md.alice.btc.calendar.ots
```

`ots verify` needs access to a Bitcoin node to give a final answer; without one it may fail or report that it cannot check. `ots info` shows the full proof path, ending in the Bitcoin block, without a node.

**Expected result:** the a.pool and alice proofs attest in block **977,516**; the b.pool and bob proofs attest in block **977,519**. Both are below 977,596, so the rule was fixed before the selection block existed.

### 3. The FreeTSA timestamp

With OpenSSL, after checking the certificates against https://freetsa.org:

```
openssl ts -verify -data amend-pp1-change3.md -in freetsa/amend-pp1-change3.md.tsr -CAfile freetsa/cacert.pem -untrusted freetsa/tsa.crt
openssl ts -reply -in freetsa/amend-pp1-change3.md.tsr -text
```

**Expected result:** `Verification: OK`, with a time of `Nov 25 11:50:55 2026 GMT`. OpenSSL first prints a warning that `tsa.crt` "is not a CA cert"; that is normal, because it is FreeTSA's signing certificate, not its root.

## A note on how these proof files were made

OpenTimestamps calendars first return a "pending" proof, which is completed once the calendar's batch is written into a Bitcoin block. We collected the completed parts from each calendar and joined them to the original pending proofs, then checked each completed proof's Bitcoin block against two independent block explorers (mempool.space and blockstream.info). If you prefer, you can redo the completion step yourself: `ots upgrade` on a pending proof fetches the same data from the calendar.
