# Future enhancements: ops binaries, Terraform daily drivers, and ops paste-targets

Roadmap for the next ops-oriented viewers. Implement against
[`CONTRIBUTING.md`](../CONTRIBUTING.md) and the house rules below. When a viewer
lands, copy its Accepts / Limits / Features / Exports table into
[`docs/viewers.md`](viewers.md) and mark that section done here.

Three sources:

- A landscape pass against “drop anything” forensic suites (HAR, git, PE/ELF,
  disk images, packages). KSI Tools stays a **structured ops viewer**, not a
  1,200-format identifier.
- Daily Terraform work. We already ship plan JSON, state, and a single-file HCL
  outline. The gaps below are the other files a practitioner actually opens
  (lockfile, CLI logs, module roots, graph DOT, cost JSON) plus a few honest
  upgrades to the three Terraform pages we have.
- Remaining paste-targets from the ChorusOps Stage 10+ hand-off that KSI does
  not already ship (SQL `EXPLAIN`, `hs_err_pid`, ALB access logs, VPC Flow Logs,
  pprof / folded stacks) plus two upgrades to pages we have (log-viewer
  lazy-slice, EMV names on the ASN.1 tree). Stages 10–13 of that hand-off are
  already in this repo — do not rebuild them.

---

## 0. Constraints (non-negotiable)

Same rules as every other page in this repo:

1. **Local only.** Parse in the tab. No CDN, no WASM fetch, no telemetry.
   Prefer `DataView` + `DecompressionStream` over new embedded engines.
   If a library is unavoidable, vendor it inside the HTML (sql.js pattern).
2. **One file, one viewer.** `ksitools-*-viewer.html` opens standalone.
   End users get no build step.
3. **Brand.** Display name **KSI Tools**. Session keys `ksitools.tools.*`.
4. **Honest limits.** `MAX_BYTES` / `MAX_RENDER` / `MAX_NODES` in the UI, not
   only in comments. Prefer `File.slice` over slurping a multi-GB image.
5. **Ops UX.** Sticky toolbar, `prefers-color-scheme`, useful errors, useful
   exports. Mask secrets by default (Authorization, cookies, PEM inside
   packages, PE overlay strings that look like keys, ALB query tokens, JVM
   crash `password=` / JDBC URLs, EMV PAN).
6. **No customer data in fixtures.** Synthetic files only. No live tokens,
   hostnames of real customers, private keys, or real card PANs.
7. **Large files (house idiom).** Anything that routinely exceeds ~64 MB
   (GC logs, heap dumps, JFR, disk images, ALB / VPC logs, the generic log
   viewer) copies the sqlite / GC-log pattern — do not invent a new one.
   Read [`ksitools-gc-log-viewer.html`](../ksitools-gc-log-viewer.html) and
   [`ksitools-sqlite-viewer.html`](../ksitools-sqlite-viewer.html) first:

   ```js
   const MEMORY_MAX_BYTES = 64 * 1024 * 1024        // above this → lazy mode
   const ABSURD_MAX_BYTES = 8 * 1024 * 1024 * 1024  // hard refuse (tune per format)
   const MAX_RENDER = 5000                          // DOM row cap
   const useLazy = file.size > MEMORY_MAX_BYTES
   setOpenMode(useLazy ? 'lazy' : 'memory', file.size)  // visible chip
   // lazy path: File.slice / 8 MB windows; never arrayBuffer() the whole file
   ```

   **No silent caps.** Truncation is visible and states what it means for
   search / completeness (copy the GC log wording: counts cover every event;
   the table / CSV / percentiles use the retained sample). Progress UI above
   ~64 MB so a multi-GB scan does not look hung. Binary lazy parsers must
   self-check that a record straddling a slice boundary parses identically
   (GC / heap dump already do this at window sizes 1, 7, 13, 64, …).

There is still **no `package.json` / test runner**. Browser DevTools plus an
on-load self-check (see Telegraf / Vector viewers) is the test harness.

---

## 1. Current state (do not duplicate)

| Ask | Today | Action |
|-----|--------|--------|
| HAR | [`ksitools-har-viewer.html`](../ksitools-har-viewer.html) already exists (200 MB, 5k rows, method/status/URL/type/size/time waterfall, header/body/timing detail) | **Enhance in place** — do not add a second HAR file |
| JAR / ZIP listing | [`ksitools-archive-viewer.html`](../ksitools-archive-viewer.html) lists `.zip` `.jar` `.war` `.ear` `.nupkg` `.tar` / `.tar.gz`, ZIP64, small text preview | Keep as generic archive; **package viewer** adds control/manifest/RPM tags on top |
| RPM / DEB / ISO sniff | File type sniffer recognises them but **jumps to the archive viewer**, which cannot parse them | Re-point `viewer:` after the new pages exist |
| PE / ELF / git objects | Sniffer IDs ELF (`7F ELF`) and PE (`MZ`); `viewer: null`. No git object page | New viewers |
| Terraform plan JSON | [`ksitools-terraform-plan-viewer.html`](../ksitools-terraform-plan-viewer.html) — `terraform show -json` / `resource_changes`, action chips, before/after, secret mask. Also slurps state JSON. | **Enhance in place** — outputs, replace paths, drift, TFC wrap |
| Terraform state | [`ksitools-terraform-state-viewer.html`](../ksitools-terraform-state-viewer.html) — inventory, outputs, sensitive flags. One file. | **Enhance in place** — two-file diff; accept `terraform output -json` |
| HCL outline | [`ksitools-hcl-viewer.html`](../ksitools-hcl-viewer.html) — one `.tf` / `.hcl` / `.tfvars`, brace-matched blocks | Keep as single-file outline. **Do not** turn it into a module graph. Bounce `.terraform.lock.hcl` to the lock viewer |
| Language lockfiles | [`ksitools-lockfile-viewer.html`](../ksitools-lockfile-viewer.html) — npm / Go / Cargo / Poetry / pip / … | **Do not** stuff `.terraform.lock.hcl` in here (wrong job, wrong columns) |
| SARIF / findings | [`ksitools-sarif-viewer.html`](../ksitools-sarif-viewer.html) — SARIF + Trivy/Grype JSON | **Enhance in place** — Checkov / tfsec / Terrascan JSON + Terraform address column |
| `.terraform.lock.hcl`, CLI plan text, `plan -json` JSONL, `terraform graph` DOT, Infracost JSON, multi-file module roots | Nothing dedicated | New Terraform viewers in §4.6–4.13; SARIF in §4.14 |
| SQL dump / slow logs / AWR | [`ksitools-sql-dump-viewer.html`](../ksitools-sql-dump-viewer.html), [`ksitools-pg-slowlog-viewer.html`](../ksitools-pg-slowlog-viewer.html), [`ksitools-mysql-log-viewer.html`](../ksitools-mysql-log-viewer.html), [`ksitools-oracle-awr-viewer.html`](../ksitools-oracle-awr-viewer.html), SQLite `EXPLAIN` inside the DB viewer | **Do not** stuff `EXPLAIN` plans into the dump or slow-log pages. New viewer in §4.15 |
| JVM diagnostics | Thread dump, GC log, heap dump, JFR already ship | **Enhance the set** — crash log (`hs_err_pid`) is missing; JFR stays schema/inventory (no flame graph there) |
| CloudTrail / cloud audit | [`ksitools-cloudtrail-viewer.html`](../ksitools-cloudtrail-viewer.html) + [`ksitools-cloud-audit-viewer.html`](../ksitools-cloud-audit-viewer.html) — API-call JSON | **Do not** parse ALB or VPC Flow there (wrong columns). New viewers in §4.17–4.18 |
| Generic log | [`ksitools-log-viewer.html`](../ksitools-log-viewer.html) — 100 MB `readAsText`, 50k rows, no lazy path | **Enhance in place** — GC-log slice streaming (§4.20). Keep as the default `.log` page |
| Protobuf schema | [`ksitools-proto-viewer.html`](../ksitools-proto-viewer.html) — `.proto` outline only | **Do not** turn it into pprof. New viewer in §4.19 |
| ASN.1 / BER | [`ksitools-asn1-viewer.html`](../ksitools-asn1-viewer.html) — generic DER/BER tree, OID names | **Enhance in place** — EMV tag dictionary overlay (§4.21), not a second BER page |
| Caddy / nginx / Apache / HAProxy | Dedicated nginx/Apache/HAProxy pages plus generic [`ksitools-proxy-viewer.html`](../ksitools-proxy-viewer.html) (already outlines Caddy) | **No** dedicated Caddyfile page unless asked |
| mbox / EML | [`ksitools-mbox-viewer.html`](../ksitools-mbox-viewer.html) already accepts `.eml` | **Do not** add a second EML viewer |

---

## 2. Shared build process

