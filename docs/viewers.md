# Viewer reference

Quick reference for each tool. Open the linked HTML file and use **Open file** or drag-and-drop.

---

## JSON — `ksitools-json-viewer.html`

| | |
|--|--|
| **Accepts** | `.json`, `.jsonc`, `.txt` |
| **Limits** | 100 MB; tree capped at 100k nodes |
| **Features** | Collapsible tree, expand/collapse all, search keys & values |
| **Exports** | Copy pretty JSON · Save `.json` · Save `.csv` (arrays of objects; nested keys dotted) · Save `.html` view |

---

## YAML — `ksitools-yaml-viewer.html`

| | |
|--|--|
| **Accepts** | `.yaml`, `.yml`, `.txt` |
| **Limits** | 100 MB; 100k nodes |
| **Features** | Local parse via embedded js-yaml, collapsible tree, search |
| **Exports** | Copy · Save `.yaml` · Save `.json` · Save `.html` |

---

## XML — `ksitools-xml-viewer.html`

| | |
|--|--|
| **Accepts** | `.xml`, `.xsd`, `.xsl`, `.svg`, `.rss`, `.config`, … |
| **Limits** | 100 MB; 100k nodes |
| **Features** | Native `DOMParser`, collapsible tree, colored tags/attrs/text/CDATA/comments |
| **Exports** | Copy pretty XML · Save `.xml` · Save `.html` |

---

## CSV / TSV — `ksitools-csv-viewer.html`

| | |
|--|--|
| **Accepts** | `.csv`, `.tsv`, `.txt` |
| **Limits** | 100 MB; 50k rows drawn |
| **Features** | Auto delimiter (comma / tab / semicolon / pipe), sort columns, filter rows, sticky header & row numbers, header toggle where applicable |
| **Exports** | Copy as TSV (Excel-friendly) · Save `.csv` · Save `.json` · Save `.html` |

---

## Config — `ksitools-config-viewer.html`

| | |
|--|--|
| **Accepts** | `.env`, `.properties`, `.ini`, `.conf`, `.cfg`, `.cnf`, `.toml` (best-effort), `.txt` |
| **Limits** | 25 MB |
| **Features** | Section grouping, secret-like key masking (password/token/key/…), duplicate-key warnings, filter |
| **Exports** | Copy normalized `key=value` · Save `.json` · Save `.html` · Toggle mask |

---

## Log — `ksitools-log-viewer.html`

| | |
|--|--|
| **Accepts** | `.log`, `.txt`, `.out`, `.err` |
| **Limits** | 100 MB; 50k rows drawn |
| **Features** | Level detection (ERROR/WARN/INFO/DEBUG and common synonyms), timestamp tint, level filter, text search, wrap toggle, line numbers |
| **Exports** | Copy visible lines · Save `.log` · Save `.html` |

---

## Diff — `ksitools-diff-viewer.html`

| | |
|--|--|
| **Accepts** | Any two text files (50 MB each) |
| **Limits** | 50 MB/side; 60k diff rows drawn |
| **Features** | Dual drop zones, LCS line diff, word-level highlights on changes, swap sides, “changes only” mode |
| **Exports** | Copy unified diff · Save `.diff` |

---

## Hex — `ksitools-hex-viewer.html`

| | |
|--|--|
| **Accepts** | Any file type |
| **Limits** | 50 MB |
| **Features** | Offset / hex / ASCII columns, magic-byte file-type hints |
| **Exports** | Copy shown dump · Save `.txt` |

---

## Markdown — `ksitools-markdown-viewer.html`

| | |
|--|--|
| **Accepts** | `.md`, `.markdown`, `.txt` |
| **Features** | GitHub-flavored Markdown (marked), sanitized HTML inject (DOMPurify), print-friendly CSS |
| **Exports** | Save as PDF (browser print) · Save as Word-compatible `.doc` · Save `.html` |

---

## PDF → Markdown — `ksitools-pdf-viewer.html`

