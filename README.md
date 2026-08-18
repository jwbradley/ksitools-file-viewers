# KSI Tools File Viewers

**Private, offline-first file viewers that run entirely in your browser.**

Drop a file onto a page. It is parsed and displayed locally. Nothing is uploaded, nothing is stored on a server, and there is no backend.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Privacy: local-only](https://img.shields.io/badge/Privacy-local--only-brightgreen.svg)](docs/privacy.md)
[![Dependencies: offline](https://img.shields.io/badge/Network-none%20required-lightgrey.svg)](docs/architecture.md)

---

## Why these exist

Ops, support, and engineering work often means opening someone else’s JSON export, a production log, a YAML config, or a SQLite dump — sometimes on a machine that should not send customer data anywhere.

These viewers are single HTML files you can open from disk, share on a USB stick, or host on a static site. Each one is self-contained: CSS, JS, and (where needed) embedded libraries ship inside the file. No CDN. No install. No account.

Some archive/Excel inflate paths use the browser built-in **DecompressionStream** API (no extra download).

## Viewers

### Migration engagement

| Viewer | File | What it does |
|--------|------|----------------|
| **Excel inventory** | [`ksitools-excel-viewer.html`](ksitools-excel-viewer.html) | `.xlsx` sheet tables (OOXML), filter, export CSV/JSON |
| **Secret / PII redactor** | [`ksitools-redactor.html`](ksitools-redactor.html) | Mask keys, tokens, PEMs, JWTs, emails (optional IPs/phones) for safer tickets |
| **Archive** | [`ksitools-archive-viewer.html`](ksitools-archive-viewer.html) | ZIP/JAR/TAR/**`.tar.gz`** listing, ZIP64, small text member preview |
| **CIDR** | [`ksitools-cidr-calculator.html`](ksitools-cidr-calculator.html) | **IPv4 + IPv6** subnet math, contains, overlap, split, bulk lists |
| **Cloud audit log** | [`ksitools-cloud-audit-viewer.html`](ksitools-cloud-audit-viewer.html) | CloudTrail + Azure Activity + GCP audit in one table |
| **Network rules** | [`ksitools-network-rules-viewer.html`](ksitools-network-rules-viewer.html) | AWS SG/NACL + Azure NSG flatten; world-open / SSH/RDP flags |
| **SQL dump** | [`ksitools-sql-dump-viewer.html`](ksitools-sql-dump-viewer.html) | `.sql` DDL/DML statement outline for schema migrations |

### Host & network config

| Viewer | File | What it does |
|--------|------|----------------|
| **systemd unit / timer** | [`ksitools-systemd-viewer.html`](ksitools-systemd-viewer.html) | Unit/timer section outline; User/ExecStart/WantedBy summary + hygiene flags |
| **Proxy / LB config** | [`ksitools-proxy-viewer.html`](ksitools-proxy-viewer.html) | nginx / Apache / HAProxy / Caddy outline; TLS & cleartext-upstream flags |
| **Firewall rules** | [`ksitools-firewall-viewer.html`](ksitools-firewall-viewer.html) | iptables-save / nftables / UFW / firewalld → rule table; world-open / SSH flags |
| **DNS / CoreDNS** | [`ksitools-dns-viewer.html`](ksitools-dns-viewer.html) | BIND zone records + CoreDNS Corefile plugins for cutovers |

### Platform ops

| Viewer | File | What it does |
|--------|------|----------------|
| **Observability** | [`ksitools-observability-viewer.html`](ksitools-observability-viewer.html) | Prometheus scrape/rules, Alertmanager routes/receivers, Grafana panel inventory |
| **CI pipeline** | [`ksitools-ci-viewer.html`](ksitools-ci-viewer.html) | GitHub Actions / GitLab CI / Azure Pipelines jobs & steps outline |
| **SSH config** | [`ksitools-ssh-config-viewer.html`](ksitools-ssh-config-viewer.html) | `~/.ssh/config` Host/Match blocks; ProxyJump + StrictHostKeyChecking flags |
| **WireGuard** | [`ksitools-wireguard-viewer.html`](ksitools-wireguard-viewer.html) | Interface/peers; keys masked by default; full-tunnel AllowedIPs flags |
| **Ansible** | [`ksitools-ansible-viewer.html`](ksitools-ansible-viewer.html) | Inventory (INI/YAML) + playbook plays/roles/tasks summary |

### Enterprise / migration

| Viewer | File | What it does |
|--------|------|----------------|
| **LDAP LDIF** | [`ksitools-ldif-viewer.html`](ksitools-ldif-viewer.html) | Directory entry inventory; credential attrs masked; duplicate-DN flags |
| **Dependency lockfiles** | [`ksitools-lockfile-viewer.html`](ksitools-lockfile-viewer.html) | Pins from npm / Go / Cargo / Poetry / pip (complements SBOM) |

### Cloud IaC & packaging

| Viewer | File | What it does |
|--------|------|----------------|
| **Terraform plan** | [`ksitools-terraform-plan-viewer.html`](ksitools-terraform-plan-viewer.html) | `terraform show -json` plan/state: actions filter, before/after, secret mask |
| **HCL** | [`ksitools-hcl-viewer.html`](ksitools-hcl-viewer.html) | `.tf` / `.hcl` block outline (resource/module/variable/…) |
| **CloudFormation** | [`ksitools-cloudformation-viewer.html`](ksitools-cloudformation-viewer.html) | Template resources or change-set Changes[] table |
| **ARM / Bicep** | [`ksitools-arm-bicep-viewer.html`](ksitools-arm-bicep-viewer.html) | Azure ARM JSON resources + Bicep syntactic outline |
| **Docker Compose** | [`ksitools-compose-viewer.html`](ksitools-compose-viewer.html) | Services, images, ports, depends_on, secret-like env |
| **Helm** | [`ksitools-helm-viewer.html`](ksitools-helm-viewer.html) | Chart.yaml metadata + values key outline |
| **Dockerfile** | [`ksitools-dockerfile-viewer.html`](ksitools-dockerfile-viewer.html) | Stages, ports, secret-like ENV warnings |

### Kubernetes & identity

| Viewer | File | What it does |
|--------|------|----------------|
| **kubeconfig** | [`ksitools-kubeconfig-viewer.html`](ksitools-kubeconfig-viewer.html) | Contexts/clusters/users with tokens & key data masked by default |
| **Kubernetes** | [`ksitools-k8s-viewer.html`](ksitools-k8s-viewer.html) | Multi-doc YAML index by kind/name/namespace |
| **Base64 / Secrets** | [`ksitools-base64-viewer.html`](ksitools-base64-viewer.html) | Live two-pane encode/decode (standard / URL-safe); K8s Secret/ConfigMap `data` maps |
| **PKCS#12 / PFX** | [`ksitools-pkcs12-viewer.html`](ksitools-pkcs12-viewer.html) | Inspect `.p12`/`.pfx` cert & key bundles; private key bytes never displayed |
| **IAM policy** | [`ksitools-iam-policy-viewer.html`](ksitools-iam-policy-viewer.html) | Statement review + broad wildcard heuristics |
| **JWT & Certs** | [`ksitools-jwt-cert-viewer.html`](ksitools-jwt-cert-viewer.html) | Decode JWT claims; inspect PEM/X.509 — local only, no verify |
| **SAML** | [`ksitools-saml-viewer.html`](ksitools-saml-viewer.html) | Metadata / assertion decode for SSO migrations (no verify) |
| **SSH keys** | [`ksitools-ssh-key-viewer.html`](ksitools-ssh-key-viewer.html) | Public key / authorized_keys SHA256 fingerprints |

### Security & APIs

| Viewer | File | What it does |
|--------|------|----------------|
| **OpenAPI** | [`ksitools-openapi-viewer.html`](ksitools-openapi-viewer.html) | Path/method inventory, tags, security schemes |
| **SARIF / findings** | [`ksitools-sarif-viewer.html`](ksitools-sarif-viewer.html) | SARIF + Trivy/Grype-style findings tables |
| **SBOM** | [`ksitools-sbom-viewer.html`](ksitools-sbom-viewer.html) | CycloneDX / SPDX component inventory |
| **CloudTrail** | [`ksitools-cloudtrail-viewer.html`](ksitools-cloudtrail-viewer.html) | AWS CloudTrail JSON/JSONL event browser |
| **HAR** | [`ksitools-har-viewer.html`](ksitools-har-viewer.html) | DevTools HTTP Archive: filter, waterfall, headers/bodies/timings |
| **Hash & Checksum** | [`ksitools-hash-viewer.html`](ksitools-hash-viewer.html) | MD5, SHA-1/256/384/512, CRC32; verify against expected hash |
| **EVTX** | [`ksitools-evtx-viewer.html`](ksitools-evtx-viewer.html) | Windows `.evtx` event log table + per-event XML |
| **PCAP / PCAPNG** | [`ksitools-pcap-viewer.html`](ksitools-pcap-viewer.html) | Packet table with L2–L4 header dissection + hex dump |

### Utilities

| Viewer | File | What it does |
|--------|------|----------------|
| **URL Encode / Decode** | [`ksitools-url-viewer.html`](ksitools-url-viewer.html) | Percent-encode/decode; inspect URL components & query params |
| **Hex encode / decode** | [`ksitools-hexcodec-viewer.html`](ksitools-hexcodec-viewer.html) | hex2bin / bin2hex codec with optional URL-decode chain |
| **Regex Tester** | [`ksitools-regex-viewer.html`](ksitools-regex-viewer.html) | Live JS regex with captures, highlight, and replace |
| **Crontab** | [`ksitools-crontab-viewer.html`](ksitools-crontab-viewer.html) | Plain-English schedule explanation + next run times |
| **Timestamp & Date** | [`ksitools-timestamp-viewer.html`](ksitools-timestamp-viewer.html) | Epoch, Julian, DB2, ISO 8601 conversion; world clocks |
| **JSON Query (jq)** | [`ksitools-json-query-viewer.html`](ksitools-json-query-viewer.html) | jq-subset filter/transform for JSON and NDJSON |

### Data & documents

| Viewer | File | What it does |
|--------|------|----------------|
| **JSON** | [`ksitools-json-viewer.html`](ksitools-json-viewer.html) | Collapsible tree, search, pretty-print, export to JSON / CSV / HTML |
| **YAML** | [`ksitools-yaml-viewer.html`](ksitools-yaml-viewer.html) | Safe-load YAML tree, search, export to YAML / JSON / HTML |
| **XML** | [`ksitools-xml-viewer.html`](ksitools-xml-viewer.html) | Native DOM tree with tags, attributes, CDATA; pretty-print export |
| **CSV / TSV** | [`ksitools-csv-viewer.html`](ksitools-csv-viewer.html) | Auto delimiter detection, sort, filter, sticky headers, export |
| **TOML** | [`ksitools-toml-viewer.html`](ksitools-toml-viewer.html) | Collapsible tree for Cargo/pyproject/app configs; export JSON |
| **NDJSON** | [`ksitools-ndjson-viewer.html`](ksitools-ndjson-viewer.html) | JSONL/NDJSON streams as a table; filter, copy array, re-export |
| **Config** | [`ksitools-config-viewer.html`](ksitools-config-viewer.html) | `.env`, `.ini`, `.properties`, `.conf` — sections, secret masking |
| **Log** | [`ksitools-log-viewer.html`](ksitools-log-viewer.html) | Level highlighting (ERROR/WARN/INFO/DEBUG), filter, search, wrap |
| **Diff** | [`ksitools-diff-viewer.html`](ksitools-diff-viewer.html) | Side-by-side drop zones, LCS line diff, word-level highlights |
| **Hex** | [`ksitools-hex-viewer.html`](ksitools-hex-viewer.html) | Classic hex + ASCII dump, magic-byte type hints |
| **Markdown** | [`ksitools-markdown-viewer.html`](ksitools-markdown-viewer.html) | GFM render, save as PDF / Word / HTML (sanitized) |
| **PDF → Markdown** | [`ksitools-pdf-viewer.html`](ksitools-pdf-viewer.html) | Extract PDF text to Markdown; copy / save `.md` / `.txt` |
| **SQLite** | [`ksitools-sqlite-viewer.html`](ksitools-sqlite-viewer.html) | In-browser sql.js, lazy open for multi‑GB DBs, read-only SQL |
| **Parquet** | [`ksitools-parquet-viewer.html`](ksitools-parquet-viewer.html) | Apache Parquet schema + paginated rows (pure-JS subset) |
| **mbox / EML** | [`ksitools-mbox-viewer.html`](ksitools-mbox-viewer.html) | RFC 5322 / mbox mailboxes with MIME parts and attachments |
| **EBCDIC / Fixed-width** | [`ksitools-ebcdic-viewer.html`](ksitools-ebcdic-viewer.html) | EBCDIC codepages + copybook fixed-width layouts |

Detailed feature notes: [docs/viewers.md](docs/viewers.md)

## Quick start

### Option A — open from disk (recommended for sensitive files)

1. Clone or download this repository.
2. Double-click any `ksitools-*-viewer.html` file (or open it in your browser).
3. Drop a file onto the page, or use **Open file**.

No server required. Works offline after the first open.

### Option B — local static server

Handy when a browser restricts `file://` for certain APIs:

```bash
# Python 3
python3 -m http.server 8080 --directory .

# then open http://localhost:8080/
```

## Privacy model

| Guarantee | Detail |
|-----------|--------|
| **No upload** | Files are read with the browser File API only |
| **No network required** | Libraries are embedded; viewers work offline |
| **No backend** | Pure static HTML/CSS/JS |
| **No analytics** | No trackers or third-party beacons in the viewers |
| **SQLite is read-only** | Only `SELECT` / `PRAGMA` / `WITH` / `EXPLAIN` are accepted |

Full statement: [docs/privacy.md](docs/privacy.md)

> **Note:** “Nothing is uploaded” is a property of *these pages*. If you host them on a site that injects other scripts, that hosting layer is outside this project’s control. Prefer opening the HTML files from a trusted local copy when handling secrets.

## Design principles

1. **One file, one job** — easy to share, audit, and version.
2. **Local by default** — sensitive data never leaves the machine unless *you* export or copy.
3. **Honest limits** — large files are accepted up to documented caps; DOM rendering is capped so the UI stays usable (full data remains available for search/export where noted).
4. **Ops-friendly UX** — sticky toolbars, dark-mode via `prefers-color-scheme`, keyboard-friendly controls, export formats that paste into Excel or tickets.

Architecture notes: [docs/architecture.md](docs/architecture.md)

## File size limits (approximate)

| Viewer | Max input | Render cap (UI) |
|--------|-----------|-----------------|
| JSON, YAML, XML, CSV, Log | 100 MB | 50k–100k rows/nodes |
| Diff | 50 MB per side | 60k diff rows |
| Hex | 50 MB | virtualized-style window |
| Config | 25 MB | full (configs are small) |
| PDF → Markdown | 100 MB; ≤2000 pages | full text model (Markdown editable) |
| SQLite | ~32 GB file (lazy >64 MB) | 5k result rows drawn; paginated browse |

Exact constants live in each file as `MAX_BYTES` / `MAX_RENDER` / `MAX_NODES` (SQLite uses `MEMORY_MAX_BYTES` / `ABSURD_MAX_BYTES` for the memory vs lazy split).

## Requirements

- A modern browser (Chrome, Firefox, Edge, Safari — recent versions).
- JavaScript enabled.
- No build step, Node, or package manager.

## Third-party libraries (embedded)

| Library | Used in | License |
|---------|---------|---------|
| [marked](https://github.com/markedjs/marked) v12 | Markdown | MIT |
| [DOMPurify](https://github.com/cure53/DOMPurify) v3.1.6 | Markdown | Apache-2.0 / MPL-2.0 |
| [js-yaml](https://github.com/nodeca/js-yaml) v4.1.0 | YAML | MIT |
| [sql.js](https://github.com/sql-js/sql.js) (SQLite WASM) | SQLite | MIT |
| [PDF.js](https://github.com/mozilla/pdf.js) (pdfjs-dist) 3.11.174 | PDF → Markdown | Apache-2.0 |

Attribution and notices: [docs/third-party.md](docs/third-party.md)

## Project layout

```text
.
├── index.html                      # Hub page
├── ksitools-*-viewer.html         # Self-contained viewers
├── docs/
│   ├── architecture.md
│   ├── privacy.md
│   ├── third-party.md
│   └── viewers.md
├── LICENSE
├── SECURITY.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
└── CHANGELOG.md
```

## Contributing

Bug reports, small UX fixes, and new format viewers are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Security

Please report vulnerabilities privately as described in [SECURITY.md](SECURITY.md). Do not open public issues for unfixed security bugs.

## License

This project is released under the [MIT License](LICENSE).  
Bundled third-party code retains its own licenses (see [docs/third-party.md](docs/third-party.md)).

## Credits

Part of the **KSI Tools** suite — day-to-day ops, support, and integration utilities where opening a file quickly and privately matters more than installing another desktop app.

---

**Tip:** Bookmark the hub (`index.html`) or pin the three viewers you use most. For regulated data, open the HTML from a local path you control, not from an untrusted mirror.
