<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/sphragis-oss/.github/main/assets/banner.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/sphragis-oss/.github/main/assets/banner.svg">
  <img alt="Sphragis" src="https://raw.githubusercontent.com/sphragis-oss/.github/main/assets/banner.svg" width="100%">
</picture>

<br/>

<p align="center">
  <strong>The EU AI Act compliance gateway you actually control.</strong>
</p>

<p align="center">
  <a href="https://github.com/sphragis-oss/sphragis/releases"><img src="https://img.shields.io/github/v/release/sphragis-oss/sphragis?include_prereleases&style=flat-square&color=d92b21&label=release" alt="Release"></a>
  <a href="https://github.com/sphragis-oss/sphragis/blob/main/LICENSE"><img src="https://img.shields.io/github/license/sphragis-oss/sphragis?style=flat-square&color=d92b21" alt="License"></a>
  <a href="https://goreportcard.com/report/github.com/sphragis-oss/sphragis"><img src="https://goreportcard.com/badge/github.com/sphragis-oss/sphragis?style=flat-square" alt="Go Report Card"></a>
  <a href="https://sphragis.eu"><img src="https://img.shields.io/badge/website-sphragis.eu-111111?style=flat-square" alt="Website"></a>
</p>

---

## What is Sphragis?

**Sphragis** is a self-hosted gateway that sits between your apps and any OpenAI- or Anthropic-compatible LLM, with first-class support for **Claude Code** and the Anthropic SDKs. It strips personal data out of every request *before* it leaves your network and writes a tamper-evident, hash-chained record of each call. There is no SaaS in the data path: your prompts never reach us, because there is no "us" in the path.

The name is the Greek σφραγίς (*sphragís*), the seal pressed into wax to prove a document is authentic and untampered. That is exactly what the audit log does.

```bash
# point any client at the gateway; PII is tokenized before it leaves the machine
export SPHRAGIS_UPSTREAM_BASE_URL=https://api.anthropic.com
sphragis serve                                  # listens on :8787

export ANTHROPIC_BASE_URL=http://localhost:8787 # Claude Code now runs through it

# later: prove the audit log was never altered
sphragis verify ~/.sphragis/audit.jsonl
# OK: 42 records, chain intact
```

### Key Features

| Feature | Description |
|---------|-------------|
| **Local PII redaction** | Emails, phones, IBANs, cards, SSNs, secrets, API keys, JWTs and private keys are replaced with stable tokens (`[EMAIL_1]`, `[IBAN_1]`) before a byte leaves the machine |
| **Tamper-evident audit log** | Every call is hash-chained in append-only JSONL; reordering, editing or dropping any record breaks `sphragis verify` |
| **Fails closed** | If the audit write fails, the gateway refuses to forward the call |
| **One gateway, many clients** | Anthropic Messages API (Claude Code, Agent SDK) and OpenAI chat/responses (Codex, Cursor, LangChain) on a single proxy |
| **Optional public anchoring** | Timestamp the log's Merkle root via [OpenTimestamps](https://opentimestamps.org/); only the opaque root ever leaves your network |
| **Self-hosted** | A single Go binary, no SaaS in the data path, Apache 2.0 |

## Repositories

| Repository | Description |
|------------|-------------|
| [`sphragis`](https://github.com/sphragis-oss/sphragis) | Core gateway: proxy, PII/secret redaction, hash-chained audit log, `verify` and `anchor` |
| [`homebrew-sphragis`](https://github.com/sphragis-oss/homebrew-sphragis) | Homebrew tap for installing the `sphragis` CLI on macOS |

<br/>

<p align="center">
  <sub>
    <a href="https://sphragis.eu">sphragis.eu</a> · Apache-2.0 · status: early · built in the open, headed for the <a href="https://www.cncf.io/">CNCF</a> Sandbox
  </sub>
</p>
