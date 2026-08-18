# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html)
for tagged releases once they begin.

## [Unreleased]

### Added

- **Tier-3 enterprise / migration viewers**:
  - **LDAP LDIF** (`ksitools-ldif-viewer.html`) — RFC 2849-ish entry inventory; credential attributes masked by default; duplicate-DN / changetype / external-URL flags
  - **Dependency lockfiles** (`ksitools-lockfile-viewer.html`) — pinned packages from package-lock / npm-shrinkwrap, go.mod/go.sum, Cargo.lock, poetry.lock, requirements.txt (+ Pipfile.lock / composer.lock / Gemfile.lock)
- Hub gains an **Enterprise / migration** section; README / architecture / viewer reference updated
- **Tier-2 platform ops viewers** for modern DevOps admin review:
  - **Observability** (`ksitools-observability-viewer.html`) — Prometheus scrape configs & alert/record rules, Alertmanager routes/receivers, Grafana dashboard panel inventory (secrets masked)
  - **CI pipeline** (`ksitools-ci-viewer.html`) — GitHub Actions / GitLab CI / Azure Pipelines jobs & steps; broad-perms and plaintext-secret flags
  - **SSH config** (`ksitools-ssh-config-viewer.html`) — Host/Match outline; ProxyJump chains; StrictHostKeyChecking / ForwardAgent flags
  - **WireGuard** (`ksitools-wireguard-viewer.html`) — Interface/peer outline; PrivateKey/PSK masked by default; full-tunnel AllowedIPs flags
  - **Ansible** (`ksitools-ansible-viewer.html`) — inventory INI/YAML hosts/groups + playbook plays/roles/tasks summary
- Hub gains a **Platform ops** section; README / architecture / viewer reference updated
- **Tier-1 host & network config viewers** for everyday DevOps admin review:
  - **systemd** (`ksitools-systemd-viewer.html`) — unit/timer outline, Exec*/User/WantedBy summary, root/secret-env/hygiene flags
  - **Proxy / LB** (`ksitools-proxy-viewer.html`) — nginx / Apache / HAProxy / Caddy block outline with TLS & cleartext-upstream flags
  - **Firewall** (`ksitools-firewall-viewer.html`) — iptables-save / nftables / UFW / firewalld flatten; world-open & SSH/RDP flags
  - **DNS / CoreDNS** (`ksitools-dns-viewer.html`) — BIND-style zones + Corefile plugins; wildcards, SOA, forward targets
- Hub gains a **Host & network config** section; README / architecture / viewer reference updated
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
