# Releases

All published builds of FindThatPage. Each release has a folder with the installable zip, release notes, and SHA-256 checksums.

| Version | Date | Notes |
|---|---|---|
| [v1.8.6](./v1.8.6/RELEASE_NOTES.md) | 2026-05-04 | Release |
| [v1.8.5](./v1.8.5/RELEASE_NOTES.md) | 2026-05-04 | Release |
| [v1.8.4](./v1.8.4/RELEASE_NOTES.md) | 2026-05-04 | Release |
| [v1.8.3](./v1.8.3/RELEASE_NOTES.md) | 2026-05-04 | Release |
| [v1.8.2](./v1.8.2/RELEASE_NOTES.md) | 2026-05-04 | Release |
| [v1.8.1](./v1.8.1/RELEASE_NOTES.md) | 2026-05-04 | Release |
| [v1.8.0](./v1.8.0/RELEASE_NOTES.md) | 2026-05-04 | Release |
| [v1.7.0](./v1.7.0/RELEASE_NOTES.md) | 2026-05-03 | Release |
| [v1.6.0](./v1.6.0/RELEASE_NOTES.md) | 2026-05-03 | Release |
| [v1.5.0](./v1.5.0/RELEASE_NOTES.md) | 2026-05-03 | Release |
| [v1.4.1](./v1.4.1/RELEASE_NOTES.md) | 2026-05-01 | Release |
| [v1.4.0](./v1.4.0/RELEASE_NOTES.md) | 2026-05-01 | Release |
| [v1.3.0](./v1.3.0/RELEASE_NOTES.md) | 2026-05-01 | Release |
| [v1.2.0](./v1.2.0/RELEASE_NOTES.md) | 2026-05-01 | Release |
| [v1.1.0](./v1.1.0/RELEASE_NOTES.md) | 2026-05-01 | Release |
| [v1.0.1](./v1.0.1/RELEASE_NOTES.md) | 2026-04-30 | CI pipeline verification |
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
