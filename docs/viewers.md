# Viewer reference

Quick reference for each tool. Open the linked HTML file and use **Open file** or drag-and-drop.

The original core set is documented first. Research-pack viewers (plus DMARC, Postfix, and Windows Registry) follow in hub order. Searchable catalog: [`index.html`](../index.html) and [README.md](../README.md).

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

## sudoers — `ksitools-sudoers-viewer.html`

| | |
|--|--|
| **Accepts** | `sudoers`, `.sudoers`, `sudoers.d/*`, `.txt`, paste |
| **Limits** | 5 MB |
| **Features** | Defaults / User|Runas|Host|Cmnd aliases / privilege specs; `#include(dir)`; flags for `NOPASSWD: ALL`, bare `ALL`, `!authenticate` |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Host sysfiles — `ksitools-sysfiles-viewer.html`

| | |
|--|--|
| **Accepts** | `fstab`, `hosts`, `exports`, `resolv.conf`, `.txt`, paste |
| **Limits** | 5 MB |
| **Features** | Auto-detect format; tables per dialect; flags for duplicate hosts, `no_root_squash`, wide NFS `*`, missing `nofail` on network mounts |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## fail2ban — `ksitools-fail2ban-viewer.html`

| | |
|--|--|
| **Accepts** | `jail.conf`, `jail.local`, `jail.d/*`, `filter.d/*`, `.conf`, `.local`, `.txt`, paste |
| **Limits** | 5 MB |
| **Features** | Jail vs filter auto-detect; enabled jails; `bantime=-1` / empty `failregex` flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## SSH known_hosts — `ksitools-known-hosts-viewer.html`

| | |
|--|--|
| **Accepts** | `known_hosts`, `.known_hosts`, `.txt`, paste |
| **Limits** | 10 MB |
| **Features** | Host patterns + hashed `|1|…` entries; SHA256 fingerprints; `@revoked` / `@cert-authority` markers |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

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

## OpenVPN — `ksitools-openvpn-viewer.html`

| | |
|--|--|
| **Accepts** | `.ovpn`, `.conf`, `.txt`, paste |
| **Limits** | 5 MB |
| **Features** | Remotes / cipher / auth outline; inline `<ca>`/`<cert>`/`<key>` masked by default; flags for `cipher none`, `comp-lzo`, `redirect-gateway` |
| **Exports** | Copy summary · Save `.json` · Save `.csv` (redacted unless reveal on) |

---

## Ansible — `ksitools-ansible-viewer.html`

| | |
|--|--|
| **Accepts** | `.ini`, `.yml`, `.yaml`, `.cfg`, `.txt`, inventory / playbook paste |
| **Limits** | 10 MB |
| **Features** | Inventory INI/YAML host/group flatten; playbook plays/roles/tasks/handlers summary; password-like vars masked; `become` / `hosts: all` / insecure shell flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## LDAP LDIF — `ksitools-ldif-viewer.html`

| | |
|--|--|
| **Accepts** | `.ldif`, `.ldi`, `.txt`, paste |
| **Limits** | 25 MB |
| **Features** | RFC 2849 unfold + entry parse (`attr:` / `attr::` / `attr:<`); DN / objectClass inventory; credential attrs masked (Reveal toggle); duplicate-DN, delete changetype, external URL flags |
| **Exports** | Copy summary · Save `.json` (credentials always redacted) · Save `.csv` |

---

## Dependency lockfiles — `ksitools-lockfile-viewer.html`

| | |
|--|--|
| **Accepts** | `package-lock.json`, `npm-shrinkwrap.json`, `go.mod`, `go.sum`, `Cargo.lock`, `poetry.lock`, `requirements.txt`, `Pipfile.lock`, `composer.lock`, `Gemfile.lock`, paste |
| **Limits** | 50 MB; 20k rows rendered |
| **Features** | Auto-detect format; unified Name/Version/Kind table; duplicate-version, git/path source, npm `http://` registry, and go `replace` flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Protobuf — `ksitools-proto-viewer.html`

| | |
|--|--|
| **Accepts** | `.proto`, `.txt`, paste |
| **Limits** | 10 MB |
| **Features** | proto2/proto3 outline: package, imports, messages (nested), enums, services/RPCs (streaming), maps/oneof/reserved; flags for missing syntax, proto2 `required`, duplicate field numbers |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Policy-as-code — `ksitools-policy-viewer.html`

| | |
|--|--|
| **Accepts** | `.yaml`, `.yml`, `.rego`, `.txt`, paste |
| **Limits** | 10 MB |
| **Features** | Auto-detect Kyverno / Gatekeeper / OPA Rego / ValidatingAdmissionPolicy; multi-doc YAML; Enforce vs Audit; validate/mutate/generate/verifyImages; Rego deny/violation/allow |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Java KeyStore — `ksitools-jks-viewer.html`

| | |
|--|--|
| **Accepts** | `.jks`, `.keystore`, `.ks`, `.jceks` (binary) |
| **Limits** | 25 MB |
| **Features** | JKS (best-effort JCEKS) alias inventory; trusted cert + private-key entry metadata; optional password integrity verify; X.509 subject/expiry/SHA-256; **private key bytes never displayed**; PKCS#12 sniff redirects to PKCS#12 viewer |
| **Exports** | Copy summary · Save `.json` · Save `.csv` (certs/fingerprints only) |

