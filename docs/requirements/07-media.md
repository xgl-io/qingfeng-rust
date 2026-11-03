# 07 — Real-time Media Transport

## Requirements

| ID | Requirement |
|---|---|
| QF-MED-01 | Real-time media transport MUST be a **separate library crate** in the qingfeng-rust workspace, so applications that don't need it can leave it out. |
| QF-MED-02 | It serves at least **qingfeng-im** (voice and video calls) and **qingfeng-meetings**. |
| QF-MED-03 | It carries low-latency audio, video and screen streams, with congestion control, bitrate adaptation and loss recovery. |
| QF-MED-04 | **Codecs, call and meeting logic, and UI stay in the applications.** |

## Proposed (needs confirmation)

| ID | Proposal |
|---|---|
| QF-MED-P1 | Media runs over qingfeng-rust's connectivity (LAN, internet direct, relay) and security (E2EE live channels). |
| QF-MED-P2 | It SHOULD degrade gracefully on weak networks rather than drop the stream. |

## Open questions

| # | Question |
|---|---|
| QF-MED-Q1 | Built on WebRTC (e.g. a Rust WebRTC stack) for browser compatibility, or a custom protocol over QUIC? |
| QF-MED-Q2 | Does the crate include a forwarding media server (SFU) for group calls and meetings, or only peer-to-peer media? |
| QF-MED-Q3 | Quality and latency targets? |
