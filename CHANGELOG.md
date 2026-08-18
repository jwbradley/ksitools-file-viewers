# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html)
for tagged releases once they begin.

## [Unreleased]

### Added

- **Thirteen new viewers** (ported from upstream friend toolkit, KSI-branded):
  - **Crontab** — plain-English schedule + next run times
  - **EBCDIC / Fixed-width** — CP037/1047/500… + copybook layouts
  - **EVTX** — Windows event log binary XML → filterable table
  - **Hash & Checksum** — MD5 / SHA family / CRC32 with verify
  - **Hex encode / decode** — hex2bin/bin2hex codec (separate from Hex dump viewer)
  - **JSON Query (jq-style)** — offline jq subset for JSON/NDJSON
  - **mbox / EML** — RFC 5322 mailbox + MIME parts
  - **Parquet** — pure-JS columnar reader (schema + rows)
  - **PCAP / PCAPNG** — L2–L4 dissection + hex dump
  - **PKCS#12 / PFX** — cert/key bundle inspect (keys never displayed)
  - **Regex Tester** — live JS regex with highlight + replace
  - **Timestamp & Date** — epoch / Julian / DB2 / ISO + world clocks
  - **URL Encode / Decode** — percent-coding + URL component inspect
- Hub regrouped with a **Utilities** section; README and viewer reference updated
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
- **PDF → Markdown** (`ksitools-pdf-viewer.html`) — local PDF text extraction to Markdown via embedded PDF.js
- Initial public repository packaging for the KSI Tools single-file viewers (JSON, YAML, XML, CSV/TSV, Config, Log, Diff, Hex, Markdown, SQLite)
- Hub page (`index.html`), docs suite, and community files (LICENSE MIT, SECURITY, CONTRIBUTING, CODE_OF_CONDUCT)
- **DevOps-focused viewers:** JWT & Certs, HAR, Archive, TOML, NDJSON/JSONL, Terraform plan/state, HCL, kubeconfig, Kubernetes multi-doc YAML, IAM policy, CIDR, CloudTrail, Dockerfile, SSH keys

### Changed

- **Base64 / Secrets** upgraded to live two-pane encode/decode workspace: URL-safe (base64url), MIME wrap, strip-whitespace option, line-by-line mode, swap, Live mode, binary Save, and clearer K8s Secret/ConfigMap data-key panel
- **SQLite viewer:** multi‑GB databases open lazily (Worker + File range reads) with pagination, cell truncation, and WAL warnings — no full-file ArrayBuffer load above 64 MB
- Product branding set to **KSI Tools**; viewer files named `ksitools-*-viewer.html`; docs and hub updated; intended GitHub repo `ksitools-file-viewers`

### Notes

See [docs/viewers.md](docs/viewers.md) for current capabilities per tool.
