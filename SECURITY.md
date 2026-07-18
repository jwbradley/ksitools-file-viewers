# Security Policy

## Privacy-first product promise

KSI Tools File Viewers are designed so that **user files never leave the browser**. Parsing, rendering, search, and export run client-side. There is no application server, no telemetry, and no intentional outbound request for file content.

That promise depends on:

1. Opening a **trusted copy** of these HTML files (or a host you control).
2. A browser without malicious extensions that can read page content.
3. Not pasting secrets into untrusted environments.

## Supported versions

Only the latest commit on the default branch is supported. These are static HTML tools without a versioned release train; if you redistribute copies, prefer tracking this repository.

## What to report

Please report privately if you find:

- Cross-site scripting (XSS) when rendering user-controlled content (especially Markdown, XML, HTML export paths)
- Bypass of the SQLite **read-only** query guard (`SELECT` / `PRAGMA` / `WITH` / `EXPLAIN` only)
- Any code path that **uploads file content** or phones home without clear user action
- Path or file-system access beyond the browser File API
- Prototype pollution or unsafe `innerHTML` use with unsanitized input
- Supply-chain issues in embedded libraries that affect these pages

## What not to report here

- “I opened a huge file and the tab froze” — file size / DOM caps are documented; performance issues are fine as normal issues unless they enable security bypasses
- Browser bugs unrelated to this project
- Social engineering against end users

## How to report

**Do not open a public GitHub issue for unfixed security bugs.**

Email: **james@kchoptalk.com**

Please include:

- Affected file(s) (e.g. `ksitools-markdown-viewer.html`)
- Browser and OS
- Steps to reproduce
- Impact assessment (e.g. “unsanitized markdown can run script when Save as HTML is opened”)
- Suggested fix if you have one

You should receive an acknowledgment within a few business days when possible.

## Hardening notes for operators

| Practice | Why |
|----------|-----|
| Serve over HTTPS if hosted | Avoid on-path HTML injection |
| CSP headers if self-hosting | Defense in depth for XSS |
| Prefer `file://` or a locked-down static host for secrets | Reduces exposure to injected third-party scripts |
| Keep browsers updated | WASM / File API security fixes |
| Treat **Save as HTML** exports as untrusted input if shared | They embed rendered content |

## SQLite viewer

The SQLite viewer runs **sql.js** in a Web Worker and rejects non-read queries at the UI layer (`SELECT` / `PRAGMA` / `WITH` / `EXPLAIN` only).

- **Small databases (≤ 64 MB)** are loaded fully into sql.js memory.
- **Larger databases** stay on a `File` handle; the engine reads only the byte ranges each query needs (lazy / page-range open). The full file is not copied into a single `ArrayBuffer`.
- **WAL mode:** only the file you open is used. An adjacent `-wal` is not applied unless you checkpoint first with a native SQLite client.

It is not a multi-user database server. Do not treat it as a hardened database product; treat it as a convenience browser for local inspection.
