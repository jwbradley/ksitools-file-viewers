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

## SQLite — `ksitools-sqlite-viewer.html`

| | |
|--|--|
| **Accepts** | `.db`, `.sqlite`, `.sqlite3`, `.db3` |
| **Limits** | 200 MB; 5k result rows drawn |
| **Features** | Table/view sidebar with counts, SQL box, **Ctrl+Enter** to run, read-only statement guard |
| **Allowed SQL** | `SELECT`, `PRAGMA`, `WITH`, `EXPLAIN` (first keyword) |
| **Exports** | Save results `.csv` · Save results `.json` |

> This file is large (~1 MB) because sql.js + WASM are embedded for offline use.
