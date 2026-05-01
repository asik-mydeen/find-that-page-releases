# Changelog

All notable changes to FindThatPage are documented here.

## 1.4.1 — 2026-05-01 — Popup UX fixes

- **"Recent" now means recent.** Default empty-state sort was silently
  frecency (last-visit time weighted by visit count), so a frequently-
  visited older page would outrank one you just opened. Now it's pure
  last-visit order. "Most visited" sort still works as before.
- **"Index this page" now tells you what happened.** Clicking it on a
  URL already in the index used to say "Indexed." with no count change,
  looking broken. It's now "Updated existing entry." (correct — dedupe
  by normalized URL) versus "Added to memory." for a new row.
- **Sort toggle added to the popup.** Recent / Most-visited tabs appear
  when no query is entered. Changing it writes to settings so the
  preference is consistent across the popup, overlay, and full search.

## 1.4.0 — 2026-05-01 — High-coverage indexing for chat sites

Long AI chats (ChatGPT, Claude, Gemini) now get 3× the indexing coverage
of regular pages, with near-real-time re-indexing so streaming replies
become searchable within a minute.

### What changed
- **150,000-char index cap** (was 50,000) for high-coverage domains —
  covers ~25,000 words of conversation per chat, up from ~8,000.
- **45-second re-index cadence** with a 200-char delta threshold (was
  10 minutes / 400 chars) — streaming responses surface quickly.
- **Default list:** `chatgpt.com`, `chat.openai.com`, `claude.ai`,
  `gemini.google.com`. Add your own in Options → Long content / chat sites.
- **Subdomain-aware matching** shared with the excluded-domain logic —
  `sub.chatgpt.com` picks up the setting too.

### Scope guard
- Privacy rules are unchanged; password-gated / banking / sensitive-path
  pages are still blocked regardless of high-coverage status.
- Keywords extraction still caps its input at 50k chars to avoid
  spending serious time on giant conversations.

### Tests
- 16 new tests (11 for `matchesDomainList` — subdomain correctness, suffix
  spoofing, case sensitivity — and 5 for the extractor `maxChars` override).
- 195 tests green total.

## 1.3.0 — 2026-05-01 — Chunked body search, fuzzy fallback, redesigned settings

### Search depth
- **4× more content searchable per page.** Body text is now indexed as
  overlapping ~3000-char chunks up to 20,000 chars total, replacing the
  flat 5,000-char cap. "I read it somewhere" on long Wikipedia / GitHub /
  blog posts now resolves.
- **Schema v3 migration** rebuilds FTS from stored text on first run —
  existing history gains chunked coverage without re-browsing.
- Best-chunk-per-page aggregation keeps ranking meaningful; same term
  appearing in multiple chunks no longer duplicates the page in results.

### Fuzzy "Did you mean?"
- Zero-hit queries now offer a one-click correction ("kuberntes" →
  "kubernetes") in the popup, overlay, and full-tab search page.
- Prefix-extension fallback: partial queries like "observabili" that
  FTS5 porter can't prefix-match now get suggested the full word.
- Toggleable in Settings → Search experience.

### Options page redesign
- Hero header with big-number stats (pages / domains / visits / pinned).
- Sticky section nav. Card-style sections with coloured accent bars.
- Modern switch-style toggles replace plain checkboxes.
- Segmented Recent / Most-visited control.
- Pause shortcuts extended: 15 min / 1 hour / 8 hour.

### New user preferences (all sync-mirrored)
- Show/hide "Did you mean?" suggestions
- Default-open results in background tabs
- Toggle word count on result cards
- Persist Recent vs Most-visited sort globally (overridable per session)

### New bulk action
- **Delete all pages from a domain** — retroactive cleanup separate from
  the exclude-list (exclude prevents future indexing; this clears
  existing history).

### Import & reliability
- Fixes "Import failed — not a FindThatPage export?" for large export
  files: the prior code bundled the full page list into one 64 MiB-capped
  message. Now chunked into 200-page batches with per-chunk progress and
  real error surfacing.
- Tightened `isImportablePage` guards against malformed exports crashing
  downstream UI string ops.
- Scheme-check on open: `javascript:` / `data:` / `file:` URLs smuggled
  via import or storage corruption now resolve to `about:blank` before
  reaching `browser.tabs.create`.

### Testing
- 179 tests green (was 139). New coverage for the chunker, chunked
  search ranking, fuzzy prefix matching, import validation, URL scheme
  gating, and concurrent import races.
- CI now gates releases on green typecheck + tests.

### Performance
- 50k-page bench: worst query 59ms p95 (was ~49ms pre-chunk). Trade of
  ~1.5× latency for 4× content coverage. See `docs/BENCHMARKS.md`.

## 1.2.0 — 2026-04-30 — Bigger empty state, sort toggle, Firefox install polish