Every new (or enhanced) viewer follows this loop. Do not skip the catalog
steps — a USB-stick copy that cannot find the tool is a failed ship.

Before proposing a **new** page, grep hub cards **and file contents**, not just
filenames. The generic proxy viewer already outlined nginx / Apache / HAProxy
(and still outlines Caddy); `observability-viewer` already inventories Grafana
JSON. A filename miss is how you ship a duplicate.

### 2.1 Scaffold

1. Copy a neighbor, not a blank page.
   - Table + detail pane: **HAR** or **PCAP**.
   - Binary `DataView` walk: **PCAP**, **EVTX**, **JKS**.
   - ZIP members: **Archive** / **Excel**.
   - Inflate: Archive’s `DecompressionStream('gzip'|'deflate-raw')`.
   - Terraform JSON/HCL: **plan**, **state**, or **HCL** outline (copy functions, do not iframe).
   - Streaming text / lazy slice: **GC log** (not the generic log viewer — that
     is the page we are upgrading).
   - SQL / plan trees: **pg-slowlog** or **MySQL log** for table UX; do not
     iframe SQLite.
2. Rename title/heading to **KSI Tools &lt;Thing&gt; Viewer**.
3. Constants at the top: `MAX_BYTES` / `MAX_RENDER`, and — if the format is
   routinely large — `MEMORY_MAX_BYTES`, `ABSURD_MAX_BYTES`, `SLICE_BYTES`.
4. Loading overlay + double-`rAF` before heavy sync work. Progress bar if a
   scan can exceed a couple of seconds.

### 2.2 Parse → model → capped render

- Parse into a plain object/array (search and export use the full model).
- Draw at most `MAX_RENDER` rows. Notice strip when truncated.
- Streaming parsers may keep **aggregates over the whole file** (counts,
  histograms, top-N) while the table uses a retained sample. If so, the UI
  must say which is which — copy the GC log invariant.
- Never assign untrusted bytes to `innerHTML`. `textContent` / `esc()` like HAR.
- Click-through preview of a member must re-cap (e.g. ≤256 KB text, same as archive).

### 2.3 Self-check

Embed a `_selfCheck()` / IIFE that parses the synthetic samples in this doc and
throws on regression. Log `FAIL` to the console; do not block the UI.

Lazy / sliced parsers add a slice-boundary case: the same fixture through
windows of 1, 7, 13, 64, and 4096 bytes must match the whole-file result.

### 2.4 Catalog (land in the same change)

- [ ] `ksitools-<thing>-viewer.html`
- [ ] Card on [`index.html`](../index.html) (group in §5.4)
- [ ] Row in [`README.md`](../README.md)
- [ ] Section in [`docs/viewers.md`](viewers.md)
- [ ] Unreleased note in [`CHANGELOG.md`](../CHANGELOG.md)
- [ ] Sniffer `SIGS[].viewer` in [`ksitools-filetype-viewer.html`](../ksitools-filetype-viewer.html)
- [ ] One-line in [`docs/architecture.md`](architecture.md) parsing table

### 2.5 How to test

```bash
python3 -m http.server 8080 --directory .
```

Drop each fixture in Chromium **and** Firefox. Confirm `file://` still works
(HAR/JSON will; `DecompressionStream` + `File.slice` need a glance on `file://`).

---

## 3. Implementation order

Later tools reuse earlier parsers. Do not start FAT/ISO or RPM until inflate +
TAR listing exist in a copyable shape (they already do in the archive viewer).

```text
1. HAR (enhance existing)          # JSON, privacy, curl handoff
2. Git objects / pack / idx        # zlib inflate + object types
3. PE / ELF metadata               # DataView headers, no execute
4. FAT / ISO browse                # File.slice sector I/O
5. Package guts (deb / rpm / jar)  # ar + tar + zip + RPM lead
```

Ship 1–2 as a first PR if 3–5 need more time. Each viewer must be independently
useful.

**Terraform daily drivers are a parallel track.** They are JSON / HCL / DOT /
plain text — no new inflate or `DataView` work — so they can ship before, after,
or beside the binary pack. Do not block lock/log/module on HAR or PE.

```text
Terraform daily (independent of 1–5)
  A. Plan JSON gaps (enhance existing)
  B. State two-file diff + output-only JSON (enhance existing)
  C. .terraform.lock.hcl                     # new
  D. CLI plan/apply log (text + JSONL)       # new
  E. Module + vars inventory (multi-file)    # new
  F. terraform graph DOT                     # new
  G. Cost estimate JSON (Infracost-shaped)   # new
  H. Checkov / tfsec / Terrascan → SARIF     # enhance existing
```

**Ops paste-targets are a third parallel track.** They are text / JSON /
gzip-protobuf — independent of FAT/ISO/RPM and of Terraform. Do not block
them on HAR or PE. Log-viewer streaming should land before (or with) ALB /
VPC so those pages can copy a finished feeder.

```text
Ops paste-targets (independent of 1–5 and Terraform)
  I.  SQL EXPLAIN (pg / MySQL / Oracle)      # new
  J.  JVM crash log (hs_err_pid)             # new; JVM group
  K.  Log viewer lazy-slice                  # enhance existing — do this before L/M if you can
  L.  ALB access log                         # new
  M.  VPC Flow Log                           # new
  N.  pprof + folded stacks                  # new
  O.  ASN.1 EMV tag overlay                  # enhance existing
```

Suggested calendar (one person, honest):

| Step | Effort | Depends on |
|------|--------|------------|
| HAR gaps | 0.5–1 day | existing HAR |
| Git objects | 2–3 days | `DecompressionStream('deflate')` |
| PE/ELF | 2–3 days | PCAP-style DataView |
| FAT/ISO | 3–4 days | `File.slice`, archive tree UI |
| Packages | 3–4 days | archive ZIP/TAR + new Unix `ar` + RPM header |
| Plan JSON gaps | 0.5 day | existing plan viewer |
| State diff | 1 day | existing state viewer |
| Terraform lock | 1 day | HCL outline tokenizer |
| Terraform CLI log | 1.5–2 days | plan-viewer row model |
| Module + vars | 2–3 days | HCL outline + multi-file drop |
| Graph DOT | 1 day | table UI (no graph library) |
| Cost JSON | 1 day | plan-viewer table |
| SARIF Checkov/tfsec | 0.5 day | existing SARIF viewer |
| SQL EXPLAIN | 1.5–2 days | table UI; no DB connection |
| JVM crash log | 1–1.5 days | thread-dump section nav + mask |
| Log-viewer lazy-slice | 1–1.5 days | GC-log slice feeder |
| ALB access log | 1–1.5 days | log-viewer / GC streaming |
| VPC Flow Log | 1–1.5 days | same feeder as ALB |
| pprof + folded stacks | 2 days | `DecompressionStream('gzip')`; table UI (no flame library) |
| ASN.1 EMV overlay | 0.5–1 day | existing ASN.1 tree |

---

## 4. Viewer specs

### 4.1 HAR — enhance `ksitools-har-viewer.html`

**Who:** DevOps / support looking at a HAR from Chrome/Firefox/Charles without
uploading it to a SaaS waterfall.

**Keep:** JSON.parse of HAR 1.2-style `log.entries`; filter method/status/text;
column sort; detail tabs; 200 MB / 5k row caps; Copy JSON / Save JSON / Save CSV.

**Add (must):**

| Item | Notes |
|------|--------|
| Mask-on-by-default | `Authorization`, `Proxy-Authorization`, `Cookie`, `Set-Cookie`, `X-Api-Key`, `X-Auth-Token`, query `token`/`sig`/`password`. Reveal toggle in the header, same idea as WireGuard/OpenVPN. |
| Timing breakdown | Detail pane shows blocked / dns / connect / ssl / send / wait / receive, not only total ms bar. |
| Status class chips | Count 2xx / 3xx / 4xx / 5xx / blocked(0) in the meta line. |
| Copy as curl | Selected request → clipboard. Prefer a handoff to [`ksitools-curl-viewer.html`](../ksitools-curl-viewer.html) if sessionStorage `ksitools.tools.curl` already exists; otherwise copy a one-liner. Bodies truncated at 8 KB in the curl text with a notice. |
| HAR creator notice | Show `log.creator` / `log.browser` / page count (already parsed as `meta` — surface it). |

**Add (should):**

- Flag mixed-content (`http://` request inside an `https://` page URL).
- Notice when `content.text` is missing (“bodies not captured”).
- Filter by MIME class (xhr / js / css / img / other) from `content.mimeType` / `_resourceType`.

**Not done (say so in the UI):**

- Full Chrome DevTools waterfall layout (pixel-accurate).
- HAR 1.3 WebSocket frames as a first-class viewer.
- Replay / fetch the request (that would be a network call — forbidden).
- Cookie jar reconstruction.

