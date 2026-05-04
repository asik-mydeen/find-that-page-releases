# Chrome Web Store Listing — FindThatPage v1.8.5

## Name

**FindThatPage — Local Browser Memory Search**

Alternates (pick based on which name survives Chrome's uniqueness check):
- FindThatPage: Private Page Memory
- FindThatPage: Offline Search History

## Short description (≤132 chars, single line, used under the extension icon)

Search every page you've ever visited — locally, instantly, privately. No cloud. No AI API. No account.

Alternates:
- Private full-text search across pages you've already read. Everything stays on your device. ⌘⇧K to find that page.
- Your browser, but you can actually search inside the pages you visited. 100% local. Zero telemetry.

## Category

Productivity

## Language

English (US)

## Tagline options (for screenshots / marketing)

1. "I know I read that somewhere."
2. "Your browser memory, finally searchable."
3. "Search the web you've already seen."
4. "Private full-text search across your own browsing."
5. "⌘⇧K to remember anything."

## Detailed description (Chrome Web Store "Description" field, ≤16,000 chars)

> Paste this whole block in as-is.

---

**You read something last week and can't find it again. FindThatPage fixes that.**

FindThatPage builds a private, full-text search index of every article, doc, thread, and page you visit — and runs entirely on your device. When you need to find it again, a single keyboard shortcut opens a Spotlight-style overlay that searches your own browsing memory in milliseconds.

No cloud sync. No AI API. No account. No telemetry. Your reading history never leaves your computer.

---

**Why people install it**

- You read a great essay on Tuesday, want to quote it on Thursday, and Google can't find it because it's buried in someone's newsletter archive.
- You researched a bug last month, closed the tab, and now you vaguely remember "it had something to do with timezone offsets."
- You're collecting sources for a project and want to search the stuff you've already read before searching the open web.
- You don't want your browsing history in someone else's database.

---

**What it does**

- **Instant keyboard search.** Press `⌘⇧K` (Mac) or `Ctrl+Shift+K` (Windows/Linux) on any page. A Spotlight-style overlay appears. Start typing.
- **Full-text search.** Match on titles, keywords, summaries, domains, and the actual body text of every page you've indexed. Powered by SQLite FTS5 with BM25 ranking.
- **Chip-based filter stacking.** Type a term, press **Tab** to lock it in as a chip, then keep refining. `react` → Tab → `hooks` → Tab → only pages matching both.
- **Smart chip prefixes.** Type `site:github.com` or `in:title react` or `in:body webassembly` — chips auto-scope to the right column for laser-precise filtering.
- **Highlighted snippets.** Results show the matching phrase in context, so you can see *why* a page matched before you click.
- **Deep links.** Pressing Enter opens the page scrolled directly to the matching text (uses the Chrome text-fragment API).
- **Frecency ranking.** Pages you've re-read recently and often float to the top. Visit counts are displayed on each result.
- **Pin important pages.** Pinned items survive retention cleanup and always appear at the top of results. A pinned/recent divider keeps the list organised.
- **Undo Forget.** Accidentally deleted a result? A 30-second undo toast gives you a second chance.
- **"Index this page now."** A one-click button in the popup forces indexing of the current tab — useful for pages that load content after navigation.
- **Top-domain chips + one-click domain filter.** Each result has a "More from {domain}" button, and the search page surfaces frequently-matched domains as quick-filter chips.

---

**Privacy is the product, not a feature**

- **Everything is local.** All data is stored in SQLite (via OPFS SyncAccessHandle) and IndexedDB on your device. No server. No sync (unless you opt in to Chrome's built-in settings sync, which only covers your preferences — never pages).
- **No AI APIs.** Search is keyword + synonym + BM25 ranking — not an LLM and not a vector database calling out to a third party.
- **Strict content gates:**
  - Pages with password fields are never indexed.
  - Incognito windows are never indexed.
  - A built-in blocklist covers banking, payment, and auth domains.
  - Localhost, private networks (RFC 1918, link-local, IPv6 ULA), and `.internal / .corp / .home.arpa` hostnames are skipped.
- **Fine-grained control.** Exclude any domain, pause indexing for 15 minutes or an hour, or disable it entirely from the popup.
- **Full data portability.** Export everything as JSON. Import it back on another device. Clear everything with one click. It's your data.

---

**Keyboard shortcuts**

- `⌘⇧K` / `Ctrl+Shift+K` — open search overlay on the current page
- `Tab` — add the current term as a filter chip (AND with existing chips)
- `Backspace` on empty input — remove the last chip
- `↑` / `↓` — move between results
- `Enter` — open result in a new tab (scrolled to match)
- `⌘+Enter` / `Ctrl+Enter` — open in a background tab (keep searching)
- `Shift+Enter` — open foreground but keep the overlay open
- `⌘1–⌘9` — jump directly to result 1–9
- `Esc` — close overlay

Customize the shortcut at `chrome://extensions/shortcuts`.

---

**Smart search prefixes**

Type these inside the search input and press Tab to lock in as chips:

- `site:github.com` — only pages from github.com
- `domain:reddit.com` — same as site:
- `in:title react` — match only in page titles
- `in:body webassembly` — match only in body text
- `in:summary auth` — match only in summaries
- `in:keywords cdk` — match only in keywords

---

**Who this is for**

- Writers, researchers, students — anyone who accumulates sources.
- Developers who keep bouncing between docs, GitHub issues, and Stack Overflow.
- Power users who want search-your-own-history without handing it to a startup.
- Anyone who's typed "that thing I read about…" into Google and hit a dead end.

---

**Who this is NOT for**

- If you want cloud sync of full page contents across devices — not available by design.
- If you want AI-generated summaries of your reading — this uses keyword search, not an LLM.
- If you only visit a handful of pages per week, a bookmarks folder might be enough.

---

**How it works (technical)**

- Built on WXT + React 19 + TypeScript, strict mode throughout.
- SQLite (compiled to WebAssembly) with an FTS5 virtual table, running in an OPFS SyncAccessHandle-backed persistent store.
- Porter tokenizer, BM25 ranking with column weighting (title ≫ keywords ≫ summary ≫ domain ≫ body), contentless FTS5 for space efficiency.
- Service worker coalesces concurrent indexing writes into batched transactions.
- Automatic retention prunes old non-pinned pages (default: keep 180 days or 20,000 pages, whichever hits first — configurable).
- Works on long-lived SPAs via MutationObserver + pushState/popstate hooks + 10-minute incremental re-index while visible.
- On-demand content-script injection via `chrome.scripting.executeScript` so the overlay and "Index this page" work on tabs that pre-date the extension install.
- Firefox MV3 supported via direct-Worker fallback (no offscreen document).

Release notes, issues, and support: https://github.com/asik-mydeen/find-that-page-releases

---

**Roadmap**

- Firefox AMO submission
- Optional local embedding model for semantic search (still 100% on-device, opt-in)
- Graph view of linked pages
- Per-site redaction rules for enterprise use

---

**Feedback welcome.** If it breaks, tell me where. If it's missing something you'd use daily, tell me that too.

## Permissions justification (pasted into the permission justification fields)

### `storage`
> Required to save user settings (indexing toggle, excluded domains, retention policy) and to store the indexed page database locally. No data is ever sent off-device.

### `tabs`
> Required to open search results in a new tab when the user presses Enter, and to read the active tab's URL when the user triggers "Index this page now" from the popup.

### `activeTab`
> Required to send the "toggle overlay" message to the currently focused tab when the user presses the keyboard shortcut. Without it, the Spotlight-style overlay cannot be shown on the current page.

### `offscreen`
> Required because SQLite with OPFS SyncAccessHandle is only available to dedicated Workers — which Manifest V3 service workers cannot create directly. The offscreen document hosts the Worker that runs the local database. Nothing on that page is visible to the user; no network calls are made from it.

### `scripting`
> Required so that the search overlay and the "Index this page now" button work on tabs that existed before the extension was installed or reloaded. On those tabs, the content script is not auto-injected by the browser; this permission lets the background service worker inject it on demand via `chrome.scripting.executeScript`. It is only used to inject our own content script, never remote code.

### Host permissions: `http://*/*`, `https://*/*`
> Required to run the content script that extracts a readable-text version of each page you visit (title, domain, body text). Extraction happens entirely in your browser and is only used to populate your local search index. The extension never makes its own network requests to any of these sites.

### Content scripts matching `http://*/*`, `https://*/*`
> Same reason as host permissions above: reading the DOM of the page you're currently viewing so the extension can add it to your local searchable memory. The content script does not inject anything into the page except the search overlay iframe when you press the keyboard shortcut.

## Single-purpose description (required by the Chrome Web Store policy)

FindThatPage has a single purpose: provide local, private, full-text search across the pages a user has already visited in their browser. It does not advertise, sync, track, or share user data.

## Data-usage disclosures (Chrome Web Store "Privacy practices" form)

Check the following in the privacy practices form; everything else should be unchecked.

| Disclosure | Value |
|---|---|
| Does this extension collect or use... | |
| Personally identifiable information | **No** |
| Health information | No |
| Financial and payment information | No |
| Authentication information | No |
| Personal communications | No |
| Location | No |
| Web history | **Yes — processed locally only, never transmitted** |
| User activity | No |
| Website content | **Yes — page text is read for local indexing only, never transmitted** |

### Compliance certifications (toggle all three to YES)

- [x] I do not sell or transfer user data to third parties, outside of the approved use cases.
- [x] I do not use or transfer user data for purposes that are unrelated to my item's single purpose.
- [x] I do not use or transfer user data to determine creditworthiness or for lending purposes.

## Privacy policy (hosted separately — see PRIVACY_POLICY.md)

Privacy policy is published at:

  https://asik-mydeen.github.io/find-that-page-releases/PRIVACY_POLICY

Paste that URL into the store listing's "Privacy policy" field. Source of truth is `store/PRIVACY_POLICY.md` in this repo, mirrored to `asik-mydeen/find-that-page-releases` on every `npm run release`.

## Support contact

Email: support@findthatpage.app
GitHub issues: https://github.com/asik-mydeen/find-that-page-releases/issues

*(Update both before submission.)*

## Submission artifact

Upload `store/dist/find-that-page-1.8.5-chrome.zip` to the Chrome Web Store developer console.