| | |
|--|--|
| **Accepts** | `.pdf` |
| **Limits** | 100 MB; ≤2000 pages |
| **Features** | Local text extraction via embedded PDF.js; line reconstruction; optional heading heuristics from font size; optional page markers; YAML front matter from PDF metadata; side-by-side Markdown + safe subset preview; password prompt for encrypted PDFs |
| **Not done** | OCR for scanned/image-only PDFs; perfect table/column layout; form field export; digital signature verification |
| **Exports** | Copy Markdown · Save `.md` · Save `.txt` (light strip of markers) |
| **Library** | Embedded **pdfjs-dist** 3.11.174 (main + worker blob) |

> This HTML file is large (~1.4 MB) because PDF.js and its worker are embedded for offline use.

---

## SQLite — `ksitools-sqlite-viewer.html`

| | |
|--|--|
| **Accepts** | `.db`, `.sqlite`, `.sqlite3`, `.db3` |
| **Limits** | Up to ~32 GB file size; **≤64 MB** loads fully into memory, **larger files open lazily** (page-range reads only). Result UI caps at 5k rows. |
| **Features** | Table/view sidebar, SQL box, **Ctrl+Enter** to run, read-only guard, **pagination** (Prev/Next + rows/page), large cell truncation, optional **Count rows** |
| **Allowed SQL** | `SELECT`, `PRAGMA`, `WITH`, `EXPLAIN` (first keyword) |
| **Exports** | Save current result page as `.csv` / `.json` |
| **Large DBs** | Opens in a Web Worker with File-backed I/O — the full file is never read into a single ArrayBuffer. Default browse uses `LIMIT`/`OFFSET` and fetches one extra row to detect “more”. |
| **WAL** | If the header says WAL mode, a notice warns that an adjacent `-wal` is not applied unless you checkpoint first. |

> This HTML file is large (~1 MB) because sql.js + WASM are embedded for offline use.

---

## JWT & Certs — `ksitools-jwt-cert-viewer.html`

| | |
|--|--|
| **Accepts** | `.jwt`, `.pem`, `.crt`, `.cer`, `.key`, `.csr`, paste of raw JWT or PEM |
| **Limits** | 10 MB |
| **Features** | JWT header/payload decode (JWS); claim times (`exp`/`iat`/`nbf`) humanized with expired badges; PEM X.509 fields (subject, issuer, serial, validity, SANs, algs); multi-block PEM bundles; private-key type notice |
| **Not done** | Signature verification, chain trust, JWE decryption, OCSP/CRL |
| **Exports** | Copy JSON · Save `.json` · Save `.html` snapshot |

> Decode-only by design — avoids sending tokens/certs to public decoder sites.

---

## HAR — `ksitools-har-viewer.html`

| | |
|--|--|
| **Accepts** | `.har`, `.json` (HAR shape) |
| **Limits** | 200 MB; 5k rows drawn |
| **Features** | Request table with method/status/URL/type/size/time waterfall; filter by text, method, status class; sort columns; detail pane (headers, query, bodies, timings) |
| **Exports** | Copy summary JSON · Save `.json` · Save `.csv` |

---

## Archive — `ksitools-archive-viewer.html`

| | |
|--|--|
| **Accepts** | `.zip`, `.jar`, `.war`, `.ear`, `.nupkg`, `.tar` |
| **Limits** | 512 MB in-memory listing |
| **Features** | Member path listing, sizes, method, dates; path filter; dir badges; **gzip `.tar.gz`/`.tgz` via `DecompressionStream`**; **ZIP64 EOCD**; click row to preview small text members (store/deflate, ≤256 KB) |
| **Not done** | Full recursive extract-to-disk; bzip2/xz/zstd member inflate; encrypted ZIP |
| **Exports** | Copy list · Save `.csv` · Save `.json` |

---

## TOML — `ksitools-toml-viewer.html`

