# Architecture

## Overview

Each viewer is a **single HTML document** containing:

- Embedded CSS (including dark mode via `prefers-color-scheme`)
- Minimal HTML shell (toolbar, drop zone, result regions)
- One or more inline `<script>` blocks (application logic + any vendored library)

There is no framework, no bundler, and no server-side component.

```text
┌─────────────────────────────────────────────┐
│  Browser tab                                │
│  ┌───────────────────────────────────────┐  │
│  │  ksitools-*-viewer.html              │  │
│  │   CSS · UI · parsers · exporters      │  │
│  └───────────────────────────────────────┘  │
│           ▲                                 │
│           │ File API / drag-drop            │
│           │                                 │
│      local disk / clipboard / downloads     │
└─────────────────────────────────────────────┘
         no app server · no CDN fetch for libs
```

## Shared UX patterns

Most viewers implement the same interaction model:

| Element | Role |
|---------|------|
| Sticky header bar | Title, filename, meta, search/filters, actions |
| Drop zone | First-run affordance; hides after successful load |
| Error panel | Parse failures and size-limit messages |
| Notice strip | Soft warnings (render caps, duplicate keys, etc.) |
| Loading overlay | Shown before heavy sync work; double-`rAF` yield so the spinner paints |

## Parsing strategy by format

| Format | Approach |
|--------|----------|
| JSON | `JSON.parse`, custom collapsible tree DOM |
| YAML | Embedded **js-yaml** `load` (safe schema usage as noted in UI) |
| XML | Browser `DOMParser`, collapsible tree |
| CSV/TSV | Custom delimiter sniff + RFC-style quoting |
| Config | Line-oriented parser for `.env` / ini / properties-style |
| Log | Line split + regex level/timestamp classification |
| Diff | Myers-style LCS over lines + word-level refine on change pairs |
| Hex | `ArrayBuffer` → hex/ASCII rows + magic-byte sniff |
| Markdown | Embedded **marked** + **DOMPurify** before inject |
| SQLite | Embedded **sql.js** (WASM base64) in a Worker; memory load if ≤ 64 MB, else File-backed lazy page reads |

## Performance guardrails

Browsers struggle when millions of DOM nodes are created. Viewers therefore separate:

1. **In-memory model** — full parse where feasible (for search/export)
2. **Rendered view** — capped with `MAX_RENDER` / `MAX_NODES`
3. **Hard refuse** — `MAX_BYTES` rejects files that are simply too large for a tab

When a cap applies, a visible notice explains what is shown vs what export/search still covers.

## Export model

Exports rebuild output from the in-memory model (or visible subset when intentionally filtered, e.g. log level filter):

- Clipboard: plain text / TSV / unified diff / pretty JSON
- Download: `Blob` + temporary object URL + synthetic click
- “Save .html”: standalone snapshot with embedded styles for sharing a *view* (still local generation)

## Theming

`:root { color-scheme: light dark; }` plus `@media (prefers-color-scheme: dark)` overrides. No theme toggle state is stored; OS preference wins.

## SQLite special case

`ksitools-sqlite-viewer.html` is large (~1 MB) because it embeds sql.js and WASM.

- **Small files (≤ 64 MB):** the Worker loads the whole file into sql.js memory (classic path).
- **Large files:** the Worker keeps a `File` handle and patches MEMFS `read`/`stat`/`seek` so SQLite’s `xRead` pulls only the requested byte ranges via `FileReaderSync` + `file.slice()`. Opening a multi‑GB database therefore does **not** allocate a multi‑GB `ArrayBuffer`.
- **UI:** table browsing is paginated (`LIMIT`/`OFFSET`); huge TEXT/BLOB cells are truncated in the grid; `COUNT(*)` is opt-in.
- **Safety:** statements must start with `select` / `pragma` / `with` / `explain`. Prefer `PRAGMA query_only=ON` after open.

## Why not a SPA framework?

- Auditable by security-conscious ops teams (one file, view-source)
- Zero install for recipients of a shared HTML file
- Works from USB / air-gapped machines after copy
- Avoids dependency churn and supply-chain surface for a tool whose job is *looking at files*

## Future-friendly extension points

If the project grows:

- Keep new formats as new single files, not a mega-app
- Extract shared CSS/JS only if a tiny static build or include strategy does not break “open one file” UX
- Prefer optional progressive enhancement over mandatory bundling
