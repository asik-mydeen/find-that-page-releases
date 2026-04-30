# Releases

All published builds of FindThatPage. Each release has a folder with the installable zip, release notes, and SHA-256 checksums.

| Version | Date | Notes |
|---|---|---|
| [v1.0.0](./v1.0.0/RELEASE_NOTES.md) | 2026-04-30 | Release |
| [v1.0.0](./v1.0.0/RELEASE_NOTES.md) | 2026-04-30 | First public release |

## Install from a release zip

1. Download the `-chrome.zip` (or `-firefox.zip`) from the version folder.
2. Unzip it somewhere permanent (deleting the folder uninstalls the extension).
3. Chrome: `chrome://extensions` → Developer mode → **Load unpacked** → select the unzipped folder.
4. Firefox: `about:debugging` → This Firefox → **Load Temporary Add-on** → select the `manifest.json` inside the unzipped folder.

## Verify the download

```bash
shasum -a 256 find-that-page-1.0.0-chrome.zip
```

Compare the output to the hash in `SHA256SUMS.txt`.
