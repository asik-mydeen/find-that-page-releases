# Privacy Policy — FindThatPage

**Effective date:** 2026-04-30
**Last updated:** 2026-04-30

FindThatPage is a browser extension that indexes the pages you visit so you can search them later. This policy explains, in plain language, what data it touches and what it does with that data.

## The short version

- Everything stays on your device.
- Nothing is ever sent to any server run by us, or to any third party.
- We do not have accounts, sign-in, or servers. There is no backend for us to collect from.
- You can export, delete, or clear your data at any time.

## What data the extension processes

When you visit a standard web page (`http://` or `https://`), FindThatPage reads the following from your browser and stores it in an on-device database:

- The page URL
- The page title
- The page's domain
- The page's favicon URL (a pointer, not the image itself)
- A readable-text extract of the page body
- A short auto-generated summary (the first few sentences)
- A list of high-frequency keywords
- The time you visited the page
- The number of times you have visited the same URL

### What it NEVER reads

- Passwords, form-field contents, or any page that contains a password field
- Pages from the built-in sensitive-domain blocklist (banking, payment processors, auth providers)
- Pages from any domain you have added to your personal exclusion list
- `chrome://`, `about:`, `file://`, `view-source:`, and other non-web URLs
- Localhost, private networks (10.x, 192.168.x, 172.16–31.x, IPv6 ULA/link-local, `.internal`, `.corp`, `.home.arpa`, `.local`)
- Incognito / private-window tabs

### Why the `scripting` permission exists

Chrome only auto-injects our content script into pages loaded **after** the extension was installed. On tabs that were already open, pressing `⌘⇧K` or clicking "Index this page" needs the extension to inject the script on demand using `chrome.scripting.executeScript`. The injected file is our own bundled `content-scripts/content.js`; we never inject remote code and the injection is scoped to the tab the user actively interacts with.

## Where the data is stored

On your device, in two locations provided by your browser:

- **IndexedDB** — used as an intermediate store and for settings
- **OPFS (Origin Private File System)** — used to store the SQLite database that powers full-text search

Both are sandboxed to this extension. Neither is synced by the browser, uploaded to any server, or accessible to web pages.

## Data that is optionally synced

If you enable Chrome's built-in "Sync settings" feature at the browser level, a small subset of **your preferences** is mirrored through Google's sync service so they can follow you to another Chrome profile you sign into. Specifically:

- Whether indexing is enabled
- Your excluded-domain list
- Your retention settings (days to keep, max pages)

This is **preferences only** — your indexed pages, URLs, titles, and body text are **never** synced and never leave the device they were collected on. You can disable this by signing out of Chrome or by removing the extension.

## Data we do NOT collect

- We have no servers. We cannot collect anything, even if we wanted to.
- We do not use analytics (Google Analytics, Plausible, Mixpanel, PostHog, Sentry, etc.).
- We do not use crash reporting services.
- We do not track ad events, conversions, or retargeting.
- We do not read or transmit cookies, localStorage, or sessionStorage of the pages you visit.
- We do not share or sell any data to any third party. There is nothing to share.

## Third-party services

None. FindThatPage makes zero outgoing network requests from the extension's own code.

(The pages you visit still make their own network requests, as they would regardless of whether this extension is installed. Your browser's normal privacy settings and your chosen DNS / ad-blocker continue to apply.)

## Your controls

- **Pause** indexing for 15 minutes or 1 hour at a time
- **Disable** indexing entirely (toggle in popup or options)
- **Exclude** any domain so it is never indexed
- **Forget** individual pages (with a 30-second undo window)
- **Export** your entire index as a JSON file
- **Import** a previously exported JSON file
- **Clear all memory** — one-click deletion of the entire database
- **Uninstall** — removing the extension removes the database with it

## Children's privacy

FindThatPage does not knowingly collect data from anyone, including children under 13. Since we have no backend, we do not knowingly or unknowingly store anyone's data off-device.

## Changes to this policy

If the extension's data practices change in a future version, this policy will be updated and the "Last updated" date will change. Material changes will also be called out in the extension's release notes.

## Contact

- Email: support@findthatpage.app
- GitHub Issues: https://github.com/asik-mydeen/find-that-page/issues

*(Replace the above with real contact points before publishing.)*
