# Contributing to KSI Tools File Viewers

**You are already the right person to send a PR.**

Typos. One-line UX fixes. A viewer for a format that ruined your Tuesday. Docs that finally say what the page actually does. A better error message. Dark-mode contrast. A fixture with the secrets stripped out. An idea that is only a GitHub issue so far.

We want it. All of it. This suite grew because people kept hitting “I wish this opened in the browser, privately” — if you have that itch, you belong here.

First-time contributor? Tiny change? Unsure which file to touch? Open an issue or a draft PR anyway. We would rather talk through a half-finished idea than have you sit on a useful one.

Please be excellent to each other — see the [Code of Conduct](CODE_OF_CONDUCT.md).

---

## What we would love from you

No contribution is too small. These are all first-class:

| If you want to… | Do this |
|-----------------|---------|
| Fix a bug you just hit | PR against the viewer file. Include browser + a redacted sample if you can. |
| Add a new format | New `ksitools-*-viewer.html`, plus hub / README / changelog / reference (checklist below). |
| Improve a viewer that already exists | Caps, search, exports, a11y, clearer notices — ship it. |
| Make docs less of a scavenger hunt | `README.md`, `docs/viewers.md`, `CHANGELOG.md`, this file, comments in HTML. |
| Polish the hub | Cards, groups, search copy on [`index.html`](index.html). |
| Report something without writing code | [Open an issue](https://github.com/jwbradley/ksitools-file-viewers/issues). Feature requests count. |
| Share a sample | Redact secrets. Minimize. Fixtures that teach the parser beat screenshots. |
| Improve performance | Large-file caps, incremental render, “this will be slow” notices. |
| Hunt accessibility gaps | Keyboard, contrast, labels, `prefers-color-scheme`. Huge win. |
| Tighten security / privacy | Follow [SECURITY.md](SECURITY.md) for unfixed vulns — **private**, not a public issue. |

If it makes these tools more useful, more honest, or easier to hand to a friend — send it.

---

## Five house rules (the only ones that really matter)

These exist so a USB-stick copy still works in a SCIF-adjacent Tuesday. They are not a vibe check.

1. **Local only.** Parsing and analytics never hit the network. Embed libraries in the HTML. No CDN `<script src>`. No telemetry.
2. **One file, one viewer.** Each `ksitools-*-viewer.html` must open standalone. End users get no build step, no `npm install`, no account.
3. **Brand it like the rest of the suite.** Display name is **KSI Tools** (space). Filenames are `ksitools-*-viewer.html`. Session keys, if you need them, look like `ksitools.tools.*`.
4. **Honest limits.** If a huge file would freeze the tab, cap it and say so in the UI (`MAX_BYTES` / `MAX_RENDER` / `MAX_NODES` patterns already in the tree).
5. **Ops UX.** Sticky toolbars, light/dark via `prefers-color-scheme`, useful errors, useful exports.

Everything else is “match a neighbor file and use your judgment.”

---

## Jump in

```bash
git clone git@github.com:jwbradley/ksitools-file-viewers.git
cd ksitools-file-viewers
python3 -m http.server 8080 --directory .
```

Open [http://localhost:8080/](http://localhost:8080/), hit `/` to search the hub, edit an HTML file, hard-refresh.

There is no `package.json`, bundler, or test runner **on purpose**. Browser DevTools is the debugger. That is a feature: you can audit the whole tool in one tab.

Prefer `file://`? Double-click the HTML. Some APIs are happier on localhost, which is why the tiny static server is there.

---

## How a viewer is put together

Mirror an existing page rather than inventing a new chrome. JSON and CSV are good templates.

Typical shape:

- `<style id="appcss">` — chrome + light/dark
- Sticky `header.bar` with actions
- Drop zone (`#drop`) until a file is loaded
- Error / notice regions
- Inline `<script>` with constants, parse / render / export helpers, drag-and-drop + file input

### Security (please)

- Do not assign untrusted markup to `innerHTML` without sanitization (Markdown uses DOMPurify — copy that pattern).
- Prefer `textContent` and DOM APIs for trees.
- No external script or stylesheet URLs.
- If you touch SQLite, keep write statements blocked.

### Style

- Match existing naming, spacing, and GitHub-inspired colors.
- Readable vanilla JS over frameworks.
- Comment the clever bits (diff LCS, CSV quoting, secret-key heuristics).

### New viewer checklist

Land these with the HTML so the suite stays searchable:

- [ ] File named `ksitools-<thing>-viewer.html`
- [ ] Title / heading say **KSI Tools**, not a one-word mashup
- [ ] Card on [`index.html`](index.html) in a sensible group
- [ ] Row in [`README.md`](README.md) catalog
- [ ] Section in [`docs/viewers.md`](docs/viewers.md) (Accepts / Limits / Features / Exports)
- [ ] Note under **Unreleased** in [`CHANGELOG.md`](CHANGELOG.md)
- [ ] No real customer data or live secrets in fixtures

Missing a box? Open the PR anyway and we will help fill it in.

---

## Pull requests

1. Fork, branch from the default branch, keep the PR about one viewer or one concern when you can.
2. Describe how you tested (browser + sample types). “Opened it and dropped `foo.json` in Firefox” is enough.
3. Draft PRs are welcome. Drive-by typo PRs are welcome. Giant new formats are welcome.
4. We will bikeshed product constraints (offline, single-file, honest caps) — not your willingness to contribute.

### Commit messages

Clear and imperative is plenty:

```text
json-viewer: raise MAX_NODES notice clarity
docs: document hex magic-byte list
hub: add search hint for Solr viewers
```

---

## Bugs and ideas

Open a GitHub issue with whatever you have. Ideal extras:

- Viewer file name
- Browser / OS
- A minimized, redacted sample
- Expected vs actual

“I have a format and no patch yet” is a perfectly good issue.

**Security issues:** [SECURITY.md](SECURITY.md) — email, not a public issue.

---

If you made it this far: thank you. Send the thing.
