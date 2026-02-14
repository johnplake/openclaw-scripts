# zot — Zotero CLI

A Python CLI for querying and managing Zotero research libraries.

## Features

- **Search & Browse** — Search items, list collections/tags, get item details
- **PDF Management** — Download PDFs, attach to items (local or URL)
- **Library Copying** — Copy items between libraries with all annotations
- **Annotations** — View PDF annotations (highlights, ink, notes)
- **Notes** — Add notes to items (syncs to Zotero cloud + local markdown)
- **Citations** — Export BibTeX, formatted citations
- **Local Index** — Track downloaded PDFs with metadata for offline access

## Installation

### Requirements

- Python 3.11+
- `httpx` library

### Setup

1. Copy `zot` to a directory in your PATH
2. Set environment variables (see Configuration below)

## Configuration

### Environment Variables

```bash
# Required for personal library access
ZOTERO_USER_ID=...        # Your Zotero user ID
ZOTERO_PERSONAL_KEY=...   # API key (read-only recommended)

# Required for group library access
ZOTERO_GROUP_ID=...       # Group library ID
ZOTERO_GROUP_KEY=...      # API key (read/write)

# Optional
ZOTERO_LOCAL_DIR=...      # Override local storage path (default: ~/.openclaw/workspace/zotero-local)
```

### Getting API Keys

1. Go to https://www.zotero.org/settings/keys
2. Create a new key with appropriate permissions
3. Your user ID is shown on the same page

## Commands

### Search

```bash
# Search by title/author/etc
zot search "language model" --library personal

# Full-text search (searches PDF content)
zot search "transformer" --library personal --fulltext

# Limit results
zot search "attention" --library group --limit 5
```

### Item Details

```bash
# Get full item metadata
zot item ABCD1234 --library personal
```

### Download PDF

```bash
# Download to default location (zotero-local/)
zot pdf ABCD1234 --library personal

# Download to specific path
zot pdf ABCD1234 --library personal -o /path/to/paper.pdf
```

### Collections & Tags

```bash
zot collections --library personal
zot tags --library group
```

### Recent Items

```bash
zot recent --library personal --limit 10
```

### Add Items (Group Library)

```bash
# Basic
zot add "Paper Title" --type journalArticle --authors "Last, First"

# With PDF from URL
zot add "Paper Title" \
    --type journalArticle \
    --authors "Last, First; Last2, First2" \
    --pdf https://arxiv.org/pdf/2512.15011.pdf \
    --collection "my-collection"

# Full metadata
zot add "Attention Is All You Need" \
    --type conferencePaper \
    --authors "Vaswani, Ashish; Shazeer, Noam" \
    --date "2017" \
    --doi "10.48550/arXiv.1706.03762" \
    --publication "NeurIPS 2017" \
    --url "https://arxiv.org/abs/1706.03762" \
    --abstract "The dominant sequence transduction models..." \
    --tags "transformers,attention,nlp" \
    --collection "deep-learning" \
    --pdf https://arxiv.org/pdf/1706.03762.pdf
```

**Item types:** `journalArticle`, `conferencePaper`, `book`, `bookSection`, `preprint`, `thesis`, `report`, `webpage`

### Attach PDF to Existing Item

```bash
# Local file
zot attach ABCD1234 paper.pdf

# From URL
zot attach ABCD1234 --url https://arxiv.org/pdf/2512.15011.pdf
```

### Copy Between Libraries

Copy items with PDF attachments and **all annotations** (highlights, ink, notes):

```bash
# Copy from personal to group
zot copy LP99KCUZ --from personal --to group

# Copy to a specific collection
zot copy LP99KCUZ --from personal --to group --collection "my-papers"
```

**What gets copied:**
- Item metadata (title, authors, abstract, tags, etc.)
- PDF attachments
- All PDF annotations with position data (highlights, ink drawings, text notes)

**Note:** If an item has >100 annotations, a warning will be displayed (Zotero API limit).

### View Annotations

```bash
# Text output (grouped by page)
zot annotations ABCD1234 --library personal

# JSON output
zot annotations ABCD1234 --library personal --format json

# Show full text (no truncation)
zot annotations ABCD1234 --library personal --full
```

**Annotation types:**
- 🖍️ `highlight` — Highlighted text
- 💬 `text` — Sticky note
- 🖼️ `image` — Area selection
- ✏️ `ink` — Freehand drawing

### Add Notes

```bash
# Inline content
zot note ABCD1234 --content "My summary..."

# From file (preferred for longer notes)
zot note ABCD1234 --file summary.md

# With tags
zot note ABCD1234 --file notes.md --tags "summary,review"

# List notes on an item
zot notes ABCD1234 --library personal
```

Notes sync to Zotero cloud and are also saved locally as markdown in `zotero-local/notes/`.

### Render Notes to PDF

```bash
# Render and attach to Zotero item
zot render ABCD1234 summary.md

# Custom output filename
zot render ABCD1234 summary.md -o "Summary.pdf"

# Local only (don't upload)
zot render ABCD1234 summary.md --no-attach
```

### Citations & Export

```bash
# Formatted citation
zot cite ABCD1234 --style apa --library personal

# BibTeX export
zot export ABCD1234 --format bibtex --library personal
```

**Export formats:** `bibtex`, `ris`, `csljson`, `mods`, `tei`

**Citation styles:** `apa`, `chicago`, `mla`, `harvard`, `ieee`, or any [CSL style](https://www.zotero.org/styles/)

### Local Index

Track downloaded PDFs with metadata:

```bash
# List indexed items
zot index list

# Search index
zot index find --query "attention"

# Sync with Zotero (reconcile deletions, update metadata)
zot index sync
```

## API Limits

- **Pagination:** Zotero API returns max 100 items per request
- **Annotations:** If a PDF has >100 annotations, a warning is displayed
- **Rate limiting:** Zotero may throttle excessive requests

## Local Storage

Default location: `~/.openclaw/workspace/zotero-local/`

Contents:
- Downloaded PDFs
- `index.json` — Metadata index
- `notes/` — Local copies of notes as markdown

Override with `ZOTERO_LOCAL_DIR` environment variable.

## License

MIT