**Fixtures (synthetic):** tiny HAR with one 200 JSON GET, one 401 with
`Authorization: Bearer demo-token` (must mask), one failed status 0.

---

### 4.2 Git objects / pack — new `ksitools-git-object-viewer.html`

**Who:** someone with a copied `.git` directory, a CI artifact, or a loose object
and no git binary.

**Accepts:**

| Input | Detect |
|-------|--------|
| Loose object | zlib header `78 01` / `78 9c` / `78 da`; inflate → `<type> <size>\0<payload>` |
| Pack | ASCII `PACK` @ 0, version 2/3, object count |
| Pack index | magic `\377tOc` (v2) or fanout-only v1 |
| Directory drop | optional: if the browser gives a folder, look for `objects/pack/*.pack` + matching `.idx` |

**Features (must):**

- **Commit / tag:** render header fields (tree, parent, author, committer, message). No signature verify.
- **Tree:** table of mode / type / sha / name. Click blob → preview.
- **Blob:** text preview if printable (≤256 KB); otherwise hex dump of the first 4 KB and a **jump** to the matching KSI viewer when the sniffer would (JSON, YAML, …) — or a “too big / binary” notice.
- **Pack index:** object count, first/last SHA prefix, offset list capped at `MAX_RENDER` (5k).
- **Pack:** version, object count. If idx is present (second drop or same-folder), list SHA + type **without** fully resolving every delta.

**Limits:**

- `MAX_BYTES` 128 MB for a pack/idx loaded for listing.
- Loose objects: inflate via `DecompressionStream('deflate')` (zlib wrapper). If that fails, error clearly (“not a git object / not zlib”).
- Delta resolution: **ofs-delta / ref-delta only for a user-selected object**, cap base chain at 16 and inflated size at 8 MB. If the chain is longer, notice “delta too deep — not expanded”.

**Not done:**

- `git clone` / network.
- Pack v3 extras, multi-pack index (`.midx`), cruft packs.
- Reconstruct a working tree to disk.
- Verify SHA-1 of every object on large packs (optional “hash this object” button using Web Crypto is fine).

**Exports:** Copy commit message; Save tree as `.csv`; Save selected blob.

**Fixtures:** build with `git hash-object` / `git cat-file` on a throwaway repo containing a one-line blob and a two-file tree. Do not commit the `.git` directory; keep generated fixtures under `/samples/` (already gitignored) or inline tiny hex in the self-check.

---

### 4.3 PE / ELF metadata — new `ksitools-pe-elf-viewer.html`

One viewer, format sniffed from magic. Same job: **read headers, never execute**.

**Accepts:** `.exe` `.dll` `.sys` `.so` `.o` `.elf` and extensionless binaries whose magic is `MZ` or `\x7fELF`. Mach-O is **out of scope** (sniffer already labels it; leave `viewer: null`).

**PE (must):**

- DOS stub + `e_lfanew` → `PE\0\0`.
- COFF: machine (map to x86 / x64 / ARM / ARM64 / …), section count, **TimeDateStamp** as UTC, characteristics (DLL vs EXE, stripped, large-address).
- Optional header: magic PE32/PE32+, subsystem, image base, OS/image versions.
- **DLL characteristics flags:** ASLR, DEP/NX, CFG, High-entropy VA, Dynamic base, No-SEH, Terminal server aware. Show present/absent, do not claim “secure”.
- Section table: name, vsize, raw size, characteristics (R/W/X). Flag RWX sections.
- Import directory: DLL names (cap 200). Optional first N import symbols (cap 500).
- Export directory: DLL name + export names (cap 500).
- Version info resource (`VS_VERSIONINFO`) if present: FileVersion, ProductName, CompanyName, OriginalFilename.

**ELF (must):**

- `e_ident`: class (32/64), endian, OSABI, version.
- Type (REL/EXEC/DYN/CORE), machine, entry.
- Program headers: type (LOAD/DYNAMIC/INTERP/NOTE/…), flags (R/W/X), offset/filesz.
- `PT_INTERP` string (dynamic linker).
- Dynamic `DT_NEEDED` library names.
- Section headers: name, type, flags, size. Cap 256 sections.
- Symbol tables (`SYMTAB` / `DYNSYM`) names, cap 1k, skip if stripped.

**Shared UX:**

- Summary chips at top (class, arch, type, PIE/DYN, ASLR/DEP).
- Tables + filter box.
- Hex of the first 256 bytes (confidence the magic is right).
- Flag “this looks packed” only as a **heuristic** (tiny import table + high entropy in `.text`) and label it as heuristic.

**Limits:** read at most 64 MB via `File.slice` for headers; do not map the whole file. Imports/exports/symbols beyond caps get a notice.

**Not done:**

- Disassembly, decompile, unpack, run.
- Authenticode / catalog signature **verify** (can show “certificate table present” + byte range only).
- .NET metadata streams (optional later).
- Relocations, overlay carving, malware scoring.

**Exports:** Copy summary JSON; Save `.json`; Save import list `.csv`.

**Privacy:** do not render overlay/string-table blobs that match `BEGIN .* PRIVATE KEY` / `AKIA` / `ghp_`. Truncate with “secret-like string omitted”.

**Fixtures:** a tiny `gcc -c` ELF `.o` and a `x86_64-w64-mingw32-gcc` hello `.exe` **you generate locally** under `/samples/`. Self-check can use a hand-written 64-byte ELF ident + dummy section header, not a real malware sample.

---

### 4.4 FAT / ISO browse — new `ksitools-disk-image-viewer.html`

**Who:** sysadmin with a USB/SD dump, a firmware ISO, or a FAT installer image who should not mount it.

**Accepts:**

| Kind | Detect |
|------|--------|
| FAT12 / FAT16 / FAT32 | Boot sector jump + `FAT1x`/`FAT32` OEM/fs type; BPB fields |
| ISO 9660 | `"CD001"` at offset `0x8001` (already in the sniffer) |
| Raw `.img` / `.ima` / `.dsk` | FAT heuristic if no ISO magic |
| `.iso` | ISO 9660 |

**Must:**

- Volume label, sector size, cluster size, FAT type, total size, free clusters (FAT).
- ISO: volume ID, publisher, creation date from PVD, block size (usually 2048).
- **Browsable directory tree** (left) + file table (right), archive-viewer look.
- Path filter.
- Click a file **≤256 KB** → text preview if printable, else hex (reuse hex dump style from the sniffer).
- **Sector I/O via `file.slice(start, end)`** — do not `FileReader` the whole image. Cap image size for *listing* at 8 GB as a File handle; cap bytes *read* per session with a running counter and a notice (e.g. 64 MB read budget).
- MBR: if bytes 510–511 are `55 AA`, show partition table (type, LBA, size) and let the user pick partition 1..n for FAT. ISO hybrid (MBR + ISO) : prefer ISO unless the user picks a FAT partition.

**Limits / honesty:**

- Directory entries capped at 20k files shown; notice if more.
- Recursion depth cap 32.
- FAT long-file-name (VFAT LFN) support is **must**.
- ISO Joliet (UCS-2 supplementary volume) is **should**; Rock Ridge is **not done**.
- Deleted FAT entries: show as a “deleted” badge if the first byte is `0xE5`, do not claim forensic recovery.

**Not done:**

- ext2/3/4, NTFS, exFAT, UDF, VHD/VMDK/QCOW **guest filesystems**. Sniffer may still name those files; this viewer errors with “not FAT/ISO — try a convert-to-raw in the hypervisor”.
- Write-back, format, mount.
- Extract-all to disk. Optional: download **one** file ≤8 MB as a blob (same as “preview + save”).
- Encrypted / BitLocker.

**Exports:** Copy current path list; Save listing `.csv` / `.json`.

**Fixtures:** a few-KB FAT12 floppy image created with `mkfs.vfat` and a 2-file ISO with `genisoimage`/`mkisofs`, both in `/samples/`.

---

### 4.5 Package guts — new `ksitools-package-viewer.html`

**Who:** DBA/DevOps/sysadmin handed a `.deb`, `.rpm`, or `.jar` and needing
**name, version, dependencies, and file list** without installing.

**Accepts:** `.deb`, `.rpm`, `.jar` (also `.war` / `.ear` as JAR-shaped ZIP).

Treat each dialect as a tab after sniff:

#### DEB (Unix `ar`)

Members: `debian-binary`, `control.tar.gz|xz|zst`, `data.tar.gz|xz|zst`.

- Parse `ar` global header `!<arch>\n` and 60-byte member headers.
- Inflate control tarball (`gzip` / `xz` via `DecompressionStream` if the browser supports `'xz'`; otherwise notice “xz/zstd control not inflated in this browser”).
- Show `control` fields: Package, Version, Architecture, Depends, Recommends, Conflicts, Maintainer, Description, Installed-Size.
- Data tarball: **file list only** (path, size, mode), cap 20k entries. Click small text member to preview.

