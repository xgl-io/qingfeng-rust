# 01 — Identity and Trust

## Requirements

| ID | Requirement |
|---|---|
| QF-ID-01 | Each device MUST generate its own key pair locally. The private key MUST NOT leave the device. |
| QF-ID-02 | The device ID MUST be derived from the device's public key, so it can't be forged or claimed by another device. |
| QF-ID-03 | qingfeng-rust MUST define a **standard signed credential format** binding device ID, public key and application-defined attributes (e.g. user ID, device type). |
| QF-ID-04 | qingfeng-rust MUST provide an **issuer library**, so any service (e.g. qingfeng-java, or a dev tool) can issue credentials in the standard format. |
| QF-ID-05 | Credentials MUST be verifiable **offline**, so peers can authenticate each other on a LAN with no server. |
| QF-ID-06 | Applications MUST be able to plug in a **custom verifier** (e.g. to check a different credential type, or extra rules). |
| QF-ID-07 | Device policies (how many devices per user, device types, replacement rules) belong to applications, not qingfeng-rust. |

## Proposed (needs confirmation)

| ID | Proposal |
|---|---|
| QF-ID-P1 | Private keys SHOULD be stored in OS secure storage where available, behind a pluggable key-store interface (needed for web and embedded). |
| QF-ID-P2 | Credentials SHOULD carry an expiry time and be renewable, so a revoked device is trusted offline for a limited time only. |
| QF-ID-P3 | The framework SHOULD support revocation lists that issuers publish and peers cache for offline checks. |
| QF-ID-P4 | The credential format SHOULD be specified independently of Rust (e.g. a Protocol Buffers schema), so qingfeng-java can issue credentials without Rust. |

## Open questions

| # | Question |
|---|---|
| QF-ID-Q1 | Signature algorithm and device ID format? (Design note in M1 step 1.6.) |
| QF-ID-Q2 | Does qingfeng-java call the Rust issuer library (via FFI or a sidecar service), or implement the format in Java? |
| QF-ID-Q3 | Key verification for users (safety numbers, QR): framework helper or app feature? |
| QF-ID-Q4 | Key rotation for a device without changing its device ID? |