| | |
|--|--|
| **Accepts** | `.toml`, `.tml` |
| **Limits** | 25 MB; tree capped at 100k nodes |
| **Features** | Practical TOML 1.0 parse (tables, array-tables, inline tables, arrays, strings, numbers, bools, datetimes-as-strings); collapsible tree; search |
| **Exports** | Copy JSON · Save `.json` · Save `.html` |

> Config viewer still accepts `.toml` with a best-effort line parser; use this viewer for real TOML structure.

---

## NDJSON / JSONL — `ksitools-ndjson-viewer.html`

| | |
|--|--|
| **Accepts** | `.ndjson`, `.jsonl`, `.json` (array auto-expanded), `.txt`, `.log` |
| **Limits** | 150 MB; 20k rows drawn; up to 24 flattened columns |
| **Features** | One JSON value per line as a table; nested keys flattened with dots; parse-error rows highlighted; filter + error-only mode; click row for pretty JSON |
| **Exports** | Copy JSON array · Save `.json` · Save `.ndjson` |


---

## Terraform plan / state — `ksitools-terraform-plan-viewer.html`

| | |
|--|--|
| **Accepts** | `terraform show -json` plan output; state JSON |
| **Limits** | 200 MB; 8k rows drawn |
| **Features** | Action chips (create/update/delete/replace), filters, before/after detail, secret masking |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## HCL — `ksitools-hcl-viewer.html`

| | |
|--|--|
| **Accepts** | `.tf`, `.hcl`, `.tfvars` |
| **Limits** | 25 MB |
| **Features** | Brace-matched block outline (resource/data/module/variable/output/provider/…) |
| **Exports** | Copy outline · Save `.json` |

---

## kubeconfig — `ksitools-kubeconfig-viewer.html`

| | |
|--|--|
| **Accepts** | kubeconfig YAML |
| **Features** | Contexts/clusters/users; **mask tokens & key data by default**; exec auth command shown without secrets |
| **Exports** | Copy/Save redacted summary JSON |
| **Library** | Embedded js-yaml |

---

## Kubernetes YAML — `ksitools-k8s-viewer.html`

| | |
|--|--|
| **Accepts** | Multi-doc `.yaml` / `.yml`, List types, JSON |
| **Limits** | 50 MB |
| **Features** | Index by kind/name/namespace/apiVersion; basic required-field issues; full object dump on click |
| **Exports** | Copy/Save index JSON |
| **Library** | Embedded js-yaml |

---

## IAM policy — `ksitools-iam-policy-viewer.html`

| | |
|--|--|
| **Accepts** | IAM policy JSON (or single Statement) |
| **Features** | Per-statement Effect/Action/Resource/Principal/Condition; flags for `*` wildcards and broad Allow |
| **Not done** | Live IAM simulator / account evaluation |
| **Exports** | Copy/Save summary JSON |

---

## Base64 / Secrets — `ksitools-base64-viewer.html`

| | |
|--|--|
| **Accepts** | Raw base64, K8s Secret/ConfigMap YAML `data` maps |
| **Features** | Decode/encode modes; Secret `data` / `stringData` key listing |
| **Exports** | Copy · Save `.txt` |

---

## CIDR calculator — `ksitools-cidr-calculator.html`

| | |
|--|--|
| **Accepts** | Form input or text list of CIDRs/IPs |
| **Features** | **IPv4 + IPv6** network/hosts, contains, overlap, split, bulk overlap scan |
| **Exports** | Copy JSON · Save `.csv` |

---

## CloudTrail — `ksitools-cloudtrail-viewer.html`

| | |
|--|--|
| **Accepts** | `{Records:[…]}`, JSON array, NDJSON |
| **Limits** | 200 MB; 8k rows drawn |
| **Features** | Time/event/user/IP/region/error table; full event detail |
| **Exports** | Copy · Save `.json` · Save `.csv` |

---

## Dockerfile — `ksitools-dockerfile-viewer.html`