#### RPM

- Lead: magic `ED AB EE DB`, major/minor, type, archnum, name (66 bytes), osnum, signature type.
- Signature + header: RPM header magic `8e ad e8 01`, index + store. Tags as a table.
- Must-show tags: NAME, VERSION, RELEASE, EPOCH, ARCH, SUMMARY, LICENSE, VENDOR, URL, OS, PAYLOADFORMAT, PAYLOADCOMPRESSOR, REQUIRENAME/REQUIREVERSION, PROVIDENAME, SIZE, BUILDTIME.
- Payload: if `cpio` + gzip/xz and size ≤ `MAX_PAYLOAD_LIST` (e.g. 64 MB inflated listing budget), list files. Else stop at header tags and say so.

#### JAR / WAR / EAR

- Reuse archive ZIP central-directory listing (copy the functions; do not iframe).
- Extra guts:
  - `META-INF/MANIFEST.MF` parsed (Main-Class, Implementation-Version, Automatic-Module-Name).
  - `META-INF/maven/**/pom.properties` if present (groupId / artifactId / version).
  - Count of `.class` vs resources; package prefix histogram (top 20).
  - Flag: signed (`META-INF/*.SF` present) — **do not verify** the signature.
  - Flag: nested jars (`*.jar` members).

**Limits:** 256 MB package file handle; 512 MB archive-style listing budget; member preview ≤256 KB.

**Not done:**

- Install, `dpkg -i`, `rpm --install`.
- GPG/RSA package signature verify.
- `zstd` payloads if `DecompressionStream` has no `'zstd'` (detect + honest error).
- Reconstruct a full Maven POM (that is [`ksitools-maven-pom-viewer.html`](../ksitools-maven-pom-viewer.html) if they extract `pom.xml`).
- AppImage / Snap / Flatpak / MSI / APK (later; sniffer already names some).

**Exports:** Copy control/RPM tags JSON; Save file list `.csv`; Save manifest text.

**Sniffer change:** point `rpm`, `deb` (and jar if you want the guts page instead of generic archive) at this viewer. Leave `.zip` on the archive viewer. ISO leaves this file and goes to the disk-image viewer.

**Fixtures:** `jar cf` a two-class empty project; a `dpkg-deb` of a dummy package with only a `control` file; an `rpmbuild` noarch dummy **or** a hand-rolled lead+header if rpmbuild is not available. No third-party vendor RPMs.

---

### 4.6 Terraform plan JSON — enhance `ksitools-terraform-plan-viewer.html`

**Who:** reviewer looking at `terraform show -json plan.out` (or the same document from OpenTofu / Terraform Cloud) who still has to open a second tool for outputs and replace reasons.

**Keep:** `resource_changes` table, action chips, before/after detail, mask-on-by-default, 200 MB / 8k rows, `data.plan` wrapper.

**Add (must):**

| Item | Notes |
|------|--------|
| Output changes | Table of `output_changes` (name, action, before → after). Mask `sensitive` values the same way as resource attrs. |
| Replace reasons | Detail pane lists `change.replace_paths` (and `change.replace_triggered_by` if present). |
| Drift vs plan | If `resource_drift` exists, a chip + filter. Do not merge drift rows into planned changes without a badge. |
| Variables | Collapsed “variables” panel from `variables` / `configuration.root_module.variables`. **Mask by default** — plans often embed tfvars. |
| Applyable / errored | Surface `applyable`, `errored`, `timestamp`, `format_version` in the meta line (already parsed as `meta` for version — add the rest). |

**Add (should):**

- TFC/TFE wrap: if the top-level object has `.plan` **or** `.data.attributes.plan` / `structured-output`, unwrap and say so in the notice (“Terraform Cloud run JSON”).
- `relevant_attributes` is ignored (too noisy). `configuration` is **not** rendered by default — it is huge and repeats secrets; optional “include raw configuration JSON” export is enough.
- OpenTofu: same JSON schema (`tofu show -json`). Title can stay Terraform; notice “OpenTofu plan” if `terraform_version` looks like tofu or a `x-opentofu` key appears.

**Not done (say so in the UI):**

- Binary `.tfplan` (gob / protobuf). Error: “binary plan — run `terraform show -json plan.out > plan.json` and drop that”.
- Applying, refreshing, or talking to a backend.
- Evaluating `unknown` values.

**Fixtures:** synthetic `show -json` with one create, one replace (`replace_paths: [["ami"]]`), one sensitive output, one `resource_drift` row. No customer addresses.

---

### 4.7 Terraform state — enhance `ksitools-terraform-state-viewer.html`

**Who:** someone comparing a backup `terraform.tfstate` to current, or opening `terraform output -json` without a full state file.

**Keep:** resource inventory, outputs card, secret heuristics, 30 MB / 5k rows, paste box.

**Add (must):**

| Item | Notes |
|------|--------|
| Two-file diff | Second drop (or “Compare…”). Join on resource instance **address**. Chips: only-in-A, only-in-B, attributes-changed, unchanged. Attribute diff capped at 200 keys per instance. |
| Lineage / serial | If `lineage` differs → **error-level** banner (“not the same state history”). If lineage matches and B.serial < A.serial → warn “B is older”. |
| `terraform output -json` | A file that is `{ "name": { "value", "type", "sensitive" } }` with **no** `resources` / `version` still loads, outputs-only mode. Mask `sensitive: true`. |

**Add (should):**

- Filter `mode=data` vs `managed`.
- Module rollup (count of instances per `module.` prefix).
- Provider histogram.

**Not done:**

- `terraform state pull` / S3 / TFC HTTP.
- Push / `state mv` / `state rm`.
- Decrypting or unlocking encrypted backends.

**Privacy:** comparing two states doubles secret surface. Mask stays on; “Save .csv” of a diff must redact the same keys as the single-file view.

**Fixtures:** two tiny v4 states, same lineage, serial 3 vs 4, one resource added and one attribute changed; a third file that is only `output -json`.

---

### 4.8 HCL outline — light touch on `ksitools-hcl-viewer.html`

Stay a **single-file block outline**. The multi-file / graph work belongs in §4.11–4.12.

**Add (must):**

- Accept `.tf.json` (JSON syntax): object keys `resource` / `data` / `module` / `variable` / `output` / `provider` / `terraform` → same block table.
- Accept `.tofu` / `.tofu.json` (OpenTofu).
- If the filename is `.terraform.lock.hcl` **or** the first non-comment line contains `This file is maintained automatically by "terraform init"` → do not pretend hashes are provider blocks. Notice + link to [`ksitools-terraform-lock-viewer.html`](../ksitools-terraform-lock-viewer.html) (after that page exists). Until then, still parse but label kind `lock-provider` so hashes are not “providers”.

**Not done:** HCL interpreter, `dynamic` expansion, `terraform console`.

---

### 4.9 Terraform lock — new `ksitools-terraform-lock-viewer.html`

**Who:** every `terraform init` and every provider-bump PR. “What did we pin, for which platforms, and are the hashes complete?”

**Accepts:** `.terraform.lock.hcl` (and OpenTofu’s same file). Paste box. Optional second file: a `versions.tf` / `required_providers` snippet for constraint comparison.

Do **not** add this dialect to `ksitools-lockfile-viewer.html`.

**Detect:** filename, or HCL `provider "registry.terraform.io/…"` blocks whose body has `hashes = [` / `version =`.

**Must:**

- Table: provider address (registry host / namespace / type), **version**, **constraints**, hash count, hash kinds (`zh` / `h1` / `zh:…`).
- Detail: full hash list (capped 64 shown, notice if more).
- Flags: no hashes; only `h1` (no `zh`); empty constraints; duplicate provider addresses; version does not satisfy constraints (semver `~>` / `>=` / `=` only — if the constraint is exotic, skip the check and say so).
- Optional overlay: `required_providers` from a second drop → “declared vs locked” (missing lock entry, locked but undeclared).

**Limits:** 5 MB is plenty; these files are small. `MAX_RENDER` 500 providers.

**Not done:**

- Downloading providers, verifying hashes against the registry, or running `terraform providers lock`.
- Signing / GPG of providers.
- The language lockfile formats (npm, Go, …).

**Exports:** Copy summary; Save `.csv` (address, version, constraints, hash_count); Save `.json`.

**Fixtures:** a two-provider lock (aws + random) with mixed `zh`/`h1`; a broken copy with empty `hashes = []`.

---

### 4.10 Terraform CLI log — new `ksitools-terraform-log-viewer.html`

**Who:** the person who got a plan/apply pasted into Slack, Jira, or a CI log — **not** `terraform show -json`. This is the most common Terraform artifact after state, and we currently bounce them to the generic log viewer.

**Accepts:**

