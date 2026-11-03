# qingfeng-rust — Roadmap

> Status: **draft v2** · 2026-11-02 · Rewritten for qingfeng-rust as a basic communication layer.
> Requirements: [requirements/](requirements/00-overview.md). App milestones moved to `qingfeng-im/docs/roadmap.md`.

## 1. How this roadmap works

- **Dual purpose:** every milestone builds part of the framework *and* teaches a set of Rust skills (`G-7`).
- **No dates.** Sizes only: **S < M < L < XL**.
- **Solo** (with Claude): milestones run one after another.
- **Detail fades with distance:** M1 is broken into steps; later milestones are sketches. **Reviewed at the end of every milestone** (§5).
- **Every milestone ends with a demo.**
- **No separate warm-up stage:** Rust is learned while building M1; its early steps are small and serve as exercises.
- **Design before code:** steps marked **📐** start with a short design note in `docs/design/`, discussed first. Technology names (Tokio, QUIC, …) are **candidates** until a design note decides.

## 2. Overview

| # | Milestone | Size | Depends on | Demo |
|---|---|---|---|---|
| M1 | Networking foundation (TLS only) | L | — | Two desktops exchange reliable messages by command line over relay and LAN, through the network simulator |
| M2 | E2EE live channels | M | M1 | Same, and the relay only sees ciphertext |
| M3 | First release for consumers (v0.1) | M | M2 | qingfeng-im's first milestone builds against a tagged version |
| M4 | NAT hole punching | L | M3 | Two devices behind different NATs connect directly; relay only as fallback |
| M5 | Mobile: iOS + Android | XL | M3 | A phone and a desktop exchange messages |
| M6 | Real-time media crate | XL | M4 | An audio/video stream between two devices on a weak network |
| M7 | Web (WebAssembly) | L | M3 | A browser and a desktop exchange messages |
| M8 | Embedded Linux / IoT | M | M3 | A Raspberry Pi-class board as a peer |
| S | **Scale-out track** (alongside, from M3) | XL | — | Load-test results |

**The order of M4–M8 is proposed, not decided.** It should follow what the consumers need next (qingfeng-im calls need M6; its mobile apps need M5).

```
M1 ─▶ M2 ─▶ M3 ─┬─▶ M4 ─▶ M6
                ├─▶ M5
                ├─▶ M7
                ├─▶ M8
                └─▶ S (alongside) ─▶ hardened + audited before any consumer launches
```

---

## 3. M1 — Networking foundation (detailed)

**Goal:** [09-m1-scope.md](requirements/09-m1-scope.md).

> Steps marked *(M1-Px)* rely on proposals that aren't confirmed yet; confirm or change them before that step.

Each step lists: **Build** · **Rust concepts** · **Read first** (*The Rust Programming Language*, "the Book") · **Done when**.

#### 1.1 Workspace and tooling · S
- **Build:** Cargo workspace (`crates/`), `git init`, `rustfmt`, `clippy`, a `README`, one crate with one function and one test.
- **Rust concepts:** `cargo`, crates and packages, modules, functions, variables, basic types, `cargo test`.
- **Read first:** Book ch. 1–3, 7.
- **Done when:** `cargo build`, `cargo test` and `cargo clippy` pass.

#### 1.2 📐 Frame encoder/decoder · M
- **Design:** frame layout (header fields, length, maximum size) and message encoding.
- **Build:** encode and decode frames to and from bytes; errors for truncated, oversized or malformed input. No networking.
- **Rust concepts:** **ownership and borrowing**, slices, `struct`, `enum`, `match`, `Vec<u8>`, `Result` and `?`, custom errors, unit tests.
- **Read first:** Book ch. 4–6, 8, 9, 11.
- **Done when:** round-trip tests and every error case pass.

#### 1.3 Blocking TCP echo · S
- **Build:** server and client with `std::net`, one thread per client, sending frames from 1.2.
- **Rust concepts:** `std::net`, `Read`/`Write`, threads, `move` closures, `Arc`.
- **Read first:** Book ch. 13, 16, and the ch. 21 web server project.
- **Done when:** several terminal clients exchange framed messages.