| | |
|--|--|
| **Accepts** | `Dockerfile`, `.dockerfile` |
| **Features** | Multi-stage outline, EXPOSE summary, warnings (latest, USER root, secret-like ENV, pipe-to-shell) |
| **Exports** | Copy outline · Save `.json` |

---

## SSH keys — `ksitools-ssh-key-viewer.html`

| | |
|--|--|
| **Accepts** | `.pub`, `authorized_keys`, `known_hosts`, paste |
| **Features** | SHA256 fingerprints (Web Crypto), type/comment; private PEM noted not fingerprinted |
| **Exports** | Copy fingerprints · Save `.json` |


---

## Excel inventory — `ksitools-excel-viewer.html`

| | |
|--|--|
| **Accepts** | `.xlsx` (OOXML only; not legacy `.xls`) |
| **Limits** | ~80 MB; 20k rows drawn per sheet |
| **Features** | Sheet tabs, first-row headers, filter, copy TSV, export CSV/JSON |
| **How** | ZIP parse + sharedStrings/sheet XML via browser `DecompressionStream` |
| **Not done** | `.xls` BIFF, charts/macros, formula evaluation |

---

## Secret / PII redactor — `ksitools-redactor.html`

| | |
|--|--|
| **Accepts** | Any text (logs, configs, dumps); paste or drop |
| **Limits** | 50 MB |
| **Features** | AWS keys, PEM private keys, JWT, Bearer, GitHub/Slack/GCP tokens, connection strings, secret assignments; optional email/IP/phone/long-base64 |
| **Exports** | Copy redacted · Save `.txt` |
| **Not done** | ML NER; perfect precision — always review before sharing |

---

## Cloud audit log — `ksitools-cloud-audit-viewer.html`

| | |
|--|--|
| **Accepts** | CloudTrail `{Records}`, Azure Activity arrays, GCP Cloud Logging audit JSON/JSONL |
| **Limits** | 200 MB; 8k rows drawn |
| **Features** | Auto cloud detect, unified time/action/actor/resource/status table, error filter |
| **Exports** | Copy · Save `.json` · Save `.csv` |

---

## Network rules — `ksitools-network-rules-viewer.html`

| | |
|--|--|
| **Accepts** | AWS `describe-security-groups` / `describe-network-acls`, Azure NSG ARM JSON |
| **Features** | Flattened rule table; flags for `0.0.0.0/0`, SSH/RDP/SMB |
| **Exports** | Copy · Save `.json` · Save `.csv` |
| **Not done** | Live cloud evaluation; GCP firewall JSON (use generic JSON viewer) |

---

## SQL dump — `ksitools-sql-dump-viewer.html`

| | |
|--|--|
| **Accepts** | `.sql`, `.ddl` dumps |
| **Limits** | 150 MB; 15k statements drawn |
| **Features** | Heuristic statement split; kind/object outline; click for full SQL |
| **Not done** | Execute SQL; perfect dialect parsing |

---

## Docker Compose — `ksitools-compose-viewer.html`

| | |
|--|--|
| **Accepts** | `docker-compose.yml` / Compose spec YAML or JSON |
| **Features** | Services table (image/build, ports, depends_on), secret-like env, privileged/host-network/docker.sock notes |
| **Library** | Embedded js-yaml |

---

## CloudFormation — `ksitools-cloudformation-viewer.html`

| | |
|--|--|
| **Accepts** | CFN template JSON/YAML; change-set JSON with `Changes[]` |
| **Features** | Resources by logical ID/type; parameters/outputs counts; change-set actions |
| **Library** | Embedded js-yaml for YAML templates |

---

## ARM / Bicep — `ksitools-arm-bicep-viewer.html`

| | |
|--|--|
| **Accepts** | ARM `.json`, `.bicep` |
| **Features** | Resource walk (nested); Bicep `resource`/`module`/`param` outline |
| **Not done** | Bicep type-checking or what-if |

