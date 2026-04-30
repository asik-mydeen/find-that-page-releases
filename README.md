# FindThatPage

> Spotlight search for every page you've ever visited. **Local. Private. Offline-first.**

FindThatPage is a Chrome / Firefox MV3 extension that quietly indexes every page you visit into a private on-device SQLite database and lets you search it with a single keyboard shortcut (`⌘⇧K` on Mac, `Ctrl+Shift+K` on Windows/Linux).

- Zero network. Zero cloud. Zero AI API. Zero telemetry.
- Full-text search with BM25 ranking and highlighted snippets.
- Chip-based filter stacking (press `Tab` to lock in a term, then keep narrowing).
- Smart prefixes like `site:github.com`, `in:title react`, `in:body webassembly`.
- 30-second Undo Forget, pinned/recent dividers, visit-count frecency.

## Install

**Chrome Web Store:** _coming soon — pending review._ Once live, this README will link directly.

**Manual (unpacked) install — for early adopters:**

1. Download the latest build zip from [Releases](./releases/) → unzip.
2. Open `chrome://extensions` → enable **Developer mode** (top-right).
3. Click **Load unpacked** → select the unzipped folder.
4. Click the puzzle icon → pin FindThatPage → click it → enable browser memory.

## Privacy — the short version

- Every byte stays on your device. We have no servers to ask.
- Passwords, banking, auth, localhost, incognito — never indexed. Configurable exclusion list for anything else.
- Full export / import / wipe via Settings.
- Full policy: [`PRIVACY_POLICY.md`](./PRIVACY_POLICY.md).

## Docs

- [Privacy policy](./PRIVACY_POLICY.md)
- [Changelog](./CHANGELOG.md)
- [Chrome Web Store listing copy](./STORE_LISTING.md) (same text you see on the store page)

## Source code

This repository is artefacts only — installable zips, privacy policy, changelog, and release notes. The source itself is closed. Every release zip here is built from an internal tagged commit with a reproducible build step (`npm run zip`) and hashed in `SHA256SUMS.txt`.

FindThatPage is closed-source. Released builds are reproducible from our internal tagged commits and checksummed in each release folder. For security review or audit access, email **support@findthatpage.app**.

## Contact

- Support: **support@findthatpage.app**
- Feature requests and bug reports: [open an issue](../../issues)
- Security reports: email support and include "SECURITY" in the subject — do not file public issues for security bugs.

## License

The extension artefacts in [`releases/`](./releases/) are distributed under the [MIT License](./LICENSE). The source release of the same codebase will use the same license when published.
