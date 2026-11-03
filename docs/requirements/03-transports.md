# 03 — Transports

## Requirements

| ID | Requirement |
|---|---|
| QF-TR-01 | qingfeng-rust MUST support these transports: **QUIC**, **TCP**, **TLS over TCP** (port 443), and **HTTPS / WebSocket** for proxy-only networks. |
| QF-TR-02 | **UDP blocked:** when QUIC fails, the client MUST fall back to TCP on port 443. |
| QF-TR-03 | **HTTP proxy only:** the client MUST connect through an HTTP(S) proxy (`CONNECT`), using traffic that looks like normal HTTPS or WebSocket, and MUST honor the OS proxy settings. |
| QF-TR-04 | **IPv6-only / NAT64:** client and server MUST work fully over IPv6, including NAT64. No hard-coded IPv4 addresses. |
| QF-TR-05 | By default the transport is chosen **automatically with fallback**. Applications MUST also be able to **choose a specific transport** (e.g. qingfeng-vpn's TCP, QUIC and TLS modes). |
| QF-TR-06 | Applications use one channel API regardless of the transport underneath. |

## Proposed (needs confirmation)

| ID | Proposal |
|---|---|
| QF-TR-P1 | The **TCP** transport (without TLS) is encrypted by qingfeng-rust's own secure channel, so it is never plaintext (see qingfeng-vpn `VPN-Q5`). |
| QF-TR-P2 | Automatic fallback order: QUIC → TLS/443 → HTTPS/WebSocket via proxy; the working choice is remembered per network. |
| QF-TR-P3 | On WebAssembly only browser transports are used: WebSocket, and WebTransport where available. |

## Open questions

| # | Question |
|---|---|
| QF-TR-Q1 | Proxies needing authentication (NTLM, Kerberos, basic)? |
| QF-TR-Q2 | Networks that intercept TLS (company inspection)? |
| QF-TR-Q3 | Should plain TCP also be used on the LAN, or QUIC only? |
