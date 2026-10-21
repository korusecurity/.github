<p align="center">
  <img src="banner.png" alt="Koru: deception-first runtime defense for the AI era" width="100%">
</p>

<h3 align="center"><strong>Deception-first runtime defense for the AI era: trap AI attack agents, catch hijacked AI agents.</strong></h3>

<p align="center">
  <a href="https://github.com/korusecurity/koru/actions/workflows/ci.yml"><img src="https://github.com/korusecurity/koru/actions/workflows/ci.yml/badge.svg" alt="CI"></a>
  <a href="https://github.com/korusecurity/koru/releases"><img src="https://img.shields.io/github/v/release/korusecurity/koru" alt="Release"></a>
  <a href="https://github.com/korusecurity/koru/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-Apache--2.0-blue.svg" alt="License"></a>
</p>

## What is Koru?

Koru is an open-source, deception-based runtime security tool for Linux servers. It plants
honeytokens (decoy credentials, fake SSH keys, decoy `.env` files, fake MCP configuration files
and AI-bait notes) and detects access to them at the kernel level with eBPF, before any
credential is used. It then traces the accessing process through its lineage and reports whether
the reader looks like a human, an AI agent, or a possibly hijacked AI agent.

## Why Koru

- **Access detection before credential use.** Koru alerts when a decoy file is read on the
  host, not when a fake key is used somewhere else.
- **AI-agent attribution.** Process lineage and agent signatures (Claude Code, Codex, Cursor,
  Gemini CLI, MCP servers and others) separate a human from an AI agent and from a possibly
  hijacked one.
- **Local-first and open.** Apache-2.0, no telemetry. Event data stays on the server.

In the end-to-end suite, notification latency was p95 1.06 s; the measurements are in
[`docs/reports/metrics.md`](https://github.com/korusecurity/koru/blob/main/docs/reports/metrics.md).
Koru detects and notifies; it does not block attacks.

## Repositories

- **[koru](https://github.com/korusecurity/koru)**: the eBPF honeytoken access-detection tool for Linux.

## Quick start

```bash
git clone https://github.com/korusecurity/koru.git
cd koru
cargo build --release --locked --bin koru
./target/release/koru decoy validate packages/decoys
TMP=$(cd "$(mktemp -d)" && pwd -P); mkdir -p "$TMP/home"
./target/release/koru decoy plant --package packages/decoys --root "$TMP/home" --state "$TMP/state"
./target/release/koru decoy list --state "$TMP/state"
./target/release/koru decoy uninstall --state "$TMP/state"
```

This plants, lists and removes decoys in a throwaway directory and does not touch your real home
directory.

## Roadmap

- **Available now:** version 0.1.0 with decoys, detection, event store, notifications and a
  hardened signed distribution.
- **Next:** an isolated trap VM for attacker sessions.
- **Later:** session termination and managed multi-server features.

## Security, contributing and contact

- **Security policy:** [SECURITY.md](https://github.com/korusecurity/koru/blob/main/SECURITY.md).
  Report vulnerabilities to security@korusecurity.com.
- **Contributing:** [CONTRIBUTING.md](https://github.com/korusecurity/koru/blob/main/CONTRIBUTING.md)
- **Contact:** contact@korusecurity.com
- **Website:** <https://korusecurity.com>

<sub>Koru is an open-source project by Koru Security.</sub>
