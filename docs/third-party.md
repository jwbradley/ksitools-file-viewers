# Third-party notices

KSI Tools File Viewers embed the following open-source libraries **inside** the relevant HTML files so the tools work offline without CDN requests.

This project’s own code is MIT-licensed (see [LICENSE](../LICENSE)). Third-party code remains under its original terms.

---

## marked

| | |
|--|--|
| **Where** | `ksitools-markdown-viewer.html` |
| **Version** | 12.0.2 |
| **Project** | https://github.com/markedjs/marked |
| **License** | MIT |
| **Copyright** | Copyright (c) 2011-2024, Christopher Jeffrey |

Used to parse Markdown into HTML (GFM options enabled in the viewer).

---

## DOMPurify

| | |
|--|--|
| **Where** | `ksitools-markdown-viewer.html` |
| **Version** | 3.1.6 |
| **Project** | https://github.com/cure53/DOMPurify |
| **License** | Dual: Apache License 2.0 **and** Mozilla Public License 2.0 |
| **Copyright** | Cure53 and contributors |

Used to sanitize HTML produced by marked before insertion into the page.

Full license texts:  
https://github.com/cure53/DOMPurify/blob/3.1.6/LICENSE

---

## js-yaml

| | |
|--|--|
| **Where** | `ksitools-yaml-viewer.html`, `ksitools-kubeconfig-viewer.html`, `ksitools-k8s-viewer.html` |
| **Version** | 4.1.0 |
| **Project** | https://github.com/nodeca/js-yaml |
| **License** | MIT |

Used for local YAML parsing (safe load path as indicated in the UI).

---

## sql.js

| | |
|--|--|
| **Where** | `ksitools-sqlite-viewer.html` |
| **Project** | https://github.com/sql-js/sql.js |
| **Upstream** | SQLite + Emscripten/WebAssembly build |
| **License** | MIT (sql.js packaging); SQLite is public domain |

Provides an in-browser SQLite engine so databases can be inspected without a native client. Used for both full in-memory open (small files) and File-backed lazy page reads (large files).

---

## PDF.js (pdfjs-dist)

| | |
|--|--|
| **Where** | `ksitools-pdf-viewer.html` |
| **Version** | 3.11.174 |
| **Project** | https://github.com/mozilla/pdf.js |
| **Package** | https://www.npmjs.com/package/pdfjs-dist |
| **License** | Apache License 2.0 |
| **Copyright** | Copyright 2023 Mozilla Foundation |

Used to parse PDF documents and extract positioned text items in the browser. The main library and worker script are embedded; the worker is started from a `Blob` URL so no network or separate worker file is required.

Full license text:  
https://github.com/mozilla/pdf.js/blob/master/LICENSE

---

## No other runtime dependencies

JSON, XML, CSV, config, log, diff, hex, and most DevOps viewers use browser built-ins and project-authored JavaScript only.

---

## Attribution requirement

If you redistribute modified viewers that still include these libraries, retain their copyright and license notices as required by each license.


## Browser built-ins (not bundled)

Several migration viewers use the platform **`DecompressionStream`** API (`gzip`, `deflate-raw`) for ZIP member inflate, `.tar.gz`, and Excel OOXML. No extra library is downloaded; a modern Chromium, Firefox, or Safari is required for those paths.
