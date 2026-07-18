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

## No other runtime dependencies

JSON, XML, CSV, config, log, diff, and hex viewers use browser built-ins and project-authored JavaScript only.

---

## Attribution requirement

If you redistribute modified viewers that still include these libraries, retain their copyright and license notices as required by each license.
