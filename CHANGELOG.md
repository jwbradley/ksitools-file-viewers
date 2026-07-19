# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html)
for tagged releases once they begin.

## [Unreleased]

### Added

- **Migration engagement suite** (client cutover / multi-cloud support):
  - **Excel inventory** (`ksitools-excel-viewer.html`) — `.xlsx` OOXML sheet tables offline
  - **Secret / PII redactor** (`ksitools-redactor.html`) — safer ticket share-out
  - **Cloud audit log** — CloudTrail + Azure Activity + GCP audit unified table
  - **Network rules** — AWS SG/NACL + Azure NSG flatten + risk flags
  - **SQL dump** — DDL/DML outline for schema migrations
  - **Docker Compose**, **CloudFormation**, **ARM/Bicep**, **Helm**
  - **OpenAPI**, **SARIF/findings**, **SBOM**, **SAML**
- **Archive viewer:** `.tar.gz`/`.tgz` via browser `DecompressionStream`, ZIP64 EOCD, small text member preview
- **CIDR calculator:** full **IPv6** support (contains/overlap/split/bulk)
- Hub page regrouped into Migration / IaC / K8s / Security / Data sections

- **PDF → Markdown** (`ksitools-pdf-viewer.html`) — local PDF text extraction to Markdown via embedded PDF.js; heading heuristics, page markers, front matter, copy/save `.md` / `.txt`; hub + docs updated
- Initial public repository packaging for the KSI Tools single-file viewers:
  - JSON, YAML, XML, CSV/TSV, Config, Log, Diff, Hex, Markdown, SQLite
- Hub page (`index.html`) for local browsing
- Documentation suite: README, privacy, architecture, per-viewer notes, third-party notices
- Community files: LICENSE (MIT), SECURITY, CONTRIBUTING, CODE_OF_CONDUCT, issue/PR templates
- **DevOps-focused viewers** (pure JS, no new third-party embeds):
  - **JWT & Certs** — decode JWT claims; inspect PEM/X.509 (subject, SANs, validity); paste or drop
  - **HAR** — HTTP Archive table, filters, waterfall, header/body/timing detail
  - **Archive** — ZIP/JAR/TAR member listing without extract
  - **TOML** — structured tree for Cargo/pyproject/app configs
  - **NDJSON / JSONL** — line-oriented JSON streams as a filterable table
  - **Cloud / DevOps toolkit:**
  - **Terraform plan/state** — action filters, before/after, secret masking
  - **HCL** — `.tf` block outline
  - **kubeconfig** — contexts with masked credentials
  - **Kubernetes multi-doc YAML** — kind/name/namespace index
  - **IAM policy** — statement review + broad-permission heuristics
  - **Base64 / K8s Secrets** — decode Secret data maps offline
  - **CIDR calculator** — subnet/contains/overlap/split
  - **CloudTrail** — event browser for JSON/JSONL dumps
  - **Dockerfile** — stages and hygiene warnings
  - **SSH keys** — SHA256 fingerprints for `.pub` / authorized_keys

### Changed

- **SQLite viewer:** multi‑GB databases open lazily (Worker + File range reads) with pagination, cell truncation, and WAL warnings — no full-file ArrayBuffer load above 64 MB

- Product branding set to **KSI Tools**; viewer files named `ksitools-*-viewer.html`; docs and hub updated; intended GitHub repo `ksitools-file-viewers`
- Hub, README, architecture, and viewer reference updated for the five new DevOps tools

### Notes

Viewer feature sets shipped as they existed prior to the first repository packaging
(July 2026). See [docs/viewers.md](docs/viewers.md) for current capabilities.
