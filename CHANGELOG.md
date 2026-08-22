# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html)
for tagged releases once they begin.

## [Unreleased]

### Added

- Future enhancements spec ([docs/future-enhancements.md](docs/future-enhancements.md)) for HAR privacy/timing, git objects/pack, PE/ELF metadata, FAT/ISO browse, package guts (deb/rpm/jar), and **Terraform daily drivers** (plan/state gaps, `.terraform.lock.hcl`, CLI plan/apply logs, module+vars inventory, graph DOT, cost-estimate JSON, Checkov/tfsec/Terrascan in SARIF)


- **Research-pack viewers** (82 new single-file tools plus three shelf items). Hub catalog is now **156 viewers**. Grouped as on [`index.html`](index.html) / [README.md](README.md):
  - **Migration / packaging:** File type sniffer, Maven POM, package.json
  - **Host & network config:** nginx, Apache httpd, HAProxy, fstab, sshd_config, sysctl, PAM, NFS exports, Samba smb.conf, logrotate, package repo sources, limits.conf, audit.rules, Postfix, DHCPd/Kea
  - **Platform ops:** Fluent Bit/Fluentd, Vector, Telegraf, OTel Collector, Loki, Jaeger, Prometheus exposition, Splunk `.conf`, rsyslog, journald JSON, auditd log, sar/sysstat
  - **Enterprise / APIs:** Kyverno, Rego/OPA, JSON Schema, GraphQL schema, AsyncAPI
  - **Cloud IaC:** Terraform state, Pulumi state
  - **Kubernetes / GitOps:** Argo CD, Flux CD, Tekton, cert-manager, Istio, Kustomize
  - **Security:** ASN.1/certificates, PASETO tokens, DMARC/SPF/DKIM, Windows Registry
  - **Utilities:** curl inspector, UUID/ULID, JS beautifier, CSS/HTML beautifier
  - **Data formats:** HOCON, Avro, BSON, MessagePack, CBOR, Arrow/Feather, Amazon Ion, Apache ORC, iCalendar/vCard
  - **Network gear & VPN:** Cisco IOS/IOS-XE, Juniper JunOS, F5 BIG-IP, pfSense/OPNsense, routing table, interface/IP state, Keepalived/Pacemaker, SNMP MIB/walk, FreeRADIUS, OpenVPN/IPsec
  - **JVM diagnostics:** Thread dump, GC log, Heap dump, JFR
  - **Databases:** PostgreSQL config, Postgres slow log, MySQL slow/binlog, Oracle AWR/.ora
  - **Solr:** query response, schema, solrconfig.xml, explain, cluster, DIH, ZooKeeper znodes
- Hub (`index.html`) lists every viewer and gains a client-side **search filter** (`/` to focus, `Esc` to clear; empty groups hide)
- README catalog updated to match the hub (156 tools, including the four new groups: Network gear & VPN, JVM diagnostics, Databases, Solr)
- Viewer reference ([docs/viewers.md](docs/viewers.md)) expanded with Accepts / Limits / Features / Exports for the research-pack tools

- **Tier-A host access & cutover viewers** (research fan-out pack):
  - **sudoers** (`ksitools-sudoers-viewer.html`) — Defaults/aliases/privilege specs; NOPASSWD:ALL / bare ALL / !authenticate flags
  - **OpenVPN** (`ksitools-openvpn-viewer.html`) — `.ovpn` remotes + inline `<ca>`/`<cert>`/`<key>` masked by default
  - **Host sysfiles** (`ksitools-sysfiles-viewer.html`) — fstab / hosts / exports / resolv.conf with cutover risk flags
  - **fail2ban** (`ksitools-fail2ban-viewer.html`) — jail/filter INI; permanent bantime and failregex inventory
  - **SSH known_hosts** (`ksitools-known-hosts-viewer.html`) — SHA256 fingerprints; hashed hosts, @revoked, @cert-authority
- Hub Host & network / Platform ops sections updated; README / architecture / viewer reference updated
- **Tier-3 enterprise / migration viewers** (full set):
  - **LDAP LDIF** (`ksitools-ldif-viewer.html`) — RFC 2849-ish entry inventory; credential attributes masked by default; duplicate-DN / changetype / external-URL flags
  - **Dependency lockfiles** (`ksitools-lockfile-viewer.html`) — pinned packages from package-lock / npm-shrinkwrap, go.mod/go.sum, Cargo.lock, poetry.lock, requirements.txt (+ Pipfile.lock / composer.lock / Gemfile.lock)
  - **Protobuf** (`ksitools-proto-viewer.html`) — `.proto` message/enum/service/RPC outline; duplicate field numbers; proto2 `required` warnings
  - **Policy-as-code** (`ksitools-policy-viewer.html`) — Kyverno / Gatekeeper YAML + OPA Rego inventory; Enforce/Audit and mutate/validate flags
  - **Java KeyStore** (`ksitools-jks-viewer.html`) — JKS/JCEKS alias & cert-chain inspect; integrity verify; private key bytes never displayed
- Hub **Enterprise / migration** section expanded; README / architecture / viewer reference updated
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

- **Windows Registry** file renamed `ksitools-reg-viewer.html` → [`ksitools-registry-viewer.html`](ksitools-registry-viewer.html) (`git mv`; hub and README links follow)
- Research-pack HTML branded to **KSI Tools** (display name, `ksitools-*-viewer.html` cross-links, `ksitools.tools.*` sessionStorage) to match the rest of the suite
- **Base64 / Secrets** upgraded to live two-pane encode/decode workspace: URL-safe (base64url), MIME wrap, strip-whitespace option, line-by-line mode, swap, Live mode, binary Save, and clearer K8s Secret/ConfigMap data-key panel
- **SQLite viewer:** multi‑GB databases open lazily (Worker + File range reads) with pagination, cell truncation, and WAL warnings — no full-file ArrayBuffer load above 64 MB
- Product branding set to **KSI Tools**; viewer files named `ksitools-*-viewer.html`; docs and hub updated; intended GitHub repo `ksitools-file-viewers`

### Notes

See [README.md](README.md) for the full catalog and [docs/viewers.md](docs/viewers.md) for per-tool Accepts / Limits / Features / Exports.
