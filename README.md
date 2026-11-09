# Bulk File & Image Downloader (URLs to ZIP)

Download **any list of file URLs** (images, PDFs, documents, audio, video, anything) into Apify storage in one run, and get **one ZIP with everything**. Each file gets a row with its **file name, type, size, SHA-256 hash** and a direct download link. Identical files are stored once, and you can keep only the types and sizes you want.

Paste URLs, or point it at the **dataset of another Actor** (a product, real-estate or social media scraper, for example) to download every image it found.

## What can this bulk downloader do?

- 📥 **Any file type:** JPG, PNG, WebP, AVIF, SVG, GIF, PDF, Word, Excel, CSV, ZIP, MP3, MP4 and more.
- 🗜️ **One ZIP of everything:** `ALL_FILES.zip` in the run's key-value store, ready to download (up to 200 MB of files).
- 🔗 **URLs from other Actors:** give a dataset ID and the fields to read (`images`, `imageUrl`, `url`...). Lists of URLs and objects with a `url` work too.
- 🧬 **Duplicate detection:** files with identical content are stored once, even from different URLs. Duplicates are free.
- 🎯 **Filters:** only certain types (`image/`, `pdf`, `jpg`, `video/mp4`), and a maximum file size.
- 🏷️ **Proper names and types:** names come from the server or the URL, and the type is checked against the file's own bytes, so a JPG served as "application/octet-stream" still gets `.jpg`.
- 🔁 **Retries** on timeouts, rate limits and server errors, and runs resume where they stopped after a migration.

## Who uses it?

- **E-commerce teams** downloading product images found by a store scraper.
- **Researchers and analysts** saving PDFs, reports and datasets from a list of links.
- **Marketers and agencies** backing up creatives and media libraries.
- **AI and ML teams** collecting image datasets, with SHA-256 for de-duplication and integrity.

## How to download files in bulk

1. Paste **File URLs**, one per line, or enter a **dataset ID** from another Actor run.
2. Optionally set **File types**, **Max file size** and whether to **Create a ZIP**.
3. Click **Start**. Open the **Storage → Key-value store** tab for the files and `ALL_FILES.zip`, or the **Output** tab for the list.

## Input example

```json
{
    "urls": ["https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf", "https://httpbin.org/image/png"],
    "datasetId": "",
    "fileTypes": ["image/", "pdf"],
    "maxFileSizeMb": 25,
    "deduplicate": true,
    "createZip": true
}
```

## Output example

```json
{
    "resultType": "file",
    "url": "https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf",
    "finalUrl": "https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf",
    "fileName": "dummy.pdf",
    "contentType": "application/pdf",
    "sizeBytes": 13264,
    "sha256": "3df79d34abbca99308e79cb94461c1893582604d68329a41fd4bec1885e6adb4",
    "httpStatus": 200,
    "status": "downloaded",
    "key": "dummy.pdf",
    "fileUrl": "https://api.apify.com/v2/key-value-stores/<store-id>/records/dummy.pdf",
    "downloadedAt": "2026-11-09T12:00:00.000Z"
}
```

Every URL gets a row. `status` is `downloaded`, `duplicate` (with `duplicateOf`), `skipped` (too large or filtered out, with the reason) or `failed` (with the HTTP status or error).

## Use it from AI agents (Claude, ChatGPT, Cursor)

AI assistants can run this Actor as a tool through [Apify's MCP server](https://docs.apify.com/platform/integrations/mcp):

```
https://mcp.apify.com?tools=rsiddiq95/bulk-file-downloader
```

## How much does it cost?

Pay per file stored, plus a small amount per full megabyte for large files (files under 1 MB pay nothing extra). See the **Pricing** tab. Failed, skipped and duplicate URLs are free.

## Limitations

- Direct file links only. Pages that need a login, a browser or a bot check are not supported; you can pass your own headers (for example a token for your own storage) in **Request headers**.
- Files are kept in the run's key-value store for your plan's data retention period. Download the ZIP or copy the files elsewhere if you need them longer.
- The ZIP holds up to 200 MB of files; any beyond that are still stored individually and the log says so.
- Maximum single file size is 500 MB.

## FAQ

**Where are my files?**
In the run's **Storage → Key-value store**. Each row's `fileUrl` links straight to its file.

**Something isn't working.**
Open an issue on the **Issues** tab with your input.

## Run it

This Actor runs on Apify: https://apify.com/rsiddiq95/bulk-file-downloader
