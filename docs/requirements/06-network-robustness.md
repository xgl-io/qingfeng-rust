# 06 — Network Robustness

## Requirements

| ID | Requirement |
|---|---|
| QF-NR-01 | Reliable messages MUST work on the **bad mobile** profile: **50–100 kbps**, **500–1000 ms** latency, **10–20% packet loss**, **frequent disconnects and network switches** (Wi-Fi ↔ 4G, subway, elevators). |
| QF-NR-02 | No message loss and no duplicates under these conditions (`QF-CH-02`). |
| QF-NR-03 | The client MUST reconnect automatically after a disconnect or network change. |
| QF-NR-04 | After reconnecting, the client MUST resume: send only what wasn't acknowledged, receive only what it missed. |
| QF-NR-05 | Dead connections MUST be detected rather than waited on indefinitely. |
| QF-NR-06 | Mass reconnection (e.g. after a server restart) MUST NOT overload the servers at the largest consumer's scale (`G-6`). |
| QF-NR-07 | Restricted networks: see `QF-TR-02` to `QF-TR-04`. |

## Proposed (needs confirmation)

| ID | Proposal |
|---|---|
| QF-NR-P1 | A **network simulator** (latency, loss, bandwidth limits, forced disconnects) is part of the project and used in automated tests. |
| QF-NR-P2 | **Acceptance test:** on the bad mobile profile plus a forced disconnect every 30 s, 1,000 reliable messages between two devices all arrive exactly once, in order. |
| QF-NR-P3 | **Latency target:** on the bad mobile profile, 95% of reliable messages arrive within 3 s while connected. |
| QF-NR-P4 | Small protocol overhead (compact encoding, batched acknowledgements). |
| QF-NR-P5 | Reconnect with exponential backoff and random jitter, but immediately on a network change or wake from sleep. |

## Open questions

| # | Question |
|---|---|
| QF-NR-Q1 | Mobile battery and data limits (heartbeat frequency, background connections)? |
| QF-NR-Q2 | Is an "extreme" profile (<20 kbps, 2–5 s latency, 30%+ loss) a later target? |
| QF-NR-Q3 | Robustness targets for real-time media (see `07-media.md`)? |
