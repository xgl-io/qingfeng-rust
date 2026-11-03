# 08 — Platforms and Integration

## Requirements

### Platforms
| ID | Requirement |
|---|---|
| QF-PLT-01 | qingfeng-rust MUST support: **Windows, macOS, Linux desktops**; **Linux servers**; **iOS and Android**; **Web (WebAssembly)**; **embedded Linux / IoT** (not microcontrollers without an OS). |
| QF-PLT-02 | Where a platform can't support a feature (e.g. browsers have no raw UDP/TCP and no LAN discovery), the library MUST report it clearly and use what the platform allows. |

### Distribution
| ID | Requirement |
|---|---|
| QF-PLT-03 | qingfeng-rust is **closed source** and consumed as a **private git dependency** with version tags. |

### Server side
| ID | Requirement |
|---|---|
| QF-PLT-04 | Server components (gateway, relay, discovery/coordination) are provided as **libraries plus a reference server binary**. **Each product deploys its own instance** and plugs in its own credential verification and policies. |
| QF-PLT-05 | The server libraries MUST scale **horizontally**, up to the largest consumer's target (qingfeng-im: **10M+ concurrent connections**). Early milestones may run small but MUST NOT block this. |

## Proposed (needs confirmation)

| ID | Proposal |
|---|---|
| QF-PLT-P1 | Version tags follow **semantic versioning**; breaking API changes only in major versions (pre-1.0: minor). |
| QF-PLT-P2 | Every public API is documented, with runnable examples. |
| QF-PLT-P3 | Workspace layout (to decide in the design): `qf-protocol`, `qf-identity`, `qf-transport`, `qf-net` (connectivity), `qf-channel`, `qf-client`, `qf-server`, `qf-media`, `qf-chaos` (simulator), `qf-ffi` (bindings). |
| QF-PLT-P4 | CI builds and tests on Windows, macOS and Linux. |

## Open questions

| # | Question |
|---|---|
| QF-PLT-Q1 | Mobile and other languages: does qingfeng-rust ship bindings (e.g. UniFFI for Swift/Kotlin), or does each app make its own? |
| QF-PLT-Q2 | Minimum OS versions? |
| QF-PLT-Q3 | Where is the private git repository hosted? |
| QF-PLT-Q4 | Licence for the closed-source code, and checking dependency licences (e.g. avoiding AGPL)? |