---

## Helm — `ksitools-helm-viewer.html`

| | |
|--|--|
| **Accepts** | Chart.yaml, values.yaml, multi-doc YAML |
| **Features** | Chart metadata card; flattened values paths; secret-like key flags |
| **Not done** | Template rendering (`{{ }}`) |
| **Library** | Embedded js-yaml |

---

## OpenAPI — `ksitools-openapi-viewer.html`

| | |
|--|--|
| **Accepts** | OpenAPI 3.x / Swagger 2.0 JSON or YAML |
| **Features** | Method/path table, operationId, tags, security, schema counts |
| **Library** | Embedded js-yaml for YAML specs |

---

## SARIF / findings — `ksitools-sarif-viewer.html`

| | |
|--|--|
| **Accepts** | `.sarif`, SARIF JSON; Trivy/Grype-style JSON |
| **Features** | Severity filter, rule/message/location table |
| **Not done** | Live re-scan |

---

## SBOM — `ksitools-sbom-viewer.html`

| | |
|--|--|
| **Accepts** | CycloneDX JSON, SPDX JSON, SPDX tag-value |
| **Features** | Component name/version/license/PURL table |
| **Not done** | License legal engine; CycloneDX XML |

---

## SAML — `ksitools-saml-viewer.html`

| | |
|--|--|
| **Accepts** | SAML metadata XML, Assertion/Response XML, base64 SAMLResponse paste |
| **Features** | entityID, ACS/SLO endpoints, NameID, attributes, cert previews |
| **Not done** | Signature verification, deflate-redirect inflate edge cases |

---

## Base64 / Secrets (upgraded) — `ksitools-base64-viewer.html`

| | |
|--|--|
| **Accepts** | Base64 text, `.b64`, K8s Secret/ConfigMap YAML/JSON, any file (encode mode) |
| **Limits** | ~25 MB |
| **Features** | Live two-pane encode/decode; standard + URL-safe (base64url); MIME wrap 76 cols; strip whitespace; per-line decode; swap; auto-detect K8s `data` maps; binary shown as hex with Save `.bin` |
| **Exports** | Copy output · Save text / binary |

---

## Crontab — `ksitools-crontab-viewer.html`

| | |
|--|--|
| **Accepts** | `.cron`, `.crontab`, crontab text (paste supported) |
| **Limits** | 10 MB |
| **Features** | Plain-English schedule explanation, next run times, `@daily`/`@hourly` macros, filter |
| **Exports** | Copy / save explained table |

---

## EBCDIC / Fixed-width — `ksitools-ebcdic-viewer.html`

| | |
|--|--|
| **Accepts** | EBCDIC or fixed-width binary/text; copybook-style layout defs |
| **Limits** | 50 MB; 50k rows rendered |
| **Features** | Codepages CP037/1047/500/…; record length; layout columns; packed/zoned decimal best-effort; filter |
| **Exports** | Copy / CSV of decoded rows |

---

## EVTX — `ksitools-evtx-viewer.html`

| | |
|--|--|
| **Accepts** | Windows `.evtx` |
| **Limits** | 100 MB; 20k rows rendered |
| **Features** | Pure-JS binary-XML parser; EventID / Level / Channel / Provider table; per-event XML detail; search + EventID filter |
| **Not done** | Full Wevtutil parity; remote live logs |

---

## Hash & Checksum — `ksitools-hash-viewer.html`

| | |
|--|--|
| **Accepts** | Any file or pasted text |
| **Limits** | ~200 MB |
| **Features** | MD5, SHA-1/256/384/512 (Web Crypto), CRC32; verify against expected hash; streaming progress for large files |
| **Exports** | Copy digests |

---

## Hex encode / decode — `ksitools-hexcodec-viewer.html`

| | |
|--|--|
| **Accepts** | Hex text or any file (encode) |
| **Features** | hex2bin / bin2hex; strip `0x` / spaces / `\x`; optional URL-decode chain; swap; handoff to URL tool |
| **Note** | Distinct from the Hex **dump** viewer (`ksitools-hex-viewer.html`) |