| Input | Detect |
|-------|--------|
| Human plan / apply / destroy | Lines matching `Plan:` / `Apply complete!` / `Error:` / leading `+ `/`~ `/`- `/`-/+ `/`<= ` |
| Machine UI JSONL | `terraform plan -json` / `apply -json` — each line a JSON object with `"type"` (`version`, `planned_change`, `change_summary`, `diagnostic`, `apply_start`, `apply_complete`, `resource_drift`, `outputs`, …) |
| `terraform validate -json` | Object with `valid` + `diagnostics` (not JSONL) |
| Paste | Same parsers |

If the file is a single `show -json` object (`resource_changes`), **do not** steal it — notice and point at the plan viewer.

**Must:**

- Summary chips: to-add / to-change / to-destroy / to-replace; error count; warning count. From `Plan: N to add, M to change, K to destroy` **or** `change_summary` JSONL **or** `Apply complete! Resources: …`.
- Resource table: address, action, (optional) duration. From JSONL `planned_change` / `apply_*`, or from human blocks (`# aws_instance.foo will be created` / `# … must be replaced`).
- Diagnostics table: severity, summary, address, snippet. `Error:` / `Warning:` human blocks **or** JSONL / validate `diagnostics`.
- Filter: action, errors-only, text search.
- Mask secret-like values in diagnostic snippets (AKIA, PEM, `password =`).

**Limits:** 50 MB text; `MAX_RENDER` 8k resource rows / 2k diagnostics. Human parser is **best-effort** — if a custom wrapper stripped the `Plan:` line, still list `+ ` / `- ` addresses and say “summary line not found”.

**Not done:**

- `TF_LOG=TRACE` plugin RPC dumps (noise; keep on the generic log viewer).
- Re-running the plan.
- Colour TTY reconstruction as a screenshot.

**Exports:** Copy summary; Save resource `.csv`; Save diagnostics `.json`.

**Fixtures:** a 20-line human plan with one create + one error; a 5-line JSONL `plan -json` with `change_summary` + `planned_change` + `diagnostic`; a `validate -json` with `valid: false`.

---

### 4.11 Terraform module + vars — new `ksitools-terraform-module-viewer.html`

**Who:** code review of a root module **without** running `terraform init`. “What does this directory actually declare?”

**Accepts:**

- Multiple `.tf` / `.tofu` / `.tf.json` (multi-select or directory drop via `webkitRelativePath` — optional, with a “this browser gave us a folder” path).
- `.terraform/modules/modules.json` from a local init (module Key / Source / Dir / Version).
- Optional **second drop**: `*.tfvars` / `*.auto.tfvars.json` / `terraform.tfvars.json` overlay.

Reuse the HCL outline parser (copy functions; do not iframe). Attribute harvest is a **line scan**, not an interpreter: `source\s*=\s*"…"`, `version\s*=\s*"…"`, `backend\s*=`, `required_version`, `sensitive\s*=\s*true`.

**Must:**

| Panel | Content |
|-------|---------|
| Terraform | `required_version`; backend type + key fields (`bucket`/`key`/`dynamodb_table` or `hostname`/`organization`/`workspaces`) — **mask tokens** (`token`, `access_key`, `secret_key`, `password`) |
| Providers | `required_providers`: name, source, version constraint. Flag missing `source`. |
| Modules | `module` blocks: name, source, version, `count`/`for_each` present. Flags: no version on a registry source; `source = "../…"` local; `git::` / `https://` without a ref; `source` looks like a gist or gist-raw URL |
| Resources / data | Counts by type (not every instance). Click type → list of names + file:line |
| Variables | name, type (string if parsed), has default, `sensitive`, `nullable`, description (first line) |
| Outputs | name, `sensitive`, description |
| Moves / imports | `moved` (from → to), `import` (to, id masked if it looks like a secret), `removed` |
| tfvars overlay | declared-and-set / declared-missing-no-default / tfvars-key-not-declared. Mask sensitive variable **values** |

**Limits:** 25 MB **total** across dropped `.tf` files; 400 files; 8k blocks. Directory drop ignores `.terraform/` except `modules/modules.json`, ignores `*.tfstate`, ignores `crash.log`.

**Not done:**

- `terraform init` / module download / provider install.
- Expanding `dynamic`, `for_each`, or interpolations.
- Evaluating locals.
- Writing a `backend.hcl` for them.

**Exports:** Copy inventory JSON; Save module-call `.csv`; Save variable `.csv` (values redacted when sensitive).

**Fixtures:** a 3-file root (`versions.tf`, `main.tf` with one module + one resource, `variables.tf` + `outputs.tf`) plus a `terraform.tfvars` that sets one var and one unknown key. No real AWS account IDs.

---

### 4.12 Terraform graph — new `ksitools-terraform-graph-viewer.html`

**Who:** lost in a large root module. They already ran `terraform graph > graph.dot` (or `-type=plan`).

**Accepts:** Graphviz DOT from `terraform graph` / `tofu graph`. Paste box.

**Detect:** `^digraph` plus at least one node that looks like a Terraform address (`aws_instance.foo`, `module.vpc`, `provider[…]`, `[root]`).

**Must:**

- Node table: id/label, kind heuristic (resource / data / module / provider / output / var / root).
- Edge table: from → to.
- Filter by kind / substring.
- Degree chips: roots (in-degree 0, excluding `[root]`), leaves, high out-degree (possible “god” resources) — top 20.
- Honest notice: “layout is a table, not `dot -Tpng`”. **No CDN graph library, no vis.js, no d3.**

**Should:** a tiny pure-JS indented tree from `[root]` (cap 2k nodes) — collapse modules. Skip if the graph is cyclic-looking or >2k nodes.

**Limits:** 20 MB DOT; 10k nodes / 20k edges drawn. Above that, summary + “file too bushy”.

**Not done:**

- Running `terraform graph`.
- Pretty force-directed SVG.
- Importing generic Graphviz from unrelated tools (if there are no Terraform-looking addresses, error and point at a future generic DOT page — do not build that here).

**Exports:** Save nodes `.csv`; Save edges `.csv`.

**Fixtures:** DOT for `root → provider.aws → aws_instance.web` plus one module hop. Generate with `terraform graph` on the §4.11 fixture if terraform is installed; otherwise hand-write 8 lines.

---

### 4.13 Terraform cost estimate — new `ksitools-terraform-cost-viewer.html`

**Who:** PR review that already has an Infracost (or compatible) **breakdown JSON** sitting in CI artifacts. Daily cost-aware Terraform; still a file drop, not a billing API.

Display name **KSI Tools Terraform Cost Viewer**. The file format is the de-facto Infracost `breakdown --format json` / `output --format json` v0.2 document. Do not fetch prices.

**Accepts:** JSON with `projects[]` and at least one of `totalMonthlyCost`, `totalHourlyCost`, `diffTotalMonthlyCost`, `currency`.

**Must:**

- Header: currency, monthly / hourly / monthly-diff, project count.
- Resource table: project, address/name, type, monthly, hourly, diff, usage (if present).
- Filter by project / type / “has diff”.
- Flag `monthlyCost == null` as **uncosted** (not “free”).
- Mask nothing cost-related; still mask any nested env/token fields if they appear.

**Limits:** 50 MB; 8k resource rows.

**Not done:**

- Calling Infracost Cloud, AWS Cost Explorer, or any network price list.
- Re-running a plan to produce the JSON.
- FinOps dashboards.

**Exports:** Copy totals; Save resource `.csv`; Save `.json`.

**Fixtures:** a 2-resource synthetic v0.2 document (`totalMonthlyCost: "12.34"`, one uncosted data source). No real account IDs.

---

### 4.14 IaC scan JSON — enhance `ksitools-sarif-viewer.html`

Checkov / tfsec / Terrascan results are what Terraform people attach to PRs. SARIF already covers the interchange form and Trivy/Grype.

**Add (must):** sniff and flatten:

| Tool | Shape |
|------|--------|
| Checkov | `results.failed_checks` (+ optional `passed_checks` behind a toggle) → severity, `check_id`, `check_name`, file, resource/`resource_address` |
| tfsec | `results[]` → `rule_id`, `severity`, `location.filename` + `start_line`, `description` |
| Terrascan | `results.violations[]` → `rule_id`, `severity`, `file`, `resource_type` / `resource_name` |

Add a **resource address** column when the tool provides one (Checkov `resource`, tfsec `location.resource`, Terrascan `resource_name`). Filter by severity / tool / rule.

**Not done:** running the scanners; auto-fix; Sentinel / OPA evaluation (that stays on the policy / Rego viewers).

**Fixtures:** one failed Checkov check on `aws_s3_bucket.b`, one tfsec `results[]` row. Synthetic IDs only.

---

### 4.15 SQL EXPLAIN — new `ksitools-sql-explain-viewer.html`

**Who:** DBA / on-call with an `EXPLAIN` paste from `psql`, `mysql`, `sqlplus`,
or a ticket. Public tools (`explain.dalibo.com`, `explain.depesz.com`) are the
leak surface this page replaces.

**Keep as-is (do not steal their files):** SQL dump, pg slow-log, MySQL
slow/binlog, Oracle AWR, SQLite (its `EXPLAIN` is a query you run *inside*
that DB). This page is the *plan text/JSON*, not the workload log.

**Accepts:**

| Input | Detect |
|-------|--------|
| PostgreSQL text tree | Leading whitespace + `Seq Scan` / `Index Scan` / `Nested Loop` / `Hash Join` / `QUERY PLAN` |
| PostgreSQL `EXPLAIN (FORMAT JSON)` | JSON object (or `[{QUERY PLAN: …}]`) with a `Plan` key |
| MySQL tabular `EXPLAIN` | Header `id` / `select_type` / `table` / `type` / `possible_keys` / `rows` / `Extra` |
| MySQL `EXPLAIN FORMAT=JSON` | JSON with `query_block` |
| Oracle `DBMS_XPLAN.DISPLAY` | ASCII box whose header includes `Id` and `Operation` (often `\| Id \| Operation \|`) |
| Paste | Same parsers |

If two parsers both match, show a **format selector** rather than guessing
wrong. One combined page — do not ship three EXPLAIN files.

**Must:**

- Node table / tree: operation, relation/index, cost, estimated rows, actual
  rows / actual time when present.
- Actual vs estimated ratio when both exist; flag `>10×` or `<0.1×`.
- Slowest nodes by actual time (top 10). pg shared-hit / read buffers if present.
- Flags (heuristic, labelled as such): pg `Seq Scan`; MySQL `type=ALL` /
  `Using filesort` / `Using temporary`; Oracle `TABLE ACCESS FULL`.
- Summary chips: dialect, node count, “has actuals” vs estimate-only.

**Limits:** 20 MB text; `MAX_RENDER` 2k nodes. Plans are small; refuse loudly
above that rather than streaming.

**Not done (say so in the UI):**

- Connecting to a database or running `EXPLAIN`.
- Rewriting SQL / creating indexes (flags are hints, not a tuner).
- SQL Server showplan, SQLite `EXPLAIN QUERY PLAN` as a first-class dialect
  (SQLite people already have the DB viewer).
- Cost units unification across vendors (pg cost ≠ Oracle cost). Show native
  numbers.

**Exports:** Copy summary JSON; Save node `.csv`.

**Fixtures:** tiny pg JSON with one `Seq Scan` and a 100× row miss; tiny MySQL
`query_block` with `access_type: ALL`; tiny Oracle box with `TABLE ACCESS FULL`.
No real schema names from a customer DB.

---

### 4.16 JVM crash log — new `ksitools-jvm-crash-viewer.html`

**Who:** someone with `hs_err_pid<PID>.log` after a JVM death. Sibling of the
four JVM pages we already ship (thread dump, GC log, heap dump, JFR) — not a
substitute for any of them.

**Accepts:** `hs_err_pid*.log` / `.txt`; paste; text whose first 2 KB contain
`A fatal error has been detected by the Java Runtime Environment`.

If the file is a HotSpot `jstack` / javacore, **do not** steal it — notice and
point at the thread-dump viewer.

**Must:**

| Panel | Content |
|-------|---------|
| Signal | Exception / signal name, PC, pid, tid, compiled vs interpreter |
| Thread | Current thread name + stack, cap 200 frames |
| Heap | Heap summary at crash (generation sizes if present) |
| VM args | Command line + selected system properties. **Mask-on-by-default** `user=`, `password=`, JDBC URLs, `*.secret`, `*.key`. Reveal toggle, same idea as javacore ENVINFO |
| OS | OS / CPU / memory lines |
| Classification | Chips: `SIGSEGV` / `SIGBUS` / `EXCEPTION_ACCESS_VIOLATION` / `OutOfMemoryError` / `Internal Error` — do not claim a root cause beyond the header |

Also surface JVM version and GC name when the header has them.

**Limits:** `MEMORY_MAX_BYTES` 64 MB; `ABSURD_MAX_BYTES` 512 MB (crash logs are
smaller than GC logs; 50–200 MB happens). Streaming line scan in lazy mode.
`MAX_RENDER` 400 frames / 2k loaded-library rows.

**Not done:**

- Native disassembly, gdb, core-dump walk.
- Reproducing the crash or talking to a running JVM.
- IBM javacore (that is the thread-dump viewer). PHD (heap-dump viewer).

**Exports:** Copy summary; Save thread stack `.txt`; Save `.json` of parsed
sections (args redacted when mask is on).

**Fixtures:** a 40-line synthetic `hs_err` with `SIGSEGV` and a 6-frame Java
stack; a second with `OutOfMemoryError`; a third with
`-Djavax.net.ssl.keyStorePassword=demo-secret` in VM args (must mask).

---

### 4.17 AWS ALB access log — new `ksitools-alb-log-viewer.html`

**Who:** someone who pulled ALB (or NLB-in-ALB-format) access logs from S3.
This is an HTTP request log, **not** CloudTrail.

**Keep:** CloudTrail / cloud-audit for API calls; generic log viewer for
combined-log-format / `solr.log` / everything else.

**Accepts:** space-delimited text, quoted `request` field. Typical type token
`http` / `https` / `h2` / `ws` / `wss` / `grpc`. Optional header row. Paste.

v2 is 34 fields (type, time, elb, client:port, target:port, processing times,
elb_status, target_status, bytes, request, user_agent, ssl_*, target_group_arn,
trace_id, domain_name, chosen_cert_arn, matched_rule_priority,
request_creation_time, actions_executed, redirect_url, error_reason,
target:port_list, target_status_code_list, classification,
classification_reason). Extra trailing fields = keep as `unknown_N`, do not
fail the row.

If the file is CloudTrail JSON or a `show -json` plan, do not steal it.

**Must:**

- Table: time, client, target, elb_status, target_status, method, host/path,
  `target_processing_time`, `error_reason`, target group (short).
- Chips: 2xx / 3xx / 4xx / 5xx counts; error_reason histogram; target-group
  counts; 5 slowest by `target_processing_time`.
- Filter: status class, error_reason, text search.
- Mask query `token` / `sig` / `password` / `Signature` in the request URL
  (same list idea as HAR). User-Agent shown; cookies are not an ALB field.

**Limits:** lazy slice (house idiom). `MEMORY_MAX_BYTES` 64 MB;
`ABSURD_MAX_BYTES` 8 GB; `MAX_RENDER` 8k rows. Status / error_reason counts
cover **every** line even past the render cap; the table / CSV use the sample
and the UI says so.

**Not done:** Athena, live ALB APIs, WAF body, reconstructing a HAR.

**Exports:** Copy summary; Save row `.csv`; Save `.json` of the histogram.

**Fixtures:** 5 synthetic v2 lines — one 200, one 502 with `error_reason`, one
with `?token=demo-token` (must mask). No real account IDs in ARNs — use
`arn:aws:elasticloadbalancing:us-east-1:000000000000:…`.

---

### 4.18 AWS VPC Flow Log — new `ksitools-vpc-flow-viewer.html`

**Who:** network / security triage of VPC Flow Logs (S3 / CloudWatch export as
text). Distinct from ALB (no HTTP URL) and from CloudTrail (no API event).

**Accepts:** space-delimited. Optional header
`version account-id interface-id …`. Auto-detect version from field count or
header:

| Version | Fields (baseline) |
|---------|-------------------|
| v2 | 14: version, account-id, interface-id, srcaddr, dstaddr, srcport, dstport, protocol, packets, bytes, start, end, action, log-status |
| v3+ | + vpc-id, subnet-id, instance-id, tcp-flags, type, pkt-srcaddr, pkt-dstaddr |
| v4+ | + region, az-id, sublocation-type, sublocation-id |
| v5+ | + pkt-src-aws-service, pkt-dst-aws-service, flow-direction, traffic-path |

**Must:**

- Table of flows with the fields present in that version.
- Chips: ACCEPT / REJECT counts; protocol histogram (tcp / udp / icmp / other);
  top 20 talkers (src/dst IP pair by bytes).
- Flag RFC-1918 vs public on src and dst (and pkt-src / pkt-dst when present).
- Filter: action, protocol, IP / interface substring.

**Limits:** same lazy idiom as ALB (64 MB memory / 8 GB refuse / 8k rows).
Aggregates over the whole file; table is the sample.

**Not done:**

- Geo-IP (network or a huge DB — forbidden).
- VPC APIs, packet payloads, DNS resolution.
- Resolving `account-id` to a name.

**Privacy:** show `account-id` (ops need it to know whose log this is). Do not
fetch anything with it. Synthetic fixtures use `000000000000`.

**Exports:** Copy summary; Save flow `.csv`; Save talker `.csv`.

**Fixtures:** 4 v2 lines (ACCEPT + REJECT, private + public dst); one v5 line
with `flow-direction`.

---

### 4.19 pprof + folded stacks — new `ksitools-pprof-viewer.html`

**Who:** someone with a Go `pprof` file, Java async-profiler output, or
`perf script` collapsed stacks. JFR stays on the JFR page (inventory, not
samples). Proto schema stays on the proto viewer.

Display name **KSI Tools pprof Viewer**. Accepts folded stacks too so we do
not ship a second “flame” file.

**Accepts:**

| Input | Detect |
|-------|--------|
| pprof `profile.proto` | gzip (`1f 8b`) or raw protobuf with string/function/location/sample tables (google/pprof) |
| Folded / collapsed stacks | Lines `func1;func2;func3 42` or `func1;func2;func3<TAB>42` (Brendan Gregg) |
| `.pb.gz` / `cpu.pprof` / `heap.pprof` / `.collapsed` | Filename bump |

**Must:**

- Summary: sample type (`cpu` / `heap` / `alloc` / unknown), sample count,
  period / period type when the proto has them; line count for folded input.
- Function table: name, self, cumulative, % of total. Cap `MAX_RENDER`.
- Stack table: folded form + count. Filter by function substring.
- Honest notice: **layout is a table, not a flame-graph SVG**. No d3, no
  vis.js, no CDN. Same honesty as the Terraform graph viewer.

**Should:** a tiny indented tree from roots, cap 2k nodes. Skip if the profile
is too bushy.

**Limits:** 64 MB **inflated**; gzip via `DecompressionStream('gzip')`; 10k
unique stacks drawn. Hard-refuse a gzip that inflates past 64 MB.

**Not done:**

- Live `http://*:6060/debug/pprof` fetch.
- Symbolizing from a binary / `perf` map.
- Pretty flame SVG / interactive icicle.
- OpenTelemetry Profiles protobuf as a first-class schema (pprof conversion
  is enough until someone files a native OTel profile).

**Exports:** Save function `.csv`; Save folded `.txt` (always producible from
either input).

**Fixtures:** a 6-line folded file with one hot leaf; optional hand-rolled tiny
pprof protobuf in the self-check (no production profile). Do not check in a
real customer `heap.pprof`.

---

### 4.20 Log viewer — enhance `ksitools-log-viewer.html`

**Who:** everyone who currently hits “file too large (100 MB)” on an unrotated
`solr.log`, nginx log, or CI log. The hand-off already chose **not** to add a
dedicated `solr.log` page; this is the follow-up that makes that choice hold.

**Keep:** level detection (`ERROR`/`WARN`/`INFO`/`DEBUG` and synonyms),
timestamp tint, level filter, text search, wrap, line numbers, 50k drawn
rows, Copy / Save `.log` / Save `.html`. Solr’s
`2026-08-21 09:14:02.123 INFO (qtp…)` shape already matches — do not break it.

**Add (must):**

| Item | Notes |
|------|--------|
| Lazy slice | House idiom. `MEMORY_MAX_BYTES` 64 MB (`readAsText` path as today). Above that, 8 MB slices like the GC log. `ABSURD_MAX_BYTES` 8 GB. |
| Visible mode | Memory vs lazy chip. Progress bar in lazy mode. |
| Honest truncation | Level **counts** cover every line even past `MAX_RENDER` (50k). The table / search / HTML export use the retained sample, and the notice says so. |
| Raise the old refuse | Today’s 100 MB hard refuse goes away; 8 GB is the new ceiling. |

**Add (should):** after ALB / VPC / crash-log pages exist, if the first lines
look like those formats, a notice + link — do not silently re-parse as a
generic log.

**Not done:** turning the generic log viewer into ALB/VPC/GC. Those stay
specialized. No `TF_LOG=TRACE` pretty-printer (already out of scope on the
Terraform log page).

**Fixtures:** a 30-line mixed-level log (existing behaviour must not regress);
self-check that a slice-boundary split of a line still yields one logical
line. A >64 MB fixture is **not** required in-repo; document the manual
check (`python3 -c` a 70 MB file of repeated lines) in the PR.

---

### 4.21 ASN.1 EMV overlay — enhance `ksitools-asn1-viewer.html`

**Who:** someone pasting a smartcard / EMV BER-TLV blob instead of a
certificate. `emvlab.org` is the public paste target; PAN-adjacent, so this
is a PCI problem if it leaves the machine.

**Keep:** generic DER/BER tree, OID names, PEM/DER/PKCS, copy hex / Base64 /
subtree. This is a **tag-dictionary layer**, not a second viewer.

**Add (must):**

- When a tag is in a small baked-in EMV / ISO 7816 set, show the dictionary
  name next to the tag number (`5A` → Application PAN, `50` → Application
  Label, `84` → DF Name / AID, `5F24` → Application Expiry, plus a short
  list of common `9Fxx` / `8C` / `8D` / `77` / `70`). Unknown tags stay
  numeric — do not invent names.
- Tag `5A` (PAN): **mask on by default** — first 6 + last 4, middle omitted,
  same reveal toggle pattern as HAR. Never put the full PAN in Copy / CSV
  while mask is on.
- Notice in the UI: “EMV names are a dictionary overlay, not a transaction
  validator. No cryptogram verify, no CDA/DDA.”

**Limits:** unchanged from the current ASN.1 page.

**Not done:**

- Full EMV kernel, offline data authentication, contactless replay.
- A dedicated `ksitools-emv-viewer.html`.
- PCI DSS as a product claim — we mask the PAN and stay local; that is the
  whole control.

**Fixtures:** a tiny synthetic BER constructed in the self-check with tag `50`
= `TESTAPP` and tag `5A` = 16 digits that are **not** a published issuer PAN
(e.g. `0000000000000000` is too obvious — use `9999999999999999` and assert
the masked form). No real card dumps.

---

## 5. Shared implementation notes

### 5.1 Inflate

Use the browser:

| Encoding | API |
|----------|-----|
| gzip | `DecompressionStream('gzip')` |
| zlib (git loose) | `DecompressionStream('deflate')` |
| raw deflate (ZIP) | `DecompressionStream('deflate-raw')` |
| xz | `DecompressionStream('xz')` if present, else notice |

Wrap in `blob.stream().pipeThrough(...)`. Catch and show “this browser cannot inflate X”.

pprof `.pb.gz` is gzip-of-protobuf — inflate first, then walk the proto. Folded
stacks are plain text (no inflate).

### 5.1b Large-file I/O (copy, don't invent)

Canonical implementations: **GC log** (text, sequential slices) and **SQLite /
heap dump / JFR** (binary, `File.slice` by offset). New GB-scale pages
(disk image, ALB, VPC Flow, generic log) copy one of those two, including
the visible mode chip and the “counts are complete, table is a sample”
wording.

| Constant | Role |
|----------|------|
| `MEMORY_MAX_BYTES` | Whole-file `readAsText` / `arrayBuffer` is allowed at or below this (64 MB unless the spec says otherwise). |
| `ABSURD_MAX_BYTES` | Hard refuse. Tune per format (GC log 8 GB, heap dump 64 GB, crash log 512 MB, EXPLAIN 20 MB). |
| `SLICE_BYTES` | Lazy window, typically 8 MB. Overlap or carry a remainder so a record cannot vanish on a boundary. |
| `MAX_RENDER` | DOM rows. Independent of how many records you *counted*. |

Progress callback at least every slice. A 2 GB scan that paints nothing for
minutes is a failed ship even if it eventually finishes.

**Generated JS hygiene:** when a self-check or fixture embeds unusual bytes,
escape C0 (`U+0001`–`U+001F`), C1, U+2028, U+2029, U+FEFF — do not write them
literally into the HTML `<script>` (that class of bug shipped twice in the
JVM / beautifier work). The existing GC-log sweep is the version to copy.

### 5.2 Do not run untrusted code

- PE/ELF: no WebAssembly instantiate of the dropped file, no `Function` on extracted JS from a JAR.
- JAR: do not eval.
- ISO/FAT: do not auto-open HTML members as documents (`textContent` / download only).
- Terraform: no `eval` of interpolations, no spawning `terraform` / `tofu`, no registry or backend HTTP.
- SQL EXPLAIN: no connecting to Postgres/MySQL/Oracle, no running the statement.
- pprof: no fetch of `/debug/pprof`, no WASM instantiate of the profile.
- ALB / VPC / crash log: no AWS / IMDS / JDBC calls. Parse the file you were given.

### 5.3 Jump table (sniffer)

After ship, `SIGS` in `ksitools-filetype-viewer.html`:

