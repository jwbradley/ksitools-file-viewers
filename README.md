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

## Viewers

| Viewer | File | What it does |
|--------|------|----------------|
| **JSON** | [`ksitools-json-viewer.html`](ksitools-json-viewer.html) | Collapsible tree, search, pretty-print, export to JSON / CSV / HTML |
| **YAML** | [`ksitools-yaml-viewer.html`](ksitools-yaml-viewer.html) | Safe-load YAML tree, search, export to YAML / JSON / HTML |
| **XML** | [`ksitools-xml-viewer.html`](ksitools-xml-viewer.html) | Native DOM tree with tags, attributes, CDATA; pretty-print export |
| **CSV / TSV** | [`ksitools-csv-viewer.html`](ksitools-csv-viewer.html) | Auto delimiter detection, sort, filter, sticky headers, export |
| **Config** | [`ksitools-config-viewer.html`](ksitools-config-viewer.html) | `.env`, `.ini`, `.properties`, `.conf` — sections, secret masking, duplicate-key warnings |
| **Log** | [`ksitools-log-viewer.html`](ksitools-log-viewer.html) | Level highlighting (ERROR/WARN/INFO/DEBUG), filter, search, wrap |
| **Diff** | [`ksitools-diff-viewer.html`](ksitools-diff-viewer.html) | Side-by-side drop zones, LCS line diff, word-level highlights, unified `.diff` export |
| **Hex** | [`ksitools-hex-viewer.html`](ksitools-hex-viewer.html) | Classic hex + ASCII dump, magic-byte type hints, any file type |
| **Markdown** | [`ksitools-markdown-viewer.html`](ksitools-markdown-viewer.html) | GFM render, save as PDF / Word / HTML (sanitized) |
| **SQLite** | [`ksitools-sqlite-viewer.html`](ksitools-sqlite-viewer.html) | In-browser sql.js, lazy open for multi‑GB DBs, pagination, read-only SQL |
| **JWT & Certs** | [`ksitools-jwt-cert-viewer.html`](ksitools-jwt-cert-viewer.html) | Decode JWT claims; inspect PEM/X.509 (subject, SANs, validity) — local only, no verify |
| **HAR** | [`ksitools-har-viewer.html`](ksitools-har-viewer.html) | DevTools HTTP Archive: filter, waterfall, headers/bodies/timings, CSV export |
| **Archive** | [`ksitools-archive-viewer.html`](ksitools-archive-viewer.html) | ZIP/JAR/TAR member listing (sizes, method, dates) without extracting |
| **TOML** | [`ksitools-toml-viewer.html`](ksitools-toml-viewer.html) | Collapsible tree for Cargo/pyproject/app configs; export JSON |
| **NDJSON** | [`ksitools-ndjson-viewer.html`](ksitools-ndjson-viewer.html) | JSONL/NDJSON streams as a table; filter, copy array, re-export |
| **Terraform plan** | [`ksitools-terraform-plan-viewer.html`](ksitools-terraform-plan-viewer.html) | `terraform show -json` plan/state: actions filter, before/after, secret mask |
| **HCL** | [`ksitools-hcl-viewer.html`](ksitools-hcl-viewer.html) | `.tf` / `.hcl` block outline (resource/module/variable/…) |
| **kubeconfig** | [`ksitools-kubeconfig-viewer.html`](ksitools-kubeconfig-viewer.html) | Contexts/clusters/users with tokens & key data masked by default |
| **Kubernetes** | [`ksitools-k8s-viewer.html`](ksitools-k8s-viewer.html) | Multi-doc YAML index by kind/name/namespace |
| **IAM policy** | [`ksitools-iam-policy-viewer.html`](ksitools-iam-policy-viewer.html) | Statement review + broad wildcard heuristics |
| **Base64 / Secrets** | [`ksitools-base64-viewer.html`](ksitools-base64-viewer.html) | K8s Secret `data` decode + raw base64 encode/decode |
| **CIDR** | [`ksitools-cidr-calculator.html`](ksitools-cidr-calculator.html) | Subnet calculator, contains, overlap, split, bulk lists |
| **CloudTrail** | [`ksitools-cloudtrail-viewer.html`](ksitools-cloudtrail-viewer.html) | CloudTrail JSON/JSONL event browser |
| **Dockerfile** | [`ksitools-dockerfile-viewer.html`](ksitools-dockerfile-viewer.html) | Stages, ports, secret-like ENV warnings |
| **SSH keys** | [`ksitools-ssh-key-viewer.html`](ksitools-ssh-key-viewer.html) | Public key / authorized_keys SHA256 fingerprints |

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

### Option C — GitHub Pages

If Pages is enabled on this repository, open the published site URL and pick a viewer from the hub page (`index.html`).

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

Attribution and notices: [docs/third-party.md](docs/third-party.md)

## Project layout

```text
.
├── index.html                      # Hub page (GitHub Pages friendly)
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