---

## JSON Query (jq-style) — `ksitools-json-query-viewer.html`

| | |
|--|--|
| **Accepts** | `.json`, `.ndjson`, `.jsonl` |
| **Features** | Pure-JS jq subset: `.foo`, `.[]`, `select`, `map`, `group_by`, pipes, `limit`, `if/then/else`, path helpers |
| **Exports** | Copy / save query result as JSON |

---

## mbox / EML — `ksitools-mbox-viewer.html`

| | |
|--|--|
| **Accepts** | `.mbox`, `.eml` |
| **Limits** | 50 MB |
| **Features** | RFC 5322 parse; MIME parts & attachments; encoded-word headers; message list + detail |
| **Exports** | Copy body / headers; save attachment bytes locally |

---

## Parquet — `ksitools-parquet-viewer.html`

| | |
|--|--|
| **Accepts** | `.parquet` |
| **Limits** | 256 MB; 50k rows rendered |
| **Features** | Pure-JS subset reader; schema tree; paginated rows; UNCOMPRESSED/GZIP/SNAPPY; PLAIN / dictionary / delta encodings |
| **Not done** | Full Parquet feature matrix (e.g. some nested / exotic encodings) |

---

## PCAP / PCAPNG — `ksitools-pcap-viewer.html`

| | |
|--|--|
| **Accepts** | `.pcap`, `.pcapng` |
| **Limits** | 50 MB; 10k packets rendered; 64 KB hex dump per packet |
| **Features** | Packet table; Ethernet / IPv4 / IPv6 / TCP / UDP / ICMP header dissection; filter; per-packet hex |
| **Not done** | Full protocol dissectors beyond common L2–L4 |

---

## PKCS#12 / PFX — `ksitools-pkcs12-viewer.html`

| | |
|--|--|
| **Accepts** | `.p12`, `.pfx` |
| **Limits** | 25 MB |
| **Features** | Bundle inventory; PBES2/AES and legacy 3DES decrypt; MAC verify; cert subject/issuer/validity; **private key bytes never displayed** |
| **Not done** | Key export / re-packaging |

---

## Regex Tester — `ksitools-regex-viewer.html`

| | |
|--|--|
| **Accepts** | Pattern + haystack (paste) |
| **Limits** | 5k matches; 200k chars highlighted |
| **Features** | Live JS-flavor regex; capture groups; inline highlight; replace mode |
| **Exports** | Copy matches / replacement result |

---

## Timestamp & Date — `ksitools-timestamp-viewer.html`

| | |
|--|--|
| **Accepts** | Epoch, AWD Julian (`YYYYDDD`/`YYDDD`), DB2, ISO 8601 (paste) |
| **Features** | Bidirectional conversion; live world time zones; “now” hero clock |
| **Exports** | Copy converted values |

---

## URL Encode / Decode — `ksitools-url-viewer.html`

| | |
|--|--|
| **Accepts** | URLs / query strings / text files of URLs |
| **Features** | Percent-encode/decode; component inspect (scheme/host/path/query/hash); query param table; sessionStorage handoff from hex codec |
| **Exports** | Copy encoded/decoded text |

---

## systemd unit / timer — `ksitools-systemd-viewer.html`

| | |
|--|--|
| **Accepts** | `.service`, `.timer`, `.socket`, `.mount`, `.path`, `.slice`, `.target`, `.conf`, paste |
| **Limits** | 5 MB |
| **Features** | INI section parse with `\` continuations; summary (Description, Type, User, ExecStart, WantedBy, timer calendars); flags for root/missing User, secret-like Environment=, PrivateTmp/ProtectSystem/NoNewPrivileges hygiene, Restart=no, curl/wget `http://` in Exec* |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Proxy / LB config — `ksitools-proxy-viewer.html`

