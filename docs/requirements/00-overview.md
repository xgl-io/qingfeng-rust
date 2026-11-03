# qingfeng-rust — Requirements Overview

> Status: **draft v2** · 2026-11-02 · Rewritten after the decision to make qingfeng-rust a basic communication layer.

## 1. What qingfeng-rust is

qingfeng-rust is a **basic communication layer**: Rust libraries plus a reference server that give applications secure, robust communication between devices, over the internet, through a relay, or directly on a LAN with no server.

Application-specific features belong in the applications built on top of it.

| Consumer | Status | Uses qingfeng-rust for |
|---|---|---|
| **qingfeng-im** (chat app) | Next, after qingfeng-rust's first release | Identity, transports, LAN, relay, reliable messages, media transport for calls |
| **qingfeng-meetings** | Out of scope this phase | Transports, relay, media transport |
| **qingfeng-vpn** | Out of scope this phase | Identity, transports (TCP / QUIC / TLS modes), secure channels, relay, NAT traversal |
| Future (e.g. remote desktop) | — | — |

## 2. Scope

| In qingfeng-rust | In the applications |
|---|---|
| Transports and fallbacks; app-chosen transport | Business logic, UI, accounts and login methods |
| Device keys and IDs, standard credential format, issuer library, offline verification, pluggable verifier | Device policies (e.g. one device per type), who issues credentials |
| Secure channels, **E2EE live channels through the relay** | **Async messaging E2EE** (ratchet, groups) |
| LAN discovery with a **pluggable visibility policy** | The policy itself (e.g. "contacts only") |
| Relay **forwarding between online peers**; gateway; NAT hole punching | **Store-and-forward mailboxes** for offline devices |
| Reliable streams, reliable messages (persistent, pluggable storage), unreliable datagrams | Conversations, history, sync |
| **Real-time media transport** (separate library crate) | Codecs, call and meeting logic |
| Weak-network handling, network simulator | — |
| — | VPN tunnel helpers (TUN) → qingfeng-vpn |

## 3. Documents

| File | Topic |
|---|---|
| [01-identity-and-trust.md](01-identity-and-trust.md) | Device keys, IDs, credentials, verification |
| [02-security.md](02-security.md) | Secure channels, E2EE live channels |
| [03-transports.md](03-transports.md) | Transports, fallbacks, app-chosen mode, restricted networks |
| [04-connectivity.md](04-connectivity.md) | LAN discovery, relay, gateway, NAT traversal, path switching |
| [05-channels-and-delivery.md](05-channels-and-delivery.md) | Streams, messages, datagrams, persistence |
| [06-network-robustness.md](06-network-robustness.md) | Weak-network targets |
| [07-media.md](07-media.md) | Real-time media transport crate |
| [08-platforms-and-integration.md](08-platforms-and-integration.md) | Platforms, distribution, reference server, scale |
| [09-m1-scope.md](09-m1-scope.md) | Milestone 1 |
| [../roadmap.md](../roadmap.md) | Milestones |

## 4. Conventions

- **MUST / SHOULD / MAY** as in RFC 2119. IDs like `QF-TR-05` for referencing from designs, code and tests.
- **Requirements:** decided with the project owner. **Proposed:** added while writing; **needs the owner's confirmation**. **Open questions:** not yet discussed.

## 5. Goals

| ID | Goal |
|---|---|
| G-1 | One communication foundation reused by several products. |
| G-2 | Secure by default: authenticated devices, encrypted channels, relay can't read E2EE live channels. |
| G-3 | Works on a LAN with no server, after a device has received a credential once. |
| G-4 | Reliable on bad mobile networks and gets through restrictive networks. |
| G-5 | Runs on desktop, Linux server, iOS, Android, WebAssembly and embedded Linux. |
| G-6 | Relay and gateway scale horizontally to the largest consumer's needs (qingfeng-im: 10M+ concurrent). |
| G-7 | The project is also how the owner learns Rust: built step by step, understanding before speed. |

## 6. Glossary

| Term | Meaning |
|---|---|
| **Device** | One installation using qingfeng-rust, with its own key pair. |
| **Device ID** | Identifier derived from the device's public key. |
| **Credential** | A signed statement from an issuer binding a device ID and public key to application-defined attributes (e.g. user, device type). |
| **Issuer** | The service that signs credentials (e.g. qingfeng-java for qingfeng-im). qingfeng-rust provides the library. |
| **Verifier** | Code that decides whether a peer's credential is acceptable. Pluggable. |
| **Visibility policy** | App-supplied rule deciding who may discover a device on the LAN. |
| **Relay** | Server forwarding traffic between peers that can't connect directly. |
| **Gateway** | Server endpoint holding devices' long-lived connections. |
| **Path** | How traffic reaches a peer: LAN direct, internet direct (hole punching), or relay. |
| **E2EE live channel** | A channel encrypted between two online peers; a relay in between only sees ciphertext. |