---

The sections below cover the **research-pack** viewers (plus the DMARC, Postfix, and Windows Registry shelf items). Same interaction model: **Open file**, drag-and-drop, and paste where noted. Grouped as on the hub.

## File type sniffer — `ksitools-filetype-viewer.html`

| | |
|--|--|
| **Accepts** | Any file type; hex paste |
| **Limits** | Reads at most 64 MB into memory; only the first bytes are used for sniffing |
| **Features** | Magic-byte / hex sniff with confidence; MIME + signature table; links to a matching KSI Tools viewer when one exists |
| **Exports** | None (jump to another viewer) |

> Detection is local-only. Large files are sliced — the whole payload is not parsed.

---

## Maven POM — `ksitools-maven-pom-viewer.html`

| | |
|--|--|
| **Accepts** | `pom.xml`, `.pom`, `.xml`, paste |
| **Limits** | 20 MB |
| **Features** | Dependencies / plugins / modules / parent outline; SNAPSHOT, optional, test-only, and encoding flags |
| **Not done** | Does not resolve a reactor or download from Maven Central |
| **Exports** | Copy summary · Save `.json` |

---

## package.json — `ksitools-package-json-viewer.html`

| | |
|--|--|
| **Accepts** | `package.json`, `.json`, paste |
| **Limits** | 10 MB |
| **Features** | Manifest audit: scripts, deps, engines, license; supply-chain flags (curl/wget in scripts, UNLICENSED, bundleDependencies) |
| **Exports** | Copy summary · Save `.json` |

---

## nginx config — `ksitools-nginx-config-viewer.html`

| | |
|--|--|
| **Accepts** | `nginx.conf`, `.conf`, `.nginx`, paste / included fragments |
| **Limits** | 5 MB |
| **Features** | Server/location/upstream outline; TLS, reverse-proxy, listen flags; HTTP-not-redirected, missing cert/key, empty upstream heuristics |
| **Not done** | Does not follow `include` files from disk |
| **Exports** | Copy summary · Save `.json` |

---

## Apache httpd — `ksitools-apache-config-viewer.html`

| | |
|--|--|
| **Accepts** | `httpd.conf`, `apache2.conf`, vhost `.conf`, `.htaccess`, paste |
| **Limits** | 5 MB |
| **Features** | VirtualHost / Directory / Rewrite outline; TLS and access-control flags (`Require all granted`, `Options +Indexes`, HTTP-only, `.htaccess` detected) |
| **Not done** | Does not follow `Include` / `IncludeOptional` from disk |
| **Exports** | Copy summary · Save `.json` |

---

## HAProxy — `ksitools-haproxy-config-viewer.html`

| | |
|--|--|
| **Accepts** | `haproxy.cfg`, `.cfg`, `.conf`, `.haproxy`, paste |
| **Limits** | 5 MB |
| **Features** | Frontend/backend/listen outline; TLS, ACLs, health-check notes; HTTP→HTTPS redirect, unverified backend TLS, broken `default_backend` flags |
| **Exports** | Copy summary · Save `.json` |

> Paste config only — not cert/key files.

---

## fstab / mounts — `ksitools-fstab-viewer.html`

| | |
|--|--|
| **Accepts** | `/etc/fstab`, `/proc/mounts`, `mtab`, `findmnt` text, paste |
| **Limits** | 5 MB |
| **Features** | Mount table: device, point, type, options, dump/pass; duplicate mountpoint, missing `nofail` on network mounts, bind/nfs flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## sshd_config — `ksitools-sshd-config-viewer.html`

| | |
|--|--|
| **Accepts** | `sshd_config`, `sshd_config.d/*`, `.conf`, paste |
| **Limits** | 5 MB |
| **Features** | Daemon settings table; flags for `PermitRootLogin`, `PasswordAuthentication`, `PermitEmptyPasswords`, Match blocks that relax those, `MaxAuthTries` |
| **Not done** | Does not follow `Include` from disk |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## sysctl — `ksitools-sysctl-viewer.html`

| | |
|--|--|
| **Accepts** | `sysctl.conf`, `sysctl.d/*`, `sysctl -a` dump, paste |
| **Limits** | 10 MB |
| **Features** | Kernel tunable table; duplicate keys; net/vm highlights (`ip_forward`, `accept_redirects`, ASLR off, `file-max`) |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## PAM stack — `ksitools-pam-viewer.html`

| | |
|--|--|
| **Accepts** | `/etc/pam.d/*`, `/etc/pam.conf`, `.pam`, paste |
| **Limits** | 5 MB |
| **Features** | Auth stack outline (type/control/module); `sufficient`/`required`; include/substack noted but not followed; flags for `pam_deny sufficient`, missing sha512 / password strength |
| **Not done** | `@include` / `include` / `substack` are not followed into other files |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## NFS exports — `ksitools-nfs-exports-viewer.html`

| | |
|--|--|
| **Accepts** | `/etc/exports`, `exports.d/*`, paste |
| **Limits** | 5 MB |
| **Features** | Export table (path, clients, options); `no_root_squash`, `insecure`, wildcard-client flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Samba smb.conf — `ksitools-smb-conf-viewer.html`

