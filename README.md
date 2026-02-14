# OpenClaw Scripts

Custom CLI tools for an OpenClaw assistant workspace.

## Scripts

### `zot` — Zotero CLI

A Python CLI for querying and managing Zotero libraries.

**Features:**
- Search items, download PDFs, manage collections
- Add new items with metadata and PDF attachments
- **Copy items between libraries** with all annotations (highlights, ink, notes)
- View and export PDF annotations
- Attach notes to items (syncs to Zotero cloud + local markdown)
- Render markdown notes to PDF
- Local PDF cache with index for offline access

📖 **[Full Documentation](docs/zot.md)**

**Quick start:**
```bash
# Set environment variables
export ZOTERO_USER_ID=...
export ZOTERO_PERSONAL_KEY=...
export ZOTERO_GROUP_ID=...
export ZOTERO_GROUP_KEY=...

# Search
zot search "attention mechanism" --library personal

# Copy item with all annotations
zot copy ITEMKEY --from personal --to group --collection my-papers
```

**Dependencies:** Python 3.11+, `httpx`

---

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