#### 1.4 Async with Tokio · M
- **Build:** rewrite 1.3 with Tokio: many clients, broadcast, clean shutdown.
- **Rust concepts:** `async`/`await`, futures, tasks, channels (`mpsc`, `broadcast`), `Arc<Mutex<_>>`, `Send` and `'static`.
- **Read first:** Book ch. 17, the Tokio tutorial.
- **Done when:** 100 simulated clients through one server without errors.

#### 1.5 Network simulator (`qf-chaos`) · M  *(M1-P3)*
- **Build:** a proxy adding latency, loss, bandwidth limits and forced disconnects, with a **bad mobile** preset (`QF-NR-01`).
- **Rust concepts:** command-line parsing, config with `serde`, randomness, Tokio timers, splitting I/O directions.
- **Read first:** Book ch. 10.
- **Done when:** the 1.4 echo works through it, with measurable delay and loss.

#### 1.6 📐 Identity, credentials, issuer · M
- **Design:** key and signature algorithm, device ID format, credential format (`QF-ID-Q1`, `M1-Q1`), verifier interface.
- **Build:** key generation, device ID, issuer library, **dev issuer command-line tool**, offline verification, a verifier **trait** for custom verifiers (`M1-01`–`M1-03`).
- **Rust concepts:** traits as plug-in points, crypto crates, `thiserror`/`anyhow`, serialization, files on disk.
- **Read first:** Book ch. 10 (again), ch. 15.
- **Done when:** valid credentials verify; tampered, expired or wrong-key ones are rejected (tests); a test verifier can be plugged in.

#### 1.7 📐 Transports and authenticated connection · L
- **Design:** transport abstraction; QUIC and TCP+TLS first; handshake; how the credential is presented.
- **Build:** client–server connection with credential checking (`M1-04`); a transport trait so the app can pick one (`QF-TR-05`).
- **Rust concepts:** trait objects (`dyn Trait`), generics, lifetimes, configuring TLS.
- **Read first:** Book ch. 18, 20.
- **Done when:** valid devices are accepted and invalid ones rejected, on both transports, through the simulator.

#### 1.8 📐 Liveness and reconnect · M
- **Design:** heartbeats, dead-connection timeout, backoff with jitter, network-change handling.
- **Build:** detection, automatic reconnect, connection state machine (`M1-05`, `QF-NR-06`).
- **Rust concepts:** `tokio::select!`, enums as state machines, `Instant`/`Duration`, cancellation.
- **Read first:** Book ch. 19.
- **Done when:** the client recovers by itself from simulated disconnects and network switches.

#### 1.9 📐 Reliable messages with persistent storage · L
- **Design:** message IDs, acknowledgements, outbox, dedup, resume; the **storage trait**.
- **Build:** reliable messages (`M1-07`) with a **SQLite default** storage implementation; also an in-memory one for tests.
- **Rust concepts:** designing a trait with two implementations, async database access, transactions, integration tests.
- **Done when:** messages survive a client restart, and `QF-NR-P2` passes client ↔ server.

