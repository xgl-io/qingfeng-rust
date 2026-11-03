# 04 — Connectivity: LAN, Relay, Gateway, NAT Traversal

## Requirements

### LAN direct
| ID | Requirement |
|---|---|
| QF-CON-01 | Devices on the same LAN MUST be able to communicate **with no server reachable**, using credentials obtained earlier (`QF-ID-05`). |
| QF-CON-02 | LAN discovery MUST take an app-supplied **visibility policy** deciding who may discover and identify a device (e.g. qingfeng-im: existing contacts only). Devices MUST NOT be identifiable by anyone the policy excludes. |
| QF-CON-03 | LAN discovery MUST work over IPv6 as well as IPv4. |

### Relay and gateway
| ID | Requirement |
|---|---|
| QF-CON-04 | qingfeng-rust MUST provide **relay** and **gateway** as server libraries plus a reference server binary (`QF-PLT-04`). |
| QF-CON-05 | The relay forwards traffic **only between peers that are online at the same time**. Storing data for offline devices is the application's job. |

### Internet direct
| ID | Requirement |
|---|---|
| QF-CON-06 | Peers MUST try **direct internet connections through NATs** (hole punching, coordinated by the server) and use the relay only when that fails. May come after M1. |

## Proposed (needs confirmation)

| ID | Proposal |
|---|---|
| QF-CON-P1 | Path preference: LAN direct → internet direct → relay. |
| QF-CON-P2 | A channel SHOULD move between paths automatically (e.g. a laptop leaving the office) without losing or duplicating reliable messages. |
| QF-CON-P3 | When LAN discovery can't work (multicast blocked, Wi-Fi client isolation, OS permission denied), fall back to other paths without user action, and allow a manually entered peer address. |
| QF-CON-P4 | Handle OS requirements for LAN use: Windows firewall, macOS *Local Network* permission, coexisting with Avahi on Linux. |
| QF-CON-P5 | Applications MAY disable direct internet connections (they reveal IP addresses to peers). |

## Open questions

| # | Question |
|---|---|
| QF-CON-Q1 | Does "LAN" mean one network segment only, or also routed company networks and VPNs? |
| QF-CON-Q2 | How does an offline LAN device learn about revoked credentials (see `QF-ID-P2`, `QF-ID-P3`)? |
| QF-CON-Q3 | Is the relay a separate service from the gateway, or the same process? |
