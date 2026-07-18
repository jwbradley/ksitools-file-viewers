# Contributing

Thanks for helping improve KSI Tools File Viewers.

## Project philosophy

Before you write code, please keep these constraints in mind:

1. **Local only** — no network calls for parsing or analytics. Prefer embedding libraries over CDN `<script src>`.
2. **Single-file viewers** — each `ksitools-*-viewer.html` should remain openable standalone.
3. **No build step required** for end users — plain HTML/CSS/JS they can open and audit.
4. **Honest limits** — if a feature would hang the browser on large files, add caps and user-visible notices (see existing `MAX_BYTES` / `MAX_RENDER` / `MAX_NODES` patterns).
5. **Ops UX** — sticky toolbars, light/dark via `prefers-color-scheme`, clear errors, useful exports.

## Ways to contribute

- Bug fixes and accessibility improvements
- Documentation clarifications
- Performance / large-file handling
- New format viewers that follow the same patterns
- Tests or small fixtures for parsers (keep samples free of real secrets)

## Development setup

```bash
git clone git@github.com:jwbradley/ksitools-file-viewers.git
cd ksitools-file-viewers
python3 -m http.server 8080
```

Open `http://localhost:8080/` and edit the relevant HTML file. Hard-refresh after changes.

There is no package.json, bundler, or test runner by design. Use browser DevTools for debugging.

## Coding guidelines

### Structure of a viewer

Most viewers share the same shape:

- `<style id="appcss">` — app chrome + light/dark rules  
- Sticky `header.bar` with actions  
- Drop zone (`#drop`) until a file is loaded  
- Error / notice regions  
- Inline `<script>` with:
  - constants (`MAX_BYTES`, …)
  - parse / render / export helpers
  - drag-and-drop + file input wiring

When adding a viewer, mirror an existing one (JSON or CSV are good templates).

### Security

- Never assign untrusted markup to `innerHTML` without sanitization (see Markdown + DOMPurify).
- Prefer `textContent` and DOM APIs for tree UIs.
- Do not add external script/style URLs.
- Keep SQLite write statements blocked if you touch that file.

### Style

- Match existing naming, spacing, and GitHub-inspired colors.
- Prefer readable vanilla JS over frameworks.
- Comment non-obvious algorithms (diff LCS, CSV quoting, secret-key heuristics).

## Pull requests

1. Fork and branch from the default branch.
2. Keep PRs focused (one viewer or one concern).
3. Update `docs/viewers.md` and `CHANGELOG.md` when behavior changes.
4. Describe how you tested (browser + sample file types).
5. Do not commit real customer data or secrets in fixtures.

## Commit messages

Use clear, imperative subjects:

```text
json-viewer: raise MAX_NODES notice clarity
docs: document hex magic-byte list
```

## Reporting bugs

Open a GitHub issue with:

- Viewer file name
- Browser / OS
- Sample that reproduces (redact secrets; minimize the file)
- Expected vs actual behavior

Security issues: see [SECURITY.md](SECURITY.md) — **not** public issues.

## Code of conduct

By participating, you agree to the [Code of Conduct](CODE_OF_CONDUCT.md).