| | |
|--|--|
| **Accepts** | `smb.conf`, `.conf`, paste |
| **Limits** | 5 MB |
| **Features** | Global/share outline; guest ok, browseable, path inventory; flags for `security = SHARE`, signing disabled, `encrypt passwords = no` |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## logrotate — `ksitools-logrotate-viewer.html`

| | |
|--|--|
| **Accepts** | `logrotate.conf`, `logrotate.d/*`, paste |
| **Limits** | 5 MB |
| **Features** | Rotate stanzas: path, frequency, size, compress, postrotate hooks; `copytruncate`+compress and missing frequency/size flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Package repo sources — `ksitools-repos-viewer.html`

| | |
|--|--|
| **Accepts** | apt `sources.list` / `.sources`, yum/dnf `.repo`, `pacman.conf`, apk `repositories`, paste |
| **Limits** | 5 MB |
| **Features** | Enabled repos, baseurl, GPG; flags for `gpgcheck=0`, `SigLevel Never`, HTTP, duplicate URI, allow-insecure |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## limits.conf — `ksitools-limits-conf-viewer.html`

| | |
|--|--|
| **Accepts** | `/etc/security/limits.conf`, `limits.d/*`, paste |
| **Limits** | 5 MB |
| **Features** | ulimit / pam_limits table (domain, type, item, value); flags for unlimited CPU, core dumps on, group vs `*` overrides |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## audit.rules — `ksitools-audit-rules-viewer.html`

| | |
|--|--|
| **Accepts** | `audit.rules`, `rules.d/*`, `.rules`, paste |
| **Limits** | 5 MB |
| **Features** | auditd watches / syscalls / keys; flags for `-D`, `-a never` suppression, low backlog, failure mode (`-f 0/1/2`) |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Postfix — `ksitools-postfix-viewer.html`

| | |
|--|--|
| **Accepts** | `main.cf`, `master.cf`, `.cf`, paste |
| **Limits** | 5 MB |
| **Features** | Mailer outline; relay, TLS, mynetworks; password-like values masked by default (Reveal secrets toggle) |
| **Exports** | Copy summary · Save `.json` · Save `.csv` (redacted unless reveal on) |

---

## DHCPd / Kea — `ksitools-dhcpd-viewer.html`

| | |
|--|--|
| **Accepts** | ISC `dhcpd.conf`, Kea `dhcp4`/`dhcp6` JSON/conf, paste |
| **Limits** | 5 MB |
| **Features** | Subnets, ranges, options, failover notes; ISC dhcpd and Kea dhcp4/dhcp6 auto-detect |
| **Exports** | Copy summary · Save `.json` |

---

## Fluent Bit / Fluentd — `ksitools-fluent-config-viewer.html`

| | |
|--|--|
| **Accepts** | `fluent-bit.conf`, `td-agent.conf`, `.conf`, `.cfg`, paste |
| **Limits** | 5 MB |
| **Features** | Input/filter/output (or source/filter/match) pipeline outline; `@INCLUDE`, match-shadow, OUTPUT-before-INPUT, missing Name flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Vector — `ksitools-vector-viewer.html`

| | |
|--|--|
| **Accepts** | `vector.yaml`, `vector.toml`, `.yaml`/`.yml`/`.toml`, paste |
| **Limits** | 5 MB |
| **Features** | Sources, transforms, sinks; dangling input / orphan source / insecure TLS flags |
| **Exports** | Copy summary · Save `.json` |
| **Library** | Embedded js-yaml for YAML configs |

---

## Telegraf — `ksitools-telegraf-viewer.html`

| | |
|--|--|
| **Accepts** | `telegraf.conf`, `.toml`, `.conf`, paste |
| **Limits** | 5 MB |
| **Features** | Input/output/processor plugins and `[agent]` intervals; TLS-verify-disabled, insecure HTTP output, omit_hostname flags |
| **Exports** | Copy summary · Save `.json` |

---

## OTel Collector — `ksitools-otel-collector-viewer.html`

| | |
|--|--|
| **Accepts** | `otel-collector.yaml`, `.yaml`/`.yml`/`.conf`, paste |
| **Limits** | 5 MB |
| **Features** | Receivers, processors, exporters, pipelines, extensions; unused/undefined extension, missing batch / memory_limiter flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |
| **Library** | Embedded js-yaml |

---

## Loki streams — `ksitools-loki-viewer.html`

| | |
|--|--|
| **Accepts** | Loki API JSON, `logcli` output, `.json`/`.jsonl`/`.ndjson`/`.log`, paste |
| **Limits** | 50 MB; 20k rows drawn |
| **Features** | Stream/log table; flags for OOM markers, TLS/connectivity issues, heavy streams, stack traces, time gaps |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Jaeger traces — `ksitools-jaeger-viewer.html`

| | |
|--|--|
| **Accepts** | Jaeger JSON export (`data:[]` traces), `.json`/`.jsonl`, paste |
| **Limits** | 50 MB; 10k rows drawn |
| **Features** | Trace/span timeline from a Jaeger export — local only |
| **Not done** | Not a live Jaeger query UI |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Prometheus exposition — `ksitools-promex-viewer.html`

