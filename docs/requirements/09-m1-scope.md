# 09 — Milestone 1: Networking Foundation

> The base everything else depends on, and the first Rust learning project.
> **No E2EE live channels in M1.** Traffic is protected by TLS only, so the relay can read it. M1 is for development and testing only (`QF-SEC-04`).

## In scope

- **Platforms:** Windows, macOS, Linux desktops; Linux server.
- **Components:** client libraries, server libraries plus reference server (gateway + relay), credential issuer library plus a dev issuer command-line tool, test client.

## Requirements

### Identity
| ID | Requirement |
|---|---|
| M1-01 | Device key pair generated locally; device ID derived from the public key (`QF-ID-01`, `QF-ID-02`). |
| M1-02 | Standard credential format and issuer library, plus a **dev issuer command-line tool** for tests (`QF-ID-03`, `QF-ID-04`). |
| M1-03 | The server rejects devices without a valid credential; LAN peers verify each other offline; a custom verifier can be plugged in (`QF-ID-05`, `QF-ID-06`). |

### Transport and connection
| ID | Requirement |
|---|---|
| M1-04 | Authenticated, TLS-protected connection from client to server. |
| M1-05 | Dead-connection detection, automatic reconnect, resume (`QF-NR-03`–`QF-NR-05`). |
| M1-06 | Transports and fallbacks: QUIC, TCP/443, HTTP proxy, IPv6/NAT64; automatic choice **and** app-chosen transport (`QF-TR-01`–`QF-TR-05`). |

### Delivery
| ID | Requirement |
|---|---|
| M1-07 | **Reliable messages** between two devices, persistent across restarts through the storage interface with a **SQLite default** (`QF-CH-02`–`QF-CH-04`). |
| M1-08 | **Relay path:** forwarding between two online devices (`QF-CON-04`, `QF-CON-05`). |
| M1-09 | **LAN path:** direct with no server, using a pluggable visibility policy (`QF-CON-01`, `QF-CON-02`). |
| M1-10 | No loss and no duplicates on either path and when switching between them (`QF-NR-02`). |

## Proposed (needs confirmation)

| ID | Proposal |
|---|---|
| M1-P1 | **Test client:** command line (contacts, send, receive, delivery state). |
| M1-P2 | **Example visibility policy:** an allow-list read from a config file. |
| M1-P3 | **Network simulator** built in M1 and used for all M1 tests (`QF-NR-P1`). |
| M1-P4 | **Acceptance:** `QF-NR-P2` passes over the relay path, the LAN path, and while switching. |

## Out of scope for M1

E2EE live channels · NAT hole punching · reliable streams and datagrams beyond what M1 tests need · media crate · iOS, Android, WebAssembly, embedded · bindings · horizontal scaling · revocation · qingfeng-im features.

## Open questions

| # | Question |
|---|---|
| M1-Q1 | Agree the credential format with qingfeng-java now, or let M1 define it (`QF-ID-Q2`)? |
| M1-Q2 | Two physical machines for LAN tests, or VMs / network namespaces at first? |
| M1-Q3 | Run M1 on all three desktop OSes, or start on Windows and add the others later? |
