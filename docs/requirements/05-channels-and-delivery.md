# 05 — Channels and Delivery

## Requirements

| ID | Requirement |
|---|---|
| QF-CH-01 | **Reliable streams:** ordered byte streams (e.g. file transfer, remote input). |
| QF-CH-02 | **Reliable messages:** individual messages delivered **without loss and without duplicates**, across disconnects, network changes and path switches. |
| QF-CH-03 | Reliable messages MUST **survive an app restart**: the framework keeps a persistent outbox and dedup records. |
| QF-CH-04 | Persistence uses a **pluggable storage interface**, with a default implementation (e.g. SQLite). Applications can supply their own (e.g. IndexedDB on the web). |
| QF-CH-05 | **Unreliable datagrams:** fire-and-forget packets with no retransmission (e.g. presence, VPN packets). |
| QF-CH-06 | Large transfers MUST NOT block or noticeably delay reliable messages. |

## Proposed (needs confirmation)

| ID | Proposal |
|---|---|
| QF-CH-P1 | Reliable messages within one channel are delivered in order. |
| QF-CH-P2 | The sender gets an acknowledgement when the peer has received a message. |
| QF-CH-P3 | Channels can be given priorities (control > messages > datagrams > bulk). |
| QF-CH-P4 | A reliable message is retried until acknowledged or until an app-set deadline expires, then reported as failed. |

## Open questions

| # | Question |
|---|---|
| QF-CH-Q1 | Maximum message size? Larger data goes over streams? |
| QF-CH-Q2 | Since the relay only serves online peers (`QF-CON-05`), how long does a sender's outbox wait for an offline peer? |
| QF-CH-Q3 | Should the message API be request/response (RPC) as well as one-way? |