#### 1.10 📐 Relay and gateway · M
- **Design:** gateway connection registry, routing between two online devices, behavior when the peer is offline (the sender's outbox keeps the message, `QF-CH-Q2`).
- **Build:** reference server forwarding between online peers (`M1-08`, `QF-CON-05`).
- **Rust concepts:** shared server state, `HashMap` of live connections, cleanup on disconnect.
- **Done when:** `QF-NR-P2` passes device ↔ relay ↔ device.

#### 1.11 📐 LAN discovery and direct path · L  *(M1-P2)*
- **Design:** discovery that excluded devices can't read (`QF-CON-02`); the visibility-policy trait.
- **Build:** discovery plus direct path with no server (`M1-09`); an allow-list policy as the example.
- **Rust concepts:** UDP and multicast, `#[cfg(target_os = ...)]`, OS permissions.
- **Done when:** two machines on a LAN, server off, exchange reliable messages.

#### 1.12 Path switching · M
- **Build:** move between LAN and relay automatically, no loss or duplicates (`M1-10`, `QF-CON-P2`).
- **Rust concepts:** coordinating several connections, more advanced `select!`.
- **Done when:** `QF-NR-P2` passes while the LAN is repeatedly cut and restored.

#### 1.13 Restricted networks and remaining transports · M
- **Build:** automatic fallback; HTTPS/WebSocket through an HTTP proxy with OS proxy settings; IPv6/NAT64; app-chosen transport for every mode (`M1-06`).
- **Rust concepts:** layered I/O adapters, `AsyncRead`/`AsyncWrite`, feature flags.
- **Done when:** connects with UDP blocked, through a test proxy, on IPv6-only, and in each forced mode.

#### 1.14 Test client and acceptance · S  *(M1-P1, M1-P4)*
- **Build:** a friendly command-line test client; full acceptance tests; docs.
- **Done when:** the demo runs, every `M1-*` requirement is checked, and the review (§5) is written.

**M1 Rust skills overall:** ownership and borrowing, error handling, traits and generics as plug-in points, testing, threads, async Tokio, networking (TCP, UDP, TLS, QUIC), state machines, storage.

---

## 4. Later milestones (sketch)

**M2 — E2EE live channels · M.** Channels encrypted between the two peers, so the relay forwards ciphertext only (`QF-SEC-02`); forward secrecy (`QF-SEC-P2`); a secure channel over plain TCP (`QF-TR-P1`). Starts with a small crypto learning exercise (a toy handshake, never shipped), then uses an established protocol. *Rust:* crypto APIs, `zeroize`, property tests, fuzzing decoders.

**M3 — First release v0.1 · M.** API review, docs and examples (`QF-PLT-P2`), semantic versioning and a git tag (`QF-PLT-03`, `QF-PLT-P1`), a packaged reference server, CI on three OSes. After this, qingfeng-im can start. *Rust:* API design, `rustdoc`, examples, CI.

**M4 — NAT hole punching · L.** Server-coordinated direct internet connections with relay fallback (`QF-CON-06`), path preference (`QF-CON-P1`). *Rust:* UDP techniques, more state machines, testing with simulated NATs.

**M5 — iOS + Android · XL.** Mobile builds, background and battery limits, bindings (`QF-PLT-Q1`). *Rust:* cross-compiling, FFI, generated bindings.

**M6 — Real-time media crate · XL.** Separate crate (`QF-MED-01`–`QF-MED-04`): congestion control, bitrate adaptation, loss recovery; WebRTC vs. custom decided first (`QF-MED-Q1`). *Rust:* real-time pipelines, low-latency networking, performance.

**M7 — Web (WebAssembly) · L.** Browser transports (`QF-TR-P3`), IndexedDB storage (`QF-CH-04`), clear reporting of unsupported features (`QF-PLT-02`). *Rust:* `wasm-bindgen`, async in the browser, conditional compilation.

**M8 — Embedded Linux / IoT · M.** Small-footprint builds for boards like Raspberry Pi. *Rust:* cross-compiling, binary size, feature flags.

### S — Scale-out track (alongside, from M3)
| Stage | Content |
|---|---|
| S1 | Gateway and relay across many servers: connection routing, presence between servers |
| S2 | Load-testing harness (simulated millions of clients), benchmarks |
| S3 | Horizontal scaling up to 10M+ concurrent connections (`QF-PLT-05`) |
| S4 | Hardening and **external security audit** (`QF-SEC-P4`) before any consumer launches publicly |

---

## 5. Milestone review (end of every milestone)

Copy into `docs/reviews/Mx-review.md`:

```markdown
# Mx review
- Demo: works? (notes)
- Requirements done: IDs; anything moved and why
- What I learned in Rust: concepts I now understand; what's still unclear
- What was hard / took longer than expected
- Decisions made (link the design notes)
- Changes to this roadmap
- Open questions resolved / new ones
```

## 6. Open questions

| # | Question |
|---|---|
| RM-Q1 | Order of M4–M8, depending on what qingfeng-im needs first? |
| RM-Q2 | Which desktop OS is primary for M1 development (`M1-Q3`)? |
