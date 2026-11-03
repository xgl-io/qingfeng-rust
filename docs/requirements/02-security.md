# 02 — Security

## Requirements

| ID | Requirement |
|---|---|
| QF-SEC-01 | Every connection (device to server, device to device) MUST be encrypted and mutually authenticated using device credentials (`QF-ID`). |
| QF-SEC-02 | qingfeng-rust MUST provide **E2EE live channels**: two online peers set up a channel encrypted between themselves, so a **relay in between only forwards ciphertext** it can't read. |
| QF-SEC-03 | Async messaging E2EE (encrypting for offline recipients, ratchets, groups) is **not** part of qingfeng-rust. It belongs to applications (e.g. qingfeng-im). |
| QF-SEC-04 | In **M1**, channels are protected by TLS only (no E2EE live channels yet): the relay can read traffic. M1 MUST NOT be used by real users. |

## Proposed (needs confirmation)

| ID | Proposal |
|---|---|
| QF-SEC-P1 | Only established, audited cryptography and protocols; no custom cryptographic algorithms. |
| QF-SEC-P2 | E2EE live channels SHOULD have forward secrecy (fresh keys per session). |
| QF-SEC-P3 | A written **threat model** (LAN attacker, malicious relay, stolen device) SHOULD be kept up to date with the design. |
| QF-SEC-P4 | An **external security audit** SHOULD happen before any consumer launches publicly. |
| QF-SEC-P5 | Protocol decoders SHOULD be fuzz-tested. |

## Open questions

| # | Question |
|---|---|
| QF-SEC-Q1 | Are E2EE live channels always on through a relay, or chosen by the app per channel? |
| QF-SEC-Q2 | What may the relay see (device IDs, timing, sizes), and do we try to reduce it? |
