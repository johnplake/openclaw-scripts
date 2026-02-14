# OpenClaw Scripts

Custom CLI tools for an OpenClaw assistant workspace.

## Scripts

### `zot` — Zotero CLI

A Python CLI for querying and managing Zotero libraries (personal read-only, group read/write).

**Features:**
- Search items, download PDFs, manage collections
- Add new items with metadata and PDF attachments
- Attach notes to items (syncs to Zotero cloud + saves local markdown)
- View PDF annotations made in Zotero's reader
- Render markdown notes to PDF and attach to items
- Local PDF cache with index for offline access
- Full-text search support

**Environment variables:**
```bash
ZOTERO_USER_ID=...        # Your Zotero user ID
ZOTERO_PERSONAL_KEY=...   # API key for personal library (read-only)
ZOTERO_GROUP_ID=...       # Group library ID
ZOTERO_GROUP_KEY=...      # API key for group library (read/write)
ZOTERO_LOCAL_DIR=...      # Optional: override local storage path
```

**Dependencies:** Python 3.11+, `httpx`

### `bsky` — Bluesky CLI

A bash script for basic Bluesky (AT Protocol) operations.

**Features:**
- Post skeets (with reply support)
- Like, repost, follow
- View timeline and notifications
- Search posts
- Get profiles

**Config file:** `~/.config/bsky/config.json`
```json
{
  "identifier": "your-handle.bsky.social",
  "password": "your-app-password",
  "did": "did:plc:..."
}
```

## Installation

1. Clone this repo
2. Set required environment variables / config files
3. Add scripts to your PATH or symlink them

## License

MIT
