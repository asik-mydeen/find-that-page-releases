# Changelog

All notable changes to FindThatPage are documented here.

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
