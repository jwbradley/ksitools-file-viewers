# KSI Tools File Viewers

**Private, offline-first file viewers that run entirely in your browser.**

Drop a file onto a page. It is parsed and displayed locally. Nothing is uploaded, nothing is stored on a server, and there is no backend. The suite is **156 single-file viewers** plus a searchable hub.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Privacy: local-only](https://img.shields.io/badge/Privacy-local--only-brightgreen.svg)](docs/privacy.md)
[![Dependencies: offline](https://img.shields.io/badge/Network-none%20required-lightgrey.svg)](docs/architecture.md)

---

## Why these exist

Ops, support, and engineering work often means opening someone else’s JSON export, a production log, a YAML config, or a SQLite dump — sometimes on a machine that should not send customer data anywhere.

These viewers are single HTML files you can open from disk, share on a USB stick, or host on a static site. Each one is self-contained: CSS, JS, and (where needed) embedded libraries ship inside the file. No CDN. No install. No account.

Some archive/Excel inflate paths use the browser built-in **DecompressionStream** API (no extra download).

## Viewers

Open [`index.html`](index.html) and type in the search box (or press `/`) to filter by name or description. `Esc` clears. Groups with no matches hide automatically.

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
| **File type sniffer** | [`ksitools-filetype-viewer.html`](ksitools-filetype-viewer.html) | Magic-byte / hex sniff; jump to a matching viewer |
| **Maven POM** | [`ksitools-maven-pom-viewer.html`](ksitools-maven-pom-viewer.html) | Dependencies, plugins, modules, and parent POM outline |
| **package.json** | [`ksitools-package-json-viewer.html`](ksitools-package-json-viewer.html) | Scripts, deps, engines, and supply-chain flags |

### Host & network config

| Viewer | File | What it does |
|--------|------|----------------|
| **systemd unit / timer** | [`ksitools-systemd-viewer.html`](ksitools-systemd-viewer.html) | Unit/timer section outline; User/ExecStart/WantedBy summary + hygiene flags |
| **Proxy / LB config** | [`ksitools-proxy-viewer.html`](ksitools-proxy-viewer.html) | nginx / Apache / HAProxy / Caddy outline; TLS & cleartext-upstream flags |
| **Firewall rules** | [`ksitools-firewall-viewer.html`](ksitools-firewall-viewer.html) | iptables-save / nftables / UFW / firewalld → rule table; world-open / SSH flags |
| **DNS / CoreDNS** | [`ksitools-dns-viewer.html`](ksitools-dns-viewer.html) | BIND zone records + CoreDNS Corefile plugins for cutovers |
| **sudoers** | [`ksitools-sudoers-viewer.html`](ksitools-sudoers-viewer.html) | Defaults/aliases/specs; NOPASSWD:ALL and !authenticate flags |
| **Host sysfiles** | [`ksitools-sysfiles-viewer.html`](ksitools-sysfiles-viewer.html) | fstab / hosts / exports / resolv.conf cutover tables |
| **fail2ban** | [`ksitools-fail2ban-viewer.html`](ksitools-fail2ban-viewer.html) | Jail + filter outline; permanent bantime / failregex flags |
| **SSH known_hosts** | [`ksitools-known-hosts-viewer.html`](ksitools-known-hosts-viewer.html) | Host-key fingerprints; hashed / @revoked / @cert-authority |
| **nginx config** | [`ksitools-nginx-config-viewer.html`](ksitools-nginx-config-viewer.html) | Server/location outline; TLS, reverse-proxy, and listen flags |
| **Apache httpd** | [`ksitools-apache-config-viewer.html`](ksitools-apache-config-viewer.html) | VirtualHost / Directory / Rewrite outline; TLS and access-control flags |
| **HAProxy** | [`ksitools-haproxy-config-viewer.html`](ksitools-haproxy-config-viewer.html) | Frontend/backend/listen outline; TLS, ACLs, backend health notes |
| **fstab / mounts** | [`ksitools-fstab-viewer.html`](ksitools-fstab-viewer.html) | Mount table: options, dump/pass, nofail, bind/nfs flags |
| **sshd_config** | [`ksitools-sshd-config-viewer.html`](ksitools-sshd-config-viewer.html) | PermitRootLogin, PasswordAuthentication, and listen flags |
| **sysctl** | [`ksitools-sysctl-viewer.html`](ksitools-sysctl-viewer.html) | Kernel tunables from sysctl.conf / sysctl.d |
| **PAM stack** | [`ksitools-pam-viewer.html`](ksitools-pam-viewer.html) | Auth stack outline; sufficient/required and include/substack notes |
| **NFS exports** | [`ksitools-nfs-exports-viewer.html`](ksitools-nfs-exports-viewer.html) | `/etc/exports` table; no_root_squash, insecure, wildcard-client flags |
| **Samba smb.conf** | [`ksitools-smb-conf-viewer.html`](ksitools-smb-conf-viewer.html) | Global/share outline; guest ok, browseable, path inventory |
| **logrotate** | [`ksitools-logrotate-viewer.html`](ksitools-logrotate-viewer.html) | Rotate stanzas: frequency, size, compress, postrotate hooks |
| **Package repo sources** | [`ksitools-repos-viewer.html`](ksitools-repos-viewer.html) | apt/yum/dnf/pacman repo files — enabled, baseurl, GPG flags |
| **limits.conf** | [`ksitools-limits-conf-viewer.html`](ksitools-limits-conf-viewer.html) | ulimit / pam_limits: domain, type, item, value table |
| **audit.rules** | [`ksitools-audit-rules-viewer.html`](ksitools-audit-rules-viewer.html) | auditd watches, syscalls, keys, always-on flags |
| **Postfix** | [`ksitools-postfix-viewer.html`](ksitools-postfix-viewer.html) | Mailer outline; relay, TLS; password-like values masked |
| **DHCPd / Kea** | [`ksitools-dhcpd-viewer.html`](ksitools-dhcpd-viewer.html) | ISC dhcpd or Kea dhcp4 subnets, ranges, options, failover |

### Platform ops

| Viewer | File | What it does |
|--------|------|----------------|
| **Observability** | [`ksitools-observability-viewer.html`](ksitools-observability-viewer.html) | Prometheus scrape/rules, Alertmanager routes/receivers, Grafana panel inventory |
| **CI pipeline** | [`ksitools-ci-viewer.html`](ksitools-ci-viewer.html) | GitHub Actions / GitLab CI / Azure Pipelines jobs & steps outline |
| **SSH config** | [`ksitools-ssh-config-viewer.html`](ksitools-ssh-config-viewer.html) | `~/.ssh/config` Host/Match blocks; ProxyJump + StrictHostKeyChecking flags |
| **WireGuard** | [`ksitools-wireguard-viewer.html`](ksitools-wireguard-viewer.html) | Interface/peers; keys masked by default; full-tunnel AllowedIPs flags |
| **OpenVPN** | [`ksitools-openvpn-viewer.html`](ksitools-openvpn-viewer.html) | `.ovpn` remotes + inline PEM blocks masked; cipher/auth/full-tunnel flags |
| **Ansible** | [`ksitools-ansible-viewer.html`](ksitools-ansible-viewer.html) | Inventory (INI/YAML) + playbook plays/roles/tasks summary |
| **Fluent Bit / Fluentd** | [`ksitools-fluent-config-viewer.html`](ksitools-fluent-config-viewer.html) | Input/filter/output pipeline outline for log-shipper configs |
| **Vector** | [`ksitools-vector-viewer.html`](ksitools-vector-viewer.html) | Sources, transforms, and sinks for Vector pipelines |
| **Telegraf** | [`ksitools-telegraf-viewer.html`](ksitools-telegraf-viewer.html) | Input/output/processor plugins and agent intervals |
| **OTel Collector** | [`ksitools-otel-collector-viewer.html`](ksitools-otel-collector-viewer.html) | Receivers, processors, exporters, and pipelines |
| **Loki streams** | [`ksitools-loki-viewer.html`](ksitools-loki-viewer.html) | LogQL / Loki stream dumps as a filterable log table |
| **Jaeger traces** | [`ksitools-jaeger-viewer.html`](ksitools-jaeger-viewer.html) | Trace/span timeline from a Jaeger JSON export |
| **Prometheus exposition** | [`ksitools-promex-viewer.html`](ksitools-promex-viewer.html) | Scraped `/metrics` dump → metric families and labels |
| **Splunk .conf** | [`ksitools-splunk-conf-viewer.html`](ksitools-splunk-conf-viewer.html) | Stanza inventory for props, transforms, inputs, outputs, indexes |
| **rsyslog** | [`ksitools-rsyslog-viewer.html`](ksitools-rsyslog-viewer.html) | Legacy and RainerScript rules; templates, modules, forwarding |
| **journald JSON** | [`ksitools-journald-viewer.html`](ksitools-journald-viewer.html) | `journalctl` JSON/JSONL export as a filterable event table |
| **auditd log** | [`ksitools-auditd-log-viewer.html`](ksitools-auditd-log-viewer.html) | audit.log records: type, syscall, exe, key, success/fail |
| **sar / sysstat** | [`ksitools-sar-viewer.html`](ksitools-sar-viewer.html) | Text sar/iostat/mpstat dumps as CPU/IO tables (not binary sadc) |

### Enterprise / migration

| Viewer | File | What it does |
|--------|------|----------------|
| **LDAP LDIF** | [`ksitools-ldif-viewer.html`](ksitools-ldif-viewer.html) | Directory entry inventory; credential attrs masked; duplicate-DN flags |
| **Dependency lockfiles** | [`ksitools-lockfile-viewer.html`](ksitools-lockfile-viewer.html) | Pins from npm / Go / Cargo / Poetry / pip (complements SBOM) |
| **Protobuf** | [`ksitools-proto-viewer.html`](ksitools-proto-viewer.html) | `.proto` message/enum/service/RPC outline; duplicate field # flags |
| **Policy-as-code** | [`ksitools-policy-viewer.html`](ksitools-policy-viewer.html) | Kyverno / Gatekeeper / OPA Rego policy inventory |
| **Java KeyStore** | [`ksitools-jks-viewer.html`](ksitools-jks-viewer.html) | JKS/JCEKS aliases & cert chains; private keys never displayed |
| **Kyverno** | [`ksitools-kyverno-viewer.html`](ksitools-kyverno-viewer.html) | ClusterPolicy / Policy inventory: validate, mutate, generate, verifyImages |
| **Rego / OPA** | [`ksitools-rego-viewer.html`](ksitools-rego-viewer.html) | Structural outline of OPA Rego (tokenizer, not an evaluator) |
| **JSON Schema** | [`ksitools-jsonschema-viewer.html`](ksitools-jsonschema-viewer.html) | Draft schema tree: types, required, `$ref`, constraints |
| **GraphQL schema** | [`ksitools-graphql-viewer.html`](ksitools-graphql-viewer.html) | SDL outline of types, queries, mutations, subscriptions — no execution |
| **AsyncAPI** | [`ksitools-asyncapi-viewer.html`](ksitools-asyncapi-viewer.html) | Channels, operations, messages, and servers for event APIs |

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
| **Terraform state** | [`ksitools-terraform-state-viewer.html`](ksitools-terraform-state-viewer.html) | State resource inventory; sensitive values flagged — keep offline |
| **Pulumi state** | [`ksitools-pulumi-state-viewer.html`](ksitools-pulumi-state-viewer.html) | Stack checkpoint: resources, parents, and outputs |

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
| **Argo CD** | [`ksitools-argocd-viewer.html`](ksitools-argocd-viewer.html) | Application / ApplicationSet / AppProject: sync policy, sources, destinations |
| **Flux CD** | [`ksitools-flux-viewer.html`](ksitools-flux-viewer.html) | GitRepository, HelmRelease, Kustomization, and related Flux CRDs |
| **Tekton** | [`ksitools-tekton-viewer.html`](ksitools-tekton-viewer.html) | Task / Pipeline / PipelineRun / TaskRun outline |
| **cert-manager** | [`ksitools-cert-manager-viewer.html`](ksitools-cert-manager-viewer.html) | Certificate, Issuer, ClusterIssuer; DNS-01 / HTTP-01 notes |
| **Istio** | [`ksitools-istio-viewer.html`](ksitools-istio-viewer.html) | VirtualService, DestinationRule, Gateway, and related traffic CRDs |
| **Kustomize** | [`ksitools-kustomize-viewer.html`](ksitools-kustomize-viewer.html) | Bases, resources, patches, images, overlays from kustomization.yaml |

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
| **ASN.1 / certificates** | [`ksitools-asn1-viewer.html`](ksitools-asn1-viewer.html) | DER/BER tree with OID names — certs, CSRs, PKCS#7/#12, raw blobs |
| **PASETO tokens** | [`ksitools-paseto-viewer.html`](ksitools-paseto-viewer.html) | Decode PASETO v3/v4 local and public tokens (no crypto verify) |
| **DMARC / SPF / DKIM** | [`ksitools-dmarc-viewer.html`](ksitools-dmarc-viewer.html) | Policy records and aggregate reports — alignment, p/sp/pct, per-source rollup |
| **Windows Registry** | [`ksitools-registry-viewer.html`](ksitools-registry-viewer.html) | Registry export tree with typed values; persistence / defense-evasion heuristics |

### Utilities

| Viewer | File | What it does |
|--------|------|----------------|
| **URL Encode / Decode** | [`ksitools-url-viewer.html`](ksitools-url-viewer.html) | Percent-encode/decode; inspect URL components & query params |
| **Hex encode / decode** | [`ksitools-hexcodec-viewer.html`](ksitools-hexcodec-viewer.html) | hex2bin / bin2hex codec with optional URL-decode chain |
| **Regex Tester** | [`ksitools-regex-viewer.html`](ksitools-regex-viewer.html) | Live JS regex with captures, highlight, and replace |
| **Crontab** | [`ksitools-crontab-viewer.html`](ksitools-crontab-viewer.html) | Plain-English schedule explanation + next run times |
| **Timestamp & Date** | [`ksitools-timestamp-viewer.html`](ksitools-timestamp-viewer.html) | Epoch, Julian, DB2, ISO 8601 conversion; world clocks |
| **JSON Query (jq)** | [`ksitools-json-query-viewer.html`](ksitools-json-query-viewer.html) | jq-subset filter/transform for JSON and NDJSON |
| **curl inspector** | [`ksitools-curl-viewer.html`](ksitools-curl-viewer.html) | Parse a curl command into method, URL, headers, auth, and body |
| **UUID / ULID** | [`ksitools-uuid-ulid-viewer.html`](ksitools-uuid-ulid-viewer.html) | Decode version, variant, and embedded timestamps; sort ULIDs by time |
| **JS beautifier** | [`ksitools-js-beautify-viewer.html`](ksitools-js-beautify-viewer.html) | Un-minify JavaScript locally for review |
| **CSS / HTML beautifier** | [`ksitools-css-html-beautify-viewer.html`](ksitools-css-html-beautify-viewer.html) | Pretty-print minified CSS or HTML in the browser |

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
| **HOCON** | [`ksitools-hocon-viewer.html`](ksitools-hocon-viewer.html) | Lightbend/Typesafe HOCON and `.properties` as a collapsible tree |
| **Avro** | [`ksitools-avro-viewer.html`](ksitools-avro-viewer.html) | Avro schema (`.avsc`) tree for topic/contract reviews (schema-only) |
| **BSON** | [`ksitools-bson-viewer.html`](ksitools-bson-viewer.html) | Mongo BSON / bsondump JSON inspector — `$oid`, `$date`, `$binary` |
| **MessagePack** | [`ksitools-msgpack-viewer.html`](ksitools-msgpack-viewer.html) | MessagePack binary (or hex/base64) as a typed value tree |
| **CBOR** | [`ksitools-cbor-viewer.html`](ksitools-cbor-viewer.html) | CBOR diagnostic view from raw bytes, hex, or base64 |
| **Arrow / Feather** | [`ksitools-arrow-viewer.html`](ksitools-arrow-viewer.html) | Apache Arrow IPC / Feather schema plus a first-rows preview |
| **Amazon Ion** | [`ksitools-ion-viewer.html`](ksitools-ion-viewer.html) | Ion text/binary outline for AWS-style interchange files |
| **Apache ORC** | [`ksitools-orc-viewer.html`](ksitools-orc-viewer.html) | ORC footer, schema tree, and stripe stats — parsed locally |
| **iCalendar / vCard** | [`ksitools-ical-vcard-viewer.html`](ksitools-ical-vcard-viewer.html) | Events and contacts from ICS/VCF — local parse, no calendar upload |

### Network gear & VPN

| Viewer | File | What it does |
|--------|------|----------------|
| **Cisco IOS / IOS-XE** | [`ksitools-cisco-ios-viewer.html`](ksitools-cisco-ios-viewer.html) | Interface, ACL, VLAN, routing, and AAA outline from a Cisco dump |
| **Juniper JunOS** | [`ksitools-junos-viewer.html`](ksitools-junos-viewer.html) | Stanza outline for set/curly configs — interfaces, firewall, routing-instances |
| **F5 BIG-IP** | [`ksitools-f5-bigip-viewer.html`](ksitools-f5-bigip-viewer.html) | Virtuals, pools, iRules, and profiles from a BIG-IP / tmsh list |
| **pfSense / OPNsense** | [`ksitools-pfsense-viewer.html`](ksitools-pfsense-viewer.html) | Firewall aliases, rules, NAT, and interfaces from a `config.xml` backup |
| **Routing table** | [`ksitools-routing-viewer.html`](ksitools-routing-viewer.html) | `ip route` / netstat / vendor route dumps as a filterable table |
| **Interface / IP state** | [`ksitools-ipstate-viewer.html`](ksitools-ipstate-viewer.html) | Addresses, flags, and link state from `ip addr`, ifconfig, or IOS |
| **Keepalived / Pacemaker** | [`ksitools-keepalived-pacemaker-viewer.html`](ksitools-keepalived-pacemaker-viewer.html) | VRRP / PCS cluster outline: VIPs, track scripts, resource constraints |
| **SNMP MIB / walk** | [`ksitools-snmp-viewer.html`](ksitools-snmp-viewer.html) | MIB tree and snmpwalk output for device inventory (no live poller) |
| **FreeRADIUS** | [`ksitools-radius-viewer.html`](ksitools-radius-viewer.html) | clients, users, and sites configs — secrets masked by default |
| **OpenVPN / IPsec** | [`ksitools-openvpn-ipsec-viewer.html`](ksitools-openvpn-ipsec-viewer.html) | OpenVPN remotes plus strongSwan/Libreswan IPsec conn outlines |

### JVM diagnostics

| Viewer | File | What it does |
|--------|------|----------------|
| **Thread dump** | [`ksitools-thread-dump-viewer.html`](ksitools-thread-dump-viewer.html) | Java thread dump: states, deadlocks, repeated stacks; multiple dumps per file |
| **GC log** | [`ksitools-gc-log-viewer.html`](ksitools-gc-log-viewer.html) | HotSpot GC log timeline: pauses, heap occupancy, collector notes |
| **Heap dump** | [`ksitools-heap-dump-viewer.html`](ksitools-heap-dump-viewer.html) | Class histogram, largest objects, GC roots (summary only — no dominator tree) |
| **JFR** | [`ksitools-jfr-viewer.html`](ksitools-jfr-viewer.html) | Flight Recorder event-type inventory; decode a chosen event type on demand |

### Databases

| Viewer | File | What it does |
|--------|------|----------------|
| **PostgreSQL config** | [`ksitools-postgres-config-viewer.html`](ksitools-postgres-config-viewer.html) | GUCs plus pg_hba rules; auth-method and listen/SSL flags |
| **Postgres slow log** | [`ksitools-pg-slowlog-viewer.html`](ksitools-pg-slowlog-viewer.html) | Slow-query log or pg_stat_statements CSV/JSON as a ranked query table |
| **MySQL slow / binlog** | [`ksitools-mysql-log-viewer.html`](ksitools-mysql-log-viewer.html) | MySQL slow-query log and text binlog statement outline |
| **Oracle AWR / .ora** | [`ksitools-oracle-awr-viewer.html`](ksitools-oracle-awr-viewer.html) | AWR HTML/text highlights and Oracle `.ora` file outline |

### Solr

| Viewer | File | What it does |
|--------|------|----------------|
| **Solr query response** | [`ksitools-solr-query-viewer.html`](ksitools-solr-query-viewer.html) | `/select` JSON: numFound, docs, facets, highlighting |
| **Solr schema** | [`ksitools-solr-schema-viewer.html`](ksitools-solr-schema-viewer.html) | Fields, types, copyFields, and uniqueKey from a Solr schema file |
| **solrconfig.xml** | [`ksitools-solrconfig-viewer.html`](ksitools-solrconfig-viewer.html) | Request handlers, update chains, caches, and directories |
| **Solr explain** | [`ksitools-solr-explain-viewer.html`](ksitools-solr-explain-viewer.html) | Score explain / debug JSON as a readable tree for relevance tuning |
| **Solr cluster** | [`ksitools-solr-cluster-viewer.html`](ksitools-solr-cluster-viewer.html) | CLUSTERSTATUS / clusterstate.json: collections, shards, replicas, health |
| **Solr DIH** | [`ksitools-solr-dih-viewer.html`](ksitools-solr-dih-viewer.html) | DataImportHandler entities, dataSources, and transformers |
| **Solr ZooKeeper znodes** | [`ksitools-solr-znode-viewer.html`](ksitools-solr-znode-viewer.html) | Znode path inventory from zkCli listings or Solr admin ZooKeeper JSON |

Detailed feature notes for every tool: [docs/viewers.md](docs/viewers.md)

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

# then open http://localhost:8080/  (hub search: type to filter, / to focus)
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

**Tip:** Bookmark the hub (`index.html`) and press `/` to search, or pin the three viewers you use most. For regulated data, open the HTML from a local path you control, not from an untrusted mirror.