| | |
|--|--|
| **Accepts** | Scraped `/metrics` dump, `.prom`, `.metrics`, paste |
| **Limits** | 50 MB; 50k samples drawn |
| **Features** | Metric families + labels; Families vs Samples views |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Splunk .conf — `ksitools-splunk-conf-viewer.html`

| | |
|--|--|
| **Accepts** | `props.conf`, `transforms.conf`, `inputs.conf`, `outputs.conf`, `indexes.conf`, `server.conf`, `btool` output, paste |
| **Limits** | 10 MB |
| **Features** | Stanza inventory; flags for FORMAT without REGEX, unbalanced parens, SHOULD_LINEMERGE=true, disabled=1, duplicate keys |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## rsyslog — `ksitools-rsyslog-viewer.html`

| | |
|--|--|
| **Accepts** | `rsyslog.conf`, `rsyslog.d/*`, paste |
| **Limits** | 5 MB |
| **Features** | Legacy selectors and RainerScript; templates, modules, forwarding; TLS vs UDP notes |
| **Exports** | Copy summary · Save `.json` |

---

## journald JSON — `ksitools-journald-viewer.html`

| | |
|--|--|
| **Accepts** | `journalctl -o json` / `json-pretty` / JSONL, paste |
| **Limits** | 50 MB; 20k rows drawn |
| **Features** | Filterable event table (unit, priority, message, SYSLOG_IDENTIFIER) |
| **Not done** | Does not parse binary `.journal` files |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## auditd log — `ksitools-auditd-log-viewer.html`

| | |
|--|--|
| **Accepts** | `/var/log/audit/audit.log`, `ausearch` output, paste |
| **Limits** | 100 MB; 20k rows drawn |
| **Features** | Record table: type, syscall, exe, key, success/fail |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## sar / sysstat — `ksitools-sar-viewer.html`

| | |
|--|--|
| **Accepts** | Text `sar -A`, `iostat -x`, `mpstat` dumps (not binary sadc) |
| **Limits** | 20 MB |
| **Features** | CPU/IO/memory tables; saturation, `%iowait`, `%steal`, `%util=100` flags |
| **Not done** | Binary `sadc`/`sa??` is **not** supported — run `sadf -d` or `sar -A` first |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Kyverno — `ksitools-kyverno-viewer.html`

| | |
|--|--|
| **Accepts** | Kyverno `ClusterPolicy` / `Policy` YAML or JSON, multi-doc, paste |
| **Limits** | 10 MB |
| **Features** | validate / mutate / generate / verifyImages inventory; Enforce vs Audit; background-scan notes |
| **Not done** | Does not evaluate policies against live cluster objects |
| **Exports** | Copy summary · Save `.json` |
| **Library** | Embedded js-yaml |

---

## Rego / OPA — `ksitools-rego-viewer.html`

| | |
|--|--|
| **Accepts** | `.rego`, paste |
| **Limits** | 5 MB |
| **Features** | Structural outline (package, imports, rules); deny/allow/default-allow flags; external-data notes |
| **Not done** | Tokenizer / outline only — **not an evaluator** |
| **Exports** | Copy summary · Save `.json` |

---

## JSON Schema — `ksitools-jsonschema-viewer.html`

| | |
|--|--|
| **Accepts** | `.schema.json`, `.json`, YAML schema, paste |
| **Limits** | 40 MB; tree depth 60 |
| **Features** | Draft schema tree: types, required, `$ref`, constraints; dangling-ref / missing notes |
| **Not done** | Does not validate instance documents |
| **Exports** | Copy summary · Save `.json` |
| **Library** | Embedded js-yaml for YAML schemas |

---

## GraphQL schema — `ksitools-graphql-viewer.html`

| | |
|--|--|
| **Accepts** | `.graphql`, `.graphqls`, `.gql`, paste |
| **Limits** | 10 MB |
| **Features** | SDL outline of types, queries, mutations, subscriptions, directives, interfaces |
| **Not done** | Structural outline only — **no execution engine** |
| **Exports** | Copy summary · Save `.json` |

---

## AsyncAPI — `ksitools-asyncapi-viewer.html`

| | |
|--|--|
| **Accepts** | AsyncAPI 2.x/3.x `.yaml` / `.json`, paste |
| **Limits** | 40 MB |
| **Features** | Channels, operations, messages, servers, schemas; dangling `$ref` / no-messages flags |
| **Exports** | Copy summary · Save `.json` |
| **Library** | Embedded js-yaml for YAML specs |

---

## Terraform state — `ksitools-terraform-state-viewer.html`

| | |
|--|--|
| **Accepts** | `terraform.tfstate`, `.tfstate`, state JSON |
| **Limits** | 30 MB; 5k rows drawn |
| **Features** | Resource inventory; sensitive attributes flagged; secret-like value hits |
| **Not done** | Keep state files offline — they often contain secrets. This viewer does not talk to a backend. |
| **Exports** | Copy summary · Save `.csv` |

---

## Pulumi state — `ksitools-pulumi-state-viewer.html`