| | |
|--|--|
| **Accepts** | `.conf`, `.nginx`, `.cfg`, `.caddy`, `.haproxy`, `.txt`, paste |
| **Limits** | 10 MB; 5k blocks rendered |
| **Features** | Auto-detect nginx / Apache / HAProxy / Caddy (manual override); server/location/upstream or frontend/backend outline; flags for open `:80`/`0.0.0.0` listen, cleartext upstreams, missing `server_name`, basic auth |
| **Exports** | Copy summary · Save `.csv` · Save `.json` |

---

## Firewall rules — `ksitools-firewall-viewer.html`

| | |
|--|--|
| **Accepts** | iptables-save, nftables, UFW status, firewalld zone XML; `.txt` `.rules` `.nft` `.xml` `.ufw` |
| **Limits** | 25 MB; 15k rows rendered |
| **Features** | Normalize to Table/Chain/Action/Proto/Ports/Source/Dest; world-open + SSH/RDP/telnet/SMB risk badges; Risks-only filter; pairs with cloud Network rules viewer |
| **Exports** | Copy summary · Save `.csv` · Save `.json` |

---

## DNS / CoreDNS — `ksitools-dns-viewer.html`

| | |
|--|--|
| **Accepts** | `.zone`, `.db`, Corefile / `.coredns`, `.txt`, paste |
| **Limits** | 15 MB; 20k records rendered |
| **Features** | BIND-style `$ORIGIN`/`$TTL`/SOA/RR table (multi-line parentheses); wildcards, dangling CNAME, long TTL, missing trailing-dot flags; CoreDNS plugin blocks with public-forward highlights |
| **Exports** | Copy summary · Save `.csv` · Save `.json` · Save `.html` snapshot |

---

## Observability — `ksitools-observability-viewer.html`

| | |
|--|--|
| **Accepts** | `.yml`, `.yaml`, `.json`, `.txt`, paste (Prometheus / Alertmanager / Grafana) |
| **Limits** | 15 MB |
| **Features** | Auto-detect mode (manual override); Prometheus scrape jobs + alert/record rules; Alertmanager route tree + receivers; Grafana dashboard panel inventory (incl. nested rows); secret-like tokens/URLs masked |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## CI pipeline — `ksitools-ci-viewer.html`

| | |
|--|--|
| **Accepts** | `.yml`, `.yaml`, `.txt`, paste (GitHub Actions / GitLab CI / Azure Pipelines) |
| **Limits** | 10 MB |
| **Features** | Dialect auto-detect; jobs + steps flatten; flags for `pull_request_target`, `permissions: write-all`, plaintext env secrets, insecure `curl\|bash` |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## SSH config — `ksitools-ssh-config-viewer.html`

| | |
|--|--|
| **Accepts** | `ssh_config`, `config`, `.conf`, `.txt`, paste |
| **Limits** | 5 MB |
| **Features** | Host/Match block outline; ProxyJump / IdentityFile counts; flags for `StrictHostKeyChecking no`, `UserKnownHostsFile /dev/null`, `ForwardAgent yes` |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## WireGuard — `ksitools-wireguard-viewer.html`

| | |
|--|--|
| **Accepts** | `.conf`, `.wg`, `.txt`, paste |
| **Limits** | 5 MB |
| **Features** | Interface + multi-peer outline; PrivateKey / PresharedKey masked by default (Reveal secrets toggle); full-tunnel `0.0.0.0/0` / `::/0` flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` (redacted unless reveal on) |

---

## Ansible — `ksitools-ansible-viewer.html`

| | |
|--|--|
| **Accepts** | `.ini`, `.yml`, `.yaml`, `.cfg`, `.txt`, inventory / playbook paste |
| **Limits** | 10 MB |
| **Features** | Inventory INI/YAML host/group flatten; playbook plays/roles/tasks/handlers summary; password-like vars masked; `become` / `hosts: all` / insecure shell flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |
