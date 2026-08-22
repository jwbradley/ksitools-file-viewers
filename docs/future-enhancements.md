# Future enhancements: HAR, git objects, PE/ELF, FAT/ISO, package guts

Roadmap for the next ops-oriented viewers. Implement against
[`CONTRIBUTING.md`](../CONTRIBUTING.md) and the house rules below. When a viewer
lands, copy its Accepts / Limits / Features / Exports table into
[`docs/viewers.md`](viewers.md) and mark that section done here.

These came out of a landscape pass against “drop anything” forensic suites.
KSI Tools stays a **structured ops viewer**, not a 1,200-format identifier.

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
   packages, PE overlay strings that look like keys).
6. **No customer data in fixtures.** Synthetic files only. No live tokens,
   hostnames of real customers, or private keys.

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

---

## 2. Shared build process

Every new (or enhanced) viewer follows this loop. Do not skip the catalog
steps — a USB-stick copy that cannot find the tool is a failed ship.

### 2.1 Scaffold

1. Copy a neighbor, not a blank page.
   - Table + detail pane: **HAR** or **PCAP**.
   - Binary `DataView` walk: **PCAP**, **EVTX**, **JKS**.
   - ZIP members: **Archive** / **Excel**.
   - Inflate: Archive’s `DecompressionStream('gzip'|'deflate-raw')`.
2. Rename title/heading to **KSI Tools &lt;Thing&gt; Viewer**.
3. Constants at the top: `MAX_BYTES`, `MAX_RENDER`, and any preview-size cap.
4. Loading overlay + double-`rAF` before heavy sync work.

### 2.2 Parse → model → capped render

- Parse into a plain object/array (search and export use the full model).
- Draw at most `MAX_RENDER` rows. Notice strip when truncated.
- Never assign untrusted bytes to `innerHTML`. `textContent` / `esc()` like HAR.
- Click-through preview of a member must re-cap (e.g. ≤256 KB text, same as archive).

### 2.3 Self-check

Embed a `_selfCheck()` / IIFE that parses the synthetic samples in this doc and
throws on regression. Log `FAIL` to the console; do not block the UI.

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

Suggested calendar (one person, honest):

| Step | Effort | Depends on |
|------|--------|------------|
| HAR gaps | 0.5–1 day | existing HAR |
| Git objects | 2–3 days | `DecompressionStream('deflate')` |
| PE/ELF | 2–3 days | PCAP-style DataView |
| FAT/ISO | 3–4 days | `File.slice`, archive tree UI |
| Packages | 3–4 days | archive ZIP/TAR + new Unix `ar` + RPM header |

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

### 5.2 Do not run untrusted code

- PE/ELF: no WebAssembly instantiate of the dropped file, no `Function` on extracted JS from a JAR.
- JAR: do not eval.
- ISO/FAT: do not auto-open HTML members as documents (`textContent` / download only).

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

### 5.4 Hub group

Add a hub section **Binaries, disk & packages** on `index.html` (after Migration / next to Security):

- PE / ELF metadata
- Git objects
- Disk image (FAT / ISO)
- Package guts (deb / rpm / jar)

HAR stays under **Security & APIs**.

---

## 6. Definition of done (per viewer)

A viewer is done when:

1. Drop of each must-accept fixture succeeds in Chromium and Firefox.
2. Oversize file hits `MAX_BYTES` with a readable error (not a tab freeze).
3. Caps announce themselves in the notice strip.
4. Masking (HAR headers, PE strings, package maintainer scripts that look like keys) is on by default.
5. Catalog checklist in §2.4 is complete.
6. `docs/viewers.md` lists **Not done** honestly.
7. Self-check passes on load.

Out of scope for this whole pack: Mach-O, VMDK/QCOW guest browse, NTFS/ext4, package signature verify, PE unpacking, git clone, HAR replay.

---

## 7. Open questions (resolve in the PR, not here)

- Whether JAR stays dual-listed (archive **and** package guts) or the hub card for “JAR” moves entirely to the package viewer. Recommendation: **both cards**, different blurbs — archive = member list, package = manifest + Maven coords.
- Whether xz inflate is required on Firefox ESR. If missing, DEB/RPM still must show control/header tags.
- Git SHA-256 object format (`sha256:` content) — skip until someone files a real sample.

When those are decided, write them into `docs/viewers.md` and tick the corresponding item in this file.