- **Overlay empty-state:** raised the recents cap from 20 → 50 so you can actually scroll recent history without a query.
- **New sort toggle** in the overlay: `Recent` (frecency — last visit boosted by visit count) vs. `Most visited` (raw visit count, tie-break by last visit). Applies to the entire page table, not just the visible 50.
- **Clearer count label:** `Most recent 50 · 35 total` / `Most visited 50 · 35 total` / `N matches · 35 total` replaces the old `20 of 35 pages`, which read like an active filter.
- **Sort preference persists** per tab via sessionStorage.
- **Firefox build fix:** `offscreen` permission is now emitted only for Chromium-class builds. Firefox uses the direct-Worker fallback; it rejected the unknown permission outright before.
- **Local install:** unpacked builds now mirror to `built-files/chrome-mv3/` AND `built-files/firefox/`. New `npm run sync:builds` script refreshes both from `.output/` between formal releases.

## 1.1.0 — 2026-04-30 — Branding + new icon set

- Replaced the default puzzle-piece icon with a custom magnifying-glass-over-stacked-cards mark rendered at 16 / 32 / 48 / 96 / 128 and a 1024 master source.
- Chrome Web Store assets: 440×280 promo tile and 1400×560 marquee, both matching the in-extension palette (`#1d4ed8` → `#0f172a`).
- Store listing URLs updated to link to the public releases repo.
- No functional changes; search, chips, overlay, and indexing behave identically to v1.0.1.

## 1.0.1 — 2026-04-30 — CI pipeline verification

- Release workflow hardened: removed a redundant `gh auth login` call that fought with `GH_TOKEN`, dropped the noisy upload-artifact step that was hitting storage quota.
- No user-facing changes from v1.0.0. Reinstalling is a no-op.

## 1.0.0 — 2026-04-30 — First public release

### Search
- SQLite FTS5 full-text search across titles, keywords, summaries, domains, and body text
- BM25 ranking with column weighting and frecency boost (visit count × recency)
- Highlighted snippets with Chrome text-fragment deep links (`#:~:text=...`)
- Chip-based AND filters via **Tab** key
- Smart prefixes: `site:`, `domain:`, `in:title`, `in:body`, `in:summary`, `in:keywords`
- Stopword skip (`the`, `and`, `of`, …) to reduce noise
- Narrow `SELECT` (no body column in result rows) for faster round-trips at scale
- Backspace-on-empty-input removes last chip
- Shake-on-reject for chips that parse down to nothing

### UI
- Spotlight-style overlay on any page (`⌘⇧K` / `Ctrl+Shift+K`)
- Full-screen search tab (opens in dedicated tab)
- Popup with inline search, index-this-page button, and settings link
- Pinned / Recent section dividers in result list
- Per-result actions: Open, Pin, Forget, Exclude domain, More from domain
- 30-second Undo Forget with a restore toast
- Top-domain chips on the full search page
- Dark mode via `prefers-color-scheme` (CSS custom properties + `color-scheme: light dark`)
- React error boundaries on every entrypoint
- Options page opens in its own tab (not the chrome://extensions modal)
- Settings page has a filter input that hides non-matching sections

### Indexing
- Runs at `document_idle` with MutationObserver + `pushState`/`popstate` hooks for SPAs
- Incremental re-index every 10 minutes on long-lived visible tabs when body text changes by ≥400 chars
- Coalesced batched writes (300ms window)
- "Index this page now" button in the popup, using on-demand content-script injection
- Automatic retention pruning (180 days or 20,000 pages by default, configurable)
- FTS body trimmed to 5,000 most-informative characters (full text still stored for export)

### Privacy
- Zero network calls from the extension itself
- Password-field detection blocks indexing on any page with `<input type="password">`
- Sensitive-domain blocklist (banking, payment processors, auth)
- RFC 1918 + IPv6 ULA + link-local + `.internal/.corp/.home.arpa/.local` hostname skip
- Incognito never indexed
- Per-domain exclusion list (user-managed)
- Export / import as JSON
- One-click clear-all-memory
- `chrome.storage.sync` mirrors only preferences (never pages) for cross-device settings

### Extension plumbing
- Manifest V3 with offscreen document for SQLite + SAH
- Firefox MV3 support via direct-Worker fallback when `chrome.offscreen` is unavailable
- On-demand content-script injection via `chrome.scripting.executeScript` so shortcuts and "Index this page" work on tabs that pre-date install
- Idempotent schema migrations (`ALTER TABLE ADD COLUMN IF NOT EXISTS` pattern via PRAGMA introspection)
- One-shot FTS-rebuild gate via `meta.schema_version` to recover from a prior FTS5 contentless-delete bug

### Known limitations
- First index of a very long page can block content script for ~100ms on low-end devices
- FTS rebuild on large databases (>5k pages) takes 1-2 seconds on extension reload after schema bumps
- Firefox build is untested against AMO submission rules; works on about:debugging
