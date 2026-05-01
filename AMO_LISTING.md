# Firefox Add-ons (AMO) Listing — FindThatPage v1.4.1

All fields below are paste-ready for https://addons.mozilla.org/developers/ item submission.

## Identity

| Field | Value |
|---|---|
| Add-on name | **FindThatPage — Local Browser Memory Search** |
| Summary (≤250 chars) | Search every page you've ever visited — locally, instantly, privately. No cloud. No AI API. No account. Press Ctrl+Shift+K on any page to open a spotlight-style search over your own browsing memory. |
| Description | See full text below. |
| Category (primary) | Search Tools |
| Category (secondary) | Privacy & Security |
| Tags | search, history, local, privacy, spotlight, bookmarks, productivity |
| Homepage | https://asik-mydeen.github.io/find-that-page-releases/ |
| Support site | https://github.com/asik-mydeen/find-that-page-releases/issues |
| Support email | support@findthatpage.app |
| License | MIT License |

## Description (paste into the "Description" field — rich text supported)

> Paste as-is. Line breaks become paragraphs.

**You read something last week and can't find it again. FindThatPage fixes that.**

FindThatPage builds a private, full-text search index of every article, doc, thread, and page you visit — and runs entirely on your device. When you need to find it again, a single keyboard shortcut opens a spotlight-style overlay that searches your own browsing memory in milliseconds.

No cloud sync. No AI API. No account. No telemetry. Your reading history never leaves your computer.

---

**What it does**

- Instant keyboard search. Press Ctrl+Shift+K on any page — a spotlight overlay appears, start typing.
- Full-text search across titles, keywords, summaries, domains, and page body text. Powered by SQLite FTS5 with BM25 ranking.
- Chip-based filter stacking. Type a term, press Tab to lock it in as a chip, then keep refining.
- Smart chip prefixes: `site:github.com`, `in:title react`, `in:body webassembly`.
- Highlighted snippets show where the match lives.
- Deep links: pressing Enter opens the page scrolled directly to the matching text.
- Frecency ranking with per-result visit count. Pages you re-read float to the top.
- Empty-state sort toggle: "Most recent" (frecency) or "Most visited" (raw visit count).
- 30-second Undo Forget for accidentally deleted results.

---

**Privacy is the product, not a feature**

- Everything is local. All data is stored in SQLite (WebAssembly, via Worker) and IndexedDB on your device.
- No AI APIs. Search is keyword + BM25 ranking, no remote model calls.
- Strict content gates: password-field pages, banking/auth domains, incognito, localhost, private networks, and `.internal / .corp / .home.arpa` hostnames are all skipped.
- Full user control: per-domain exclusion list, pause indexing, export/import as JSON, one-click wipe.
- Only your extension **preferences** sync (if you opt into Firefox Sync for extension settings) — page contents never sync.

---

**Keyboard shortcuts**

- Ctrl+Shift+K — open search overlay on the current page
- Tab — add the current term as a filter chip
- Backspace on empty input — remove the last chip
- Arrow keys — move between results
- Enter — open result in new tab (scrolled to match)
- Ctrl+Enter — open in background tab, keep searching
- Shift+Enter — open in foreground but keep overlay open
- Escape — close overlay

---

**How it works (technical)**

- Manifest V2 on Firefox, built with WXT 0.20 + React 19 + TypeScript.
- SQLite compiled to WebAssembly with an FTS5 virtual table (Porter tokenizer, BM25 ranking).
- On Firefox, the database runs in a dedicated Worker spawned directly from the background page (Chromium uses an offscreen document; this is a per-browser build).
- Retention policy prunes old non-pinned pages weekly (default 180 days / 20,000 pages, configurable).
- Content script runs at `document_idle` + a 10-minute visible-tab re-index loop for long-lived SPAs.
- Zero network calls from the extension's own code — you can verify in the browser devtools network panel.

---

**Source code and audit**

Released builds ship with reproducible-from-source SHA-256 hashes in the `SHA256SUMS.txt` attached to each GitHub Release. The source archive for every release is available in the Mozilla Add-ons Developer Console for reviewer audit, and can be requested separately at support@findthatpage.app.

Release notes and the privacy policy are always at:
- https://asik-mydeen.github.io/find-that-page-releases/CHANGELOG
- https://asik-mydeen.github.io/find-that-page-releases/PRIVACY_POLICY

## AMO data-collection disclosure form