| | |
|--|--|
| **Accepts** | Pulumi stack checkpoint `.json` |
| **Limits** | 20 MB; 5k rows drawn; tree capped at 2k nodes |
| **Features** | Resources, parents, outputs — parsed locally |
| **Not done** | Keep stack state offline |
| **Exports** | Copy summary · Save `.csv` |

---

## Argo CD — `ksitools-argocd-viewer.html`

| | |
|--|--|
| **Accepts** | Application / ApplicationSet / AppProject YAML or JSON, multi-doc, paste |
| **Limits** | 25 MB |
| **Features** | Sync policy, sources, destinations across `---` documents |
| **Exports** | Copy summary · Save `.json` |
| **Library** | Embedded js-yaml |

---

## Flux CD — `ksitools-flux-viewer.html`

| | |
|--|--|
| **Accepts** | GitRepository / HelmRepository / Kustomization / HelmRelease YAML, multi-doc, paste |
| **Limits** | 25 MB |
| **Features** | Flux CRD inventory; broken-ref and `disableWait` flags |
| **Exports** | Copy summary · Save `.json` |
| **Library** | Embedded js-yaml |

---

## Tekton — `ksitools-tekton-viewer.html`

| | |
|--|--|
| **Accepts** | Task / Pipeline / PipelineRun / TaskRun YAML or JSON (`tekton.dev/v1`), multi-doc, paste |
| **Limits** | 10 MB |
| **Features** | Pipeline outline; ClusterTask, inline `taskSpec`, unbounded timeout, cycle flags |
| **Exports** | Copy summary · Save `.json` |
| **Library** | Embedded js-yaml |

---

## cert-manager — `ksitools-cert-manager-viewer.html`

| | |
|--|--|
| **Accepts** | Certificate / Issuer / ClusterIssuer YAML, multi-doc, paste |
| **Limits** | 10 MB |
| **Features** | Inventory with DNS-01 / HTTP-01 notes |
| **Exports** | Copy summary · Save `.json` |
| **Library** | Embedded js-yaml |

---

## Istio — `ksitools-istio-viewer.html`

| | |
|--|--|
| **Accepts** | VirtualService / DestinationRule / Gateway / ServiceEntry / PeerAuthentication / AuthorizationPolicy / RequestAuthentication YAML, paste |
| **Limits** | 50 MB |
| **Features** | Traffic CRD outline for mesh cutovers |
| **Exports** | Copy summary · Save `.json` |
| **Library** | Embedded js-yaml |

---

## Kustomize — `ksitools-kustomize-viewer.html`

| | |
|--|--|
| **Accepts** | `kustomization.yaml`, paste |
| **Limits** | 10 MB |
| **Features** | Resources, images, generators, patches, helmCharts, overlays — readable summary, not a build |
| **Not done** | Does not run `kustomize build` or fetch remote bases |
| **Exports** | Copy summary · Save `.json` |
| **Library** | Embedded js-yaml |

---

## ASN.1 / certificates — `ksitools-asn1-viewer.html`

| | |
|--|--|
| **Accepts** | `.pem`, `.cer`, `.crt`, `.der`, `.csr`, `.p7b`/`.p7m`/`.p7s`, `.p12`/`.pfx`, hex/base64 blobs, paste |
| **Features** | Deep ASN.1 DER/BER tree with OID names — certs, CSRs, PKCS#7/#12, raw blobs; copy hex / Base64 / subtree / value |
| **Not done** | Does not verify signatures or chains; PKCS#12 private-key decrypt is inspect-oriented (prefer the PKCS#12 viewer for bundles) |
| **Exports** | Copy hex dump · Copy Base64 · Copy subtree · Copy value |

---

## PASETO tokens — `ksitools-paseto-viewer.html`

| | |
|--|--|
| **Accepts** | `.token`, `.paseto`, pasted `v3.local` / `v4.public` (etc.) strings |
| **Features** | Decode PASETO v3/v4 local and public tokens; expiry / nbf badges |
| **Not done** | **No crypto verify** — footer/payload decode only, like the JWT viewer |
| **Exports** | Decode (on-page) |

---

## DMARC / SPF / DKIM — `ksitools-dmarc-viewer.html`

| | |
|--|--|
| **Accepts** | Aggregate RUA `.xml` (optional `.gz`), DNS TXT (`v=DMARC1`), paste |
| **Limits** | 25 MB |
| **Features** | Policy records and aggregate reports — alignment, p/sp/pct, per-source rollup; optional email masking |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Windows Registry — `ksitools-registry-viewer.html`

| | |
|--|--|
| **Accepts** | `.reg` export (REGEDIT4 / Version 5.00), UTF-16 LE or UTF-8, paste |
| **Limits** | 25 MB |
| **Features** | Registry export tree with typed values; persistence / defense-evasion heuristics; secrets masked (Reveal toggle) |
| **Not done** | Not a live hive / offline NTUSER.DAT parser |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## curl inspector — `ksitools-curl-viewer.html`

| | |
|--|--|
| **Accepts** | Pasted `curl` command (or a text file of one) |
| **Features** | Parse into method, URL, headers, auth, and body; flags for `-k`/`--insecure`, `-L` with credentials, `--resolve`, Basic / API-key |
| **Not done** | Does not execute the request |
| **Exports** | Copy summary |

---

## UUID / ULID — `ksitools-uuid-ulid-viewer.html`

| | |
|--|--|
| **Accepts** | Pasted UUID / ULID strings (one per line) |
| **Features** | Decode version, variant, embedded timestamps; sort ULIDs by time; flags for non-RFC variant, all-zero/all-FF node, far-future |
| **Exports** | On-page decode (copy from table) |

---

## JS beautifier — `ksitools-js-beautify-viewer.html`

| | |
|--|--|
| **Accepts** | `.js`, `.mjs`, `.cjs`, `.jsx`, `.ts`, `.json`, paste |
| **Limits** | Refuse above 100 MB; highlight skipped above 4 MB; 50k lines drawn |
| **Features** | Un-minify JavaScript locally; optional highlight; round-trip / mangled notes |
| **Not done** | Not a full AST pretty-printer / TypeScript type checker |
| **Exports** | Copy all · Download `.js` |

---

## CSS / HTML beautifier — `ksitools-css-html-beautify-viewer.html`

| | |
|--|--|
| **Accepts** | `.css`, `.html`, `.htm`, `.xhtml`, `.svg`, `.scss`, `.less`, paste |
| **Limits** | Refuse above 100 MB; highlight skipped above 4 MB; 50k lines / 3k rows drawn |
| **Features** | Pretty-print minified CSS or HTML in the browser |
| **Not done** | Not a browser layout engine; SCSS/Less are formatted as text, not compiled |
| **Exports** | Copy all · Download |

---

## HOCON — `ksitools-hocon-viewer.html`

| | |
|--|--|
| **Accepts** | `.conf`, `.hocon`, `.properties`, `.json`, paste |
| **Features** | Lightbend/Typesafe HOCON and `.properties` as a collapsible tree |
| **Not done** | Not a full substitutions / includes resolver for every HOCON edge case |
| **Exports** | Copy JSON |

---

## Avro — `ksitools-avro-viewer.html`

| | |
|--|--|
| **Accepts** | `.avsc` schema JSON; `.avro` detected but schema-only |
| **Limits** | 8 MB |
| **Features** | Avro schema tree for topic/contract reviews; well-formed / schema-only badges |
| **Not done** | **OCF binary decode is not supported** in this build — use `.avsc` (or the JSON dump of a schema) |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## BSON — `ksitools-bson-viewer.html`

| | |
|--|--|
| **Accepts** | `.bson`, hex dump, Mongo `bsondump` JSON, paste |
| **Limits** | 64 MB; warn above 16 MB per doc; depth cap 200 |
| **Features** | Typed value tree (`$oid`, `$date`, `$binary`, UUID) |
| **Exports** | Copy summary · Save `.json` |

---

## MessagePack — `ksitools-msgpack-viewer.html`

| | |
|--|--|
| **Accepts** | `.msgpack`, `.bin`, hex, base64, paste |
| **Limits** | 16 MB; depth cap 200 |
| **Features** | Typed value tree from raw MessagePack, hex, or base64 |
| **Exports** | Copy summary · Save `.json` |

---

## CBOR — `ksitools-cbor-viewer.html`

| | |
|--|--|
| **Accepts** | `.cbor`, hex, base64/base64url, paste |
| **Limits** | 16 MB; depth cap 200 |
| **Features** | RFC 7049 / 8949 diagnostic view; COSE/CWT and embedded-CBOR notes |
| **Exports** | Copy summary · Save `.json` |

---

## Arrow / Feather — `ksitools-arrow-viewer.html`

| | |
|--|--|
| **Accepts** | `.arrow`, `.arrows`, `.ipc`, `.feather` |
| **Limits** | 200 MB |
| **Features** | Apache Arrow IPC / Feather schema plus a first-50-rows preview |
| **Not done** | Not a full Arrow compute engine; preview is first rows only |
| **Exports** | On-page schema / preview |

---

## Amazon Ion — `ksitools-ion-viewer.html`

| | |
|--|--|
| **Accepts** | `.ion` text (QLDB-style dumps), paste |
| **Limits** | 5 MB |
| **Features** | Ion text outline for AWS-style interchange files |
| **Not done** | Binary Ion is best-effort; prefer Ion text dumps |
| **Exports** | Copy summary · Save `.json` |

---

## Apache ORC — `ksitools-orc-viewer.html`

| | |
|--|--|
| **Accepts** | `.orc` |
| **Limits** | 200 MB; stripe row sample capped |
| **Features** | ORC footer, schema tree, and stripe stats — parsed locally |
| **Not done** | Not a full stripe decoder / SQL engine |
| **Exports** | On-page schema / stats |

---

## iCalendar / vCard — `ksitools-ical-vcard-viewer.html`

| | |
|--|--|
| **Accepts** | `.ics`, `.vcf`, paste |
| **Limits** | 8 MB |
| **Features** | Events and contacts from ICS/VCF — local parse, no calendar upload |
| **Exports** | Copy summary · Save `.json` |

---

## Cisco IOS / IOS-XE — `ksitools-cisco-ios-viewer.html`

