# Privacy

## Summary

**KSI Tools File Viewers process files entirely in your browser.**  
They do not upload your files to KSI Tools, GitHub, or any other server as part of normal viewing.

## What the pages do

1. You choose a file via drag-and-drop or the file picker.
2. The browser reads bytes into memory (`FileReader` / `ArrayBuffer` / text decoding).
3. JavaScript in the page parses and renders the content.
4. Optional exports download a new file to *your* machine or copy to *your* clipboard.

No step in that pipeline requires a network request.

## What they do *not* do

- No account or login
- No cookies set by the viewers for tracking
- No analytics SDKs
- No remote logging of file contents
- No CDN dependencies (scripts and styles are embedded)

## Offline use

After you have a local copy of the HTML files, you can open them with no internet connection. Embedded libraries (marked, DOMPurify, js-yaml, sql.js WASM) are already inside the files. DevOps viewers (JWT/certs, HAR, archive, TOML, NDJSON) use pure JavaScript only — no extra embeds.

**JWT & certificates:** decoding is local and **does not verify signatures or trust chains**. Prefer this over pasting production tokens into public websites; still treat Save/Copy outputs as sensitive.

**Cloud credentials:** kubeconfig, Terraform state, IAM policies, and K8s Secrets often contain long-lived credentials. Prefer **mask-on-by-default** viewers, avoid “Save .html” snapshots of those files, and open the HTML from a local path you trust.

## Hosting caveats

If someone *hosts* these HTML files on a website:

| Hosting choice | Implication |
|----------------|-------------|
| Plain static host (S3, nginx, etc.) | Viewers still process files client-side; the host only serves the HTML/JS |
| Host injects ads / analytics / tag managers | Those third parties are **not** part of this project and may observe page context |
| Corporate reverse proxy with DLP/inspection | May log HTTP of the *page load*, not your local file bytes (files still stay client-side after load) |

**For highly sensitive data, open the HTML from a local disk path you trust** (`file://` or a local static server), not from an untrusted mirror.

## Browser and OS reality

These tools cannot override:

- Malicious browser extensions that can read page DOM or clipboard
- Endpoint monitoring / EDR on your machine
- Screenshots, screen sharing, or shoulder surfing
- You voluntarily pasting content into other apps or tickets

## Clipboard and exports

**Copy** and **Save** actions write data where you ask them to (clipboard or download folder). That is intentional. Treat exports like any other file you create.

## SQLite

Databases are opened with **sql.js** entirely in your browser (Web Worker):

- **≤ 64 MB:** file bytes are loaded into WASM memory for speed.
- **> 64 MB:** the original `File` is kept and only the pages SQLite requests are read (`slice` / range I/O). The whole multi‑GB file is not loaded into RAM up front.

Queries are restricted in the UI to read-oriented statements. The database file is never uploaded and is not written back; only optional CSV/JSON **exports of the current result set** are downloaded to your machine.

## Contact

Privacy or security concerns: **james@kchoptalk.com** (see also [SECURITY.md](../SECURITY.md)).