Every AMO submission must answer "does your add-on collect or transmit data?". Paste these answers exactly:

### Does this add-on transmit any of the following data?

| Data type | Transmitted? | Notes |
|---|---|---|
| Technical data | **No** | Nothing is transmitted to us; we have no servers. |
| Interaction data | **No** | |
| Personal data (other) | **No** | |
| Search terms | **No** | Queries stay in the browser; never leave the device. |
| Browsing activity | **No** | Indexed locally only; never transmitted. |
| Website content | **No** | Extracted locally for indexing; never transmitted. |
| Website activity | **No** | |
| Financial / payment info | **No** | |
| Health info | **No** | |
| Authentication info | **No** | |
| Personal communications | **No** | |
| Location | **No** | |

### Does the add-on require data to function?

> The add-on reads **locally** the same page content you are viewing in your browser, in order to build a private, on-device search index of pages you have already visited. No data is transmitted to any server. The user enables or disables this at will and may wipe the local database with a single click.

### Privacy policy

URL: https://asik-mydeen.github.io/find-that-page-releases/PRIVACY_POLICY

## Permission justifications (paste into the "Permissions" tab during review)

### `storage`
Persists user settings (indexing toggle, excluded domains, retention) and caches for the local SQLite database proxy. No data is transmitted.

### `tabs`
Required to open a search result in a new tab and to read the current tab URL when the user clicks the popup's "Index this page now" button. No continuous tab-polling.

### `activeTab`
Required to send the "toggle overlay" message to the focused tab when the user presses Ctrl+Shift+K. Without it the spotlight overlay cannot render on top of the current page.

### `scripting`
Required so that the spotlight overlay and "Index this page now" work on tabs that existed before the add-on was installed or updated. Firefox does not retroactively inject content scripts into pre-existing tabs, so this permission allows on-demand `browser.scripting.executeScript` injection of the add-on's own bundled content script — never remote code, never cross-origin script injection.

### `<all_urls>` host permission (http/https matches)
Required to run the content script that extracts a readable-text version of each page you view, so the add-on can add it to your local searchable memory. All extraction happens locally; the add-on makes zero outbound HTTP requests of its own.

## Source code upload (required for reviewer audit)

Because the extension is built with WXT + TypeScript + Vite (i.e. compiled/bundled tooling), AMO requires a source-code zip for the human reviewer. Upload the matching `-sources.zip` from each release alongside the `-firefox.zip`:

| Release | Firefox zip | Sources zip |
|---|---|---|
| 1.4.1 | `find-that-page-1.4.1-firefox.zip` | `find-that-page-1.4.1-sources.zip` |
| 1.4.0 | `find-that-page-1.4.0-firefox.zip` | `find-that-page-1.4.0-sources.zip` |
| 1.3.0 | `find-that-page-1.3.0-firefox.zip` | `find-that-page-1.3.0-sources.zip` |
| 1.2.0 | `find-that-page-1.2.0-firefox.zip` | `find-that-page-1.2.0-sources.zip` |

Location (local): `store/dist/find-that-page-<version>-sources.zip` after `npm run zip:firefox`.
Location (public release): attached to the GitHub Release on `find-that-page-releases`.

## Build instructions to include with source upload

AMO reviewers must be able to reproduce the build. Paste this into the "Describe how to build the add-on from the uploaded source code" field:

```
Prerequisites:
  - Node.js 20 or newer
  - npm 10 or newer

Build steps:
  1. Extract find-that-page-<version>-sources.zip
  2. cd into the extracted directory
  3. npm ci
  4. npm run build:firefox
  5. The built extension is at .output/firefox-mv2/

No private network dependencies; npm ci fetches only from the public registry.
No secrets, no environment variables, no post-install scripts that download at runtime.
Output manifest version matches the submitted zip.
```

## Developer account notes

- Submit from https://addons.mozilla.org/developers/
- If prompted for distribution: **On this site (AMO)**. Self-hosted distribution is possible but bypasses review — we always want review.
- First-time submission fee: none (AMO is free).
- Review SLA: typically 1–10 business days; human review because of permissions and bundled build artefacts.
- Once approved, the extension auto-updates for existing users via Firefox's update feed (handled by AMO).

## Post-publish

Add the AMO listing URL to `store/URLS.md` under `Firefox`:

```
AMO listing: https://addons.mozilla.org/addon/findthatpage/
```

(Actual slug assigned by AMO at publish; may differ.)