| | |
|--|--|
| **Accepts** | `running-config`, `startup-config`, `.cfg`, `.conf`, paste |
| **Limits** | 10 MB |
| **Features** | Interface, ACL, VLAN, routing, AAA outline; flags for `permit ip any any`, type-7 enable password, CDP, console no login |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Juniper JunOS — `ksitools-junos-viewer.html`

| | |
|--|--|
| **Accepts** | `show configuration` (curly) or set-style `display set` output, `.conf`, paste |
| **Limits** | 10 MB |
| **Features** | Stanza outline — interfaces, firewall, routing-instances; missing root-authentication / interface-no-address flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## F5 BIG-IP — `ksitools-f5-bigip-viewer.html`

| | |
|--|--|
| **Accepts** | `tmsh list` output, `.scf`, `.conf`, UCS extract text, paste |
| **Limits** | 10 MB |
| **Features** | Virtuals, pools, iRules, profiles; VS :443 without client-ssl, VS with no pool flags |
| **Not done** | Does not unpack a binary `.ucs` archive — extract the config text first |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## pfSense / OPNsense — `ksitools-pfsense-viewer.html`

| | |
|--|--|
| **Accepts** | `config.xml` backup, paste |
| **Limits** | 25 MB; 15k rows drawn |
| **Features** | Firewall aliases, rules, NAT, and interfaces via `DOMParser`; unbound/dnsmasq notes |
| **Exports** | Copy summary · Save `.json` |

---

## Routing table — `ksitools-routing-viewer.html`

| | |
|--|--|
| **Accepts** | `ip route`, `netstat -rn`, vendor `show ip route`, paste |
| **Limits** | 20 MB |
| **Features** | Filterable prefix/next-hop table; duplicate prefix+next-hop, missing default, odd next-hop flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Interface / IP state — `ksitools-ipstate-viewer.html`

| | |
|--|--|
| **Accepts** | `ip addr` / `ip -j addr` / `ifconfig -a` / IOS `show ip int brief`, paste |
| **Limits** | 10 MB |
| **Features** | Addresses, flags, link state; DOWN-with-IP, PROMISC, MTU mismatch, no-IP flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Keepalived / Pacemaker — `ksitools-keepalived-pacemaker-viewer.html`

| | |
|--|--|
| **Accepts** | `keepalived.conf`, `corosync.conf`, `cib.xml`, `crm configure show`, `pcs config`, paste |
| **Limits** | 10 MB |
| **Features** | VRRP / PCS cluster outline: VIPs, track scripts, resource constraints; MASTER-no-BACKUP, cleartext VRRP auth flags (`auth_pass` masked) |
| **Exports** | Copy summary · Save `.json` |

---

## SNMP MIB / walk — `ksitools-snmp-viewer.html`

| | |
|--|--|
| **Accepts** | `snmpwalk` / `snmpget` / `snmpbulkwalk` output, `.mib`, paste |
| **Limits** | 20 MB |
| **Features** | MIB tree and walk table for device inventory without a live poller |
| **Not done** | Does not poll devices; not a full SMIv2 compiler |
| **Exports** | Copy summary · Save `.csv` |

---

## FreeRADIUS — `ksitools-radius-viewer.html`

| | |
|--|--|
| **Accepts** | `clients.conf`, `users`, `radiusd.conf`, sites, paste (files can be concatenated) |
| **Limits** | 5 MB |
| **Features** | clients / users / sites outline; secrets masked; empty-secret, listen-*, msg-auth-off flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## OpenVPN / IPsec — `ksitools-openvpn-ipsec-viewer.html`

| | |
|--|--|
| **Accepts** | `.ovpn`, OpenVPN `.conf`, `ipsec.conf`, `.secrets`, paste |
| **Limits** | 5 MB |
| **Features** | Combined VPN review: OpenVPN remotes plus strongSwan/Libreswan conn outlines; keys / tls-auth / ipsec.secrets masked unless revealed; full-tunnel, IKEv1, TLS<1.2 flags |
| **Exports** | Copy summary · Save `.json` |

> Pairs with the dedicated OpenVPN viewer for `.ovpn`-only reviews.

---

## Thread dump — `ksitools-thread-dump-viewer.html`

| | |
|--|--|
| **Accepts** | HotSpot `jstack` / IBM-OpenJ9 `javacore.txt`, `.dump`, `.tdump`, `.log`, `.out` |
| **Limits** | 100 MB refuse; 5k threads / 400 frames drawn |
| **Features** | Java thread dump: states, deadlocks, repeated stacks; multiple dumps per file; optional environment-line reveal |
| **Exports** | Save `.csv` |

---

## GC log — `ksitools-gc-log-viewer.html`

| | |
|--|--|
| **Accepts** | JDK 9+ `-Xlog:gc*` or legacy `-XX:+PrintGCDetails` `.log`/`.gc` |
| **Limits** | Lazy open; 64 MB memory path; refuse above ~8 GB; 200k events kept; 2k plot/table rows |
| **Features** | HotSpot GC log timeline: pauses, heap occupancy, collector notes |
| **Not done** | Not a full GC toolbox (no heap-after-GC histogram reconstruction) |
| **Exports** | Save `.csv` |

---

## Heap dump — `ksitools-heap-dump-viewer.html`