| `id` | `viewer` becomes |
|------|------------------|
| `pe` | `ksitools-pe-elf-viewer.html` |
| `elf` | `ksitools-pe-elf-viewer.html` |
| `iso` | `ksitools-disk-image-viewer.html` |
| `rpm` | `ksitools-package-viewer.html` |
| `deb` | `ksitools-package-viewer.html` |
| zip-as-jar (ext `.jar`/`.war`/`.ear`) | `ksitools-package-viewer.html` (keep generic `.zip` on archive) |

Add git signatures:

- `git-pack`: ASCII `PACK` @ 0 **and** version uint32 2 or 3 at offset 4.
- `git-idx`: `\377tOc`.
- Loose objects are zlib + path `objects/[0-9a-f]{2}/[0-9a-f]{38}` — weak on magic alone; bump score when the name looks like a git object path.

Terraform (name + light sniff, after those pages exist):

| `id` | `viewer` becomes |
|------|------------------|
| `tf-lock` | `ksitools-terraform-lock-viewer.html` — filename `.terraform.lock.hcl` |
| `tfstate` | `ksitools-terraform-state-viewer.html` — ext `.tfstate` or JSON with `lineage` + `resources` + `serial` |
| `tf-plan-json` | `ksitools-terraform-plan-viewer.html` — JSON with `resource_changes` / `planned_values` |
| `tf-plan-jsonl` | `ksitools-terraform-log-viewer.html` — first line JSON `"type":"version"` / `"type":"planned_change"` |
| `tf-graph` | `ksitools-terraform-graph-viewer.html` — `digraph` + Terraform-looking addresses |
| `tf-cost` | `ksitools-terraform-cost-viewer.html` — JSON `projects` + `totalMonthlyCost` |
| `tf` / `tf.json` / `tofu` | keep HCL viewer for a **single** file; module viewer is a hub card, not a sniffer default (sniffer sees one file) |

Binary `.tfplan` stays `viewer: null` with label “Terraform plan (binary) — convert with terraform show -json”.

Ops paste-targets (name + light sniff, after those pages exist). **Generic
`.log` stays on the log viewer** unless the signature is strong — do not
steal nginx/solr/CI logs.

| `id` | `viewer` becomes |
|------|------------------|
| `hs-err` | `ksitools-jvm-crash-viewer.html` — filename `hs_err_pid*` **or** first 2 KB contains `A fatal error has been detected by the Java Runtime Environment` |
| `alb-log` | `ksitools-alb-log-viewer.html` — first field `http`/`https`/`h2`/`ws`/`wss`/`grpc` **and** ≥12 space-separated fields with a quoted request containing `HTTP/` |
| `vpc-flow` | `ksitools-vpc-flow-viewer.html` — header `version account-id interface-id` **or** field 1 in `2..5` and field 2 a 12-digit account id |
| `pprof` | `ksitools-pprof-viewer.html` — filename `*.pprof` / `*.pb.gz` / `*.collapsed`, or gzip whose inflated first bytes look like protobuf + `samples`/`functions` strings; folded stacks: ≥3 lines of `ident;ident … <int>` |
| `sql-explain` | hub card, not a sniffer default (paste is the common path). Optional: `QUERY PLAN` / `"Plan"` JSON / Oracle `\| Id \| Operation \|` can offer a jump without stealing `.json` / `.txt` from the JSON or log viewers |

EMV has **no** new sniffer id — the ASN.1 page already owns BER.

### 5.4 Hub groups

Add a hub section **Binaries, disk & packages** on `index.html` (after Migration / next to Security):

- PE / ELF metadata
- Git objects
- Disk image (FAT / ISO)
- Package guts (deb / rpm / jar)

HAR stays under **Security & APIs**.

Terraform cards stay under **Cloud IaC & packaging** (do not create a second Terraform heading):

- Terraform lock (`.terraform.lock.hcl`)
- Terraform CLI log (plan/apply text + JSONL)
- Terraform module (directory / multi-file)
- Terraform graph (`terraform graph` DOT)
- Terraform cost (breakdown JSON)

Plan, state, and HCL cards already live there — update their blurbs when the enhance-in-place work lands (state: “compare two states”; plan: “outputs + replace reasons”).

Ops paste-targets go on **existing** headings (do not invent a second “logs”
or “AWS” group):

| Card | Hub group | Blurb note |
|------|-----------|------------|
| SQL EXPLAIN | **Databases** (already on `index.html`) | “pg / MySQL / Oracle plan text or JSON — does not run SQL” |
| JVM crash log | **JVM diagnostics** | Next to thread dump / GC / heap / JFR |
| ALB access log | **Security & APIs** | Next to HAR / CloudTrail — “HTTP access log, not API events” |
| VPC Flow Log | **Host & network config** | Next to network-rules / firewall |
| pprof + folded stacks | **Platform ops** | Not Java-only (Go / perf / async-profiler). “Table, not a flame SVG” |

When enhance-in-place work lands, update blurbs:

- Log: “lazy open for multi-GB; 50k rows drawn”
- ASN.1: “DER/BER tree + EMV tag names; PAN masked”

Do **not** add hub cards for FIX, Cedar, a dedicated Caddyfile, Elasticsearch
mappings, KCL/CUE, or CyberChef-style recipe chains unless someone asks
(see §6 / §7).

---

## 6. Definition of done (per viewer)

A viewer is done when:

1. Drop of each must-accept fixture succeeds in Chromium and Firefox.
2. Oversize file hits `MAX_BYTES` with a readable error (not a tab freeze).
3. Caps announce themselves in the notice strip.
4. Masking (HAR headers, PE strings, package maintainer scripts that look like keys, Terraform tfvars / state / plan variables, ALB query tokens, JVM crash VM args, EMV PAN) is on by default.
5. Catalog checklist in §2.4 is complete.
6. `docs/viewers.md` lists **Not done** honestly.
7. Self-check passes on load.
8. If the page uses lazy slice: visible mode chip, progress, and a
   slice-boundary self-check. Oversize hits `ABSURD_MAX_BYTES` without freezing
   the tab.

Out of scope for this whole pack: Mach-O, VMDK/QCOW guest browse, NTFS/ext4, package signature verify, PE unpacking, git clone, HAR replay.

Out of scope for the Terraform pack: running `terraform` / `tofu`, binary `.tfplan`, provider install, registry hash verify, Sentinel evaluation, `TF_LOG=TRACE`, Terraform Cloud HTTP, Cost Explorer / live prices, full HCL interpreter.

Out of scope for the ops paste-target pack: connecting to a database, AWS /
IMDS APIs, geo-IP, flame-graph SVG libraries, live `/debug/pprof`, EMV
cryptogram verify, a dedicated Caddyfile page (generic proxy viewer already
outlines Caddy), a dedicated EML page (mbox already accepts `.eml`). **Not
scheduled unless someone asks:** FIX protocol (ChorusOps / broker-dealer
wire, not the public KSI subset), Cedar, Elasticsearch index mappings,
KCL/CUE, CyberChef recipe chaining, schema-less protobuf wire decode,
HL7/FHIR.

---

## 7. Open questions (resolve in the PR, not here)

- Whether JAR stays dual-listed (archive **and** package guts) or the hub card for “JAR” moves entirely to the package viewer. Recommendation: **both cards**, different blurbs — archive = member list, package = manifest + Maven coords.
- Whether xz inflate is required on Firefox ESR. If missing, DEB/RPM still must show control/header tags.
- Git SHA-256 object format (`sha256:` content) — skip until someone files a real sample.
- Terraform module viewer vs HCL viewer: keep both (recommended). HCL = one file, line ranges. Module = directory inventory. If that feels like two doors for the same `.tf`, the hub blurb must make the split obvious.
- Cost viewer naming: “Terraform cost” vs mentioning Infracost in the hub card. Recommendation: **KSI name + “accepts Infracost breakdown JSON”** in the blurb, so people find it without turning the catalog into a vendor list.
- Whether OpenTofu gets its own hub cards. Recommendation: **no** — same files, one line in each Terraform viewer’s Accepts table.
- EMV overlay vs a dedicated EMV page. Recommendation: **overlay on ASN.1** (§4.21). A second BER walker would drift.
- pprof as a flame SVG vs a table. Recommendation: **table** (same honesty as Terraform graph). A tiny indented tree is a should, not a library.
- Whether the sniffer may steal `.log` for ALB / VPC / `hs_err`. Recommendation: **only on a strong signature** (§5.3). Weak or mixed files stay on the generic log viewer, which may *notice* after those pages exist.
- FIX protocol in public KSI. Recommendation: **no** unless a KSI user asks — it is in the ChorusOps hand-off because that estate serves broker-dealers.
- Dedicated Caddyfile / Elasticsearch mapping / Cedar. Recommendation: **leave unscheduled**; Caddy is already on the proxy viewer.

When those are decided, write them into `docs/viewers.md` and tick the corresponding item in this file.