| | |
|--|--|
| **Accepts** | HotSpot `.hprof` (`jmap` / `HeapDumpOnOutOfMemoryError`); `.bin`/`.dmp` accepted |
| **Limits** | Parsed in slices; refuse above ~64 GB; class/string/object caps (histogram / top objects / GC-root sample) |
| **Features** | Class histogram, largest objects, GC roots; sampled strings hidden until revealed |
| **Not done** | **Summary only — no dominator tree**; not MAT/VisualVM parity. IBM `.phd` is not a first-class path |
| **Exports** | Save `.csv` |

---

## JFR — `ksitools-jfr-viewer.html`

| | |
|--|--|
| **Accepts** | Java Flight Recorder `.jfr` (JDK 9+ chunked format) |
| **Limits** | Parsed in slices; refuse above ~16 GB; 20k chunks/types; decode up to 3k events of a chosen type |
| **Features** | Event-type inventory; decode a chosen event type on demand |
| **Not done** | Not JMC — no recording dump of every event by default |
| **Exports** | Save `.csv` |

---

## PostgreSQL config — `ksitools-postgres-config-viewer.html`

| | |
|--|--|
| **Accepts** | `postgresql.conf`, `pg_hba.conf`, paste |
| **Limits** | 5 MB |
| **Features** | GUCs plus pg_hba rules; auth-method and listen/SSL flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Postgres slow log — `ksitools-pg-slowlog-viewer.html`

| | |
|--|--|
| **Accepts** | PostgreSQL csvlog/stderr slow logs, `pg_stat_statements` CSV/JSON, paste |
| **Limits** | 100 MB; 20k rows drawn |
| **Features** | Ranked query table (duration, calls, hit rate, temp/shared blks) |
| **Exports** | Copy summary · Save `.csv` |

---

## MySQL slow / binlog — `ksitools-mysql-log-viewer.html`

| | |
|--|--|
| **Accepts** | MySQL slow-query log, `mysqlbinlog -v` text, paste |
| **Limits** | 100 MB; 20k rows drawn |
| **Features** | Slow-query and text binlog statement outline; exec_time / lock / large-txn / missing COMMIT flags |
| **Not done** | Does not parse binary binlog — run `mysqlbinlog -v` first |
| **Exports** | Copy summary · Save `.csv` |

---

## Oracle AWR / .ora — `ksitools-oracle-awr-viewer.html`

| | |
|--|--|
| **Accepts** | AWR text/HTML (`awrptext` / `awrpthtml`), `listener.ora` / `tnsnames.ora` / `sqlnet.ora`, paste |
| **Limits** | 50 MB; 5k rows drawn |
| **Features** | AWR highlights (instance efficiency, file IO, top SQL) plus `.ora` outline |
| **Not done** | Not a full AWR warehouse / OEM replacement |
| **Exports** | Copy summary · Save `.csv` |

---

## Solr query response — `ksitools-solr-query-viewer.html`

| | |
|--|--|
| **Accepts** | `/select?wt=json` response, `.json`, paste |
| **Limits** | 25 MB |
| **Features** | numFound, docs, facets, highlighting; SolrCloud / cursor / huge-result / error-in-body flags |
| **Exports** | Copy summary · Save `.json` · Save `.csv` |

---

## Solr schema — `ksitools-solr-schema-viewer.html`

| | |
|--|--|
| **Accepts** | `schema.xml`, `managed-schema`, paste |
| **Limits** | 8 MB |
| **Features** | Fields, types, copyFields, uniqueKey; dead-field, missing dest, catch-all copyField, `_version_` flags |
| **Exports** | Copy summary · Save `.json` |

---

## solrconfig.xml — `ksitools-solrconfig-viewer.html`

| | |
|--|--|
| **Accepts** | `solrconfig.xml`, paste |
| **Limits** | 5 MB |
| **Features** | Request handlers, update chains, caches, directories; `/select` missing, SolrCloud vs standalone, autoCommit notes |
| **Exports** | Copy summary |

---

## Solr explain — `ksitools-solr-explain-viewer.html`

| | |
|--|--|
| **Accepts** | `debug.explain` JSON, wrapped debug JSON, or raw explain string, paste |
| **Limits** | 8 MB |
| **Features** | Score explain as a readable tree (BM25, coord, idf-near-0, matched/not-matched) |
| **Exports** | Copy summary · Expand/collapse all |

---

## Solr cluster — `ksitools-solr-cluster-viewer.html`

| | |
|--|--|
| **Accepts** | `CLUSTERSTATUS` JSON or `clusterstate.json`, paste |
| **Features** | Collections, shards, replicas, and health |
| **Exports** | On-page parse |

---

## Solr DIH — `ksitools-solr-dih-viewer.html`

| | |
|--|--|
| **Accepts** | `data-config.xml`, paste |
| **Features** | DataImportHandler entities, dataSources, transformers; password-in-URL and missing-pk flags |
| **Exports** | On-page parse |

---

## Solr ZooKeeper znodes — `ksitools-solr-znode-viewer.html`

| | |
|--|--|
| **Accepts** | Solr admin ZooKeeper JSON or `zkCli` path listing, paste |
| **Features** | Znode path inventory for SolrCloud config sets |
| **Exports** | On-page parse |

---
