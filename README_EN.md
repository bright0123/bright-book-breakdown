# Bright Book Breakdown

A narrative-style book deconstruction workflow for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Core features: three-round cognitive compression + dual-layer reading notes + "first occurrence = link" wikilink convention.

## Features

- **Format-agnostic input**: Not limited to PDF or EPUB — supports virtually any book format
  - Condition: book content must be converted to a text file (.txt, .md) that Claude Code can read
  - Examples: PDF → OCR to .txt; EPUB/azw3/mobi → parse to .md; scanned books → photo + OCR
- Three-round cognitive compression: Skeleton Scan → Deep Dissection → Soul Extraction
- Narrative reading notes output: readable chapter-by-chapter prose + critical analysis layer
- "First occurrence = link" wikilink convention: jump links in prose, thematic overview at bottom
- **Optional: Batch Ingest** — partner with Obsidian vault to deconstruct a full book into an interconnected concept network

## Installation

Place the skills directory in your Claude Code skills folder.

## Usage

```
/bright-book-breakdown 《Book Title》
/拆书 《Book Title》
/拆书 《Book Title》 + {specific request}
```

## Two-Layer Workflow

### Layer 1: Single Book Deep Read (Steps 1–3)

```
Receive book
  │
  ├── Step 1: Three-round cognitive compression (internal only, not written to disk)
  │    ├── Skeleton Scan → global structure
  │    ├── Deep Dissection → argumentation chain
  │    └── Soul Extraction → beyond the author
  │
  ├── Step 2: Write narrative reading notes (output to books/sources/)
  │    ├── Narrative layer: readable chapter-by-chapter prose
  │    └── Analysis layer: critical analysis components
  │
  └── Step 3: Validate and update related concepts (grouped by theme at bottom)
```

### Layer 2: Batch Ingest into Knowledge Base (Optional)

Partner with Obsidian vault to deconstruct a full book into an interconnected concept network. **A one-time event, not ongoing construction.**

```
Batch Ingest one book
    │
    ├── Phase 1: Text extraction (text layer first, OCR as fallback)
    │    ├── Text layer available (embedded in PDF/EPUB) → extract directly
    │    └── Scanned / corrupted text layer → OCR extract to .txt
    │
    ├── Phase 2: Chapter segmentation
    │    ├── Split by table of contents into individual .txt files
    │    └── Store in vault's raw/chapters/ directory
    │
    ├── Phase 3: Parallel agent deconstruction
    │    ├── Each chapter → one background agent
    │    ├── Each agent identifies 3–8 sub-concepts
    │    ├── Each sub-concept → one books/concepts/ page
    │    └── Link with [[wikilink]] to related concepts
    │
    └── Phase 4: Integration
         ├── Wikilink validation (scan all new pages; create concept pages for dangling links first)
         ├── Update books/meta/index.md (global index)
         ├── Append books/meta/log.md (operation log)
         └── Update books/meta/hot.md (high-value page markers)
```

## Vault Output Structure

After running Batch Ingest, the vault directory structure:

```
vault/
├── raw/                          # Original book files (one-time storage)
│   ├── books/                     # Raw books (PDF/EPUB/TXT etc.)
│   └── chapters/                  # Text split by chapter
├── books/
│   ├── sources/                  # Reading notes (narrative, readable)
│   │   └── {BookTitle}.md
│   ├── concepts/                 # Sub-concept pages (batch generated)
│   │   └── {ConceptName}.md
│   ├── entities/                 # People/orgs/books etc.
│   ├── comparisons/              # Comparative analysis notes
│   ├── meta/
│   │   ├── index.md              # Global index (all concept page directory)
│   │   ├── log.md                # Operation log (appended each session)
│   │   └── hot.md                # High-value page markers (sorted by relevance)
│   └── sources/                  # Categorized book directories
└── wiki/                         # Thematic overview notes (optional)
```

**Notes**:
- `meta/index.md`, `meta/log.md`, `meta/hot.md` are the navigation infrastructure of the knowledge base
- `raw/` stores original book files — can be cleared or kept after deconstruction
- Concept pages connect via [[wikilink]] to form a navigable knowledge graph

## Wikilink Convention

**First occurrence = link**: When a concept first appears in the narrative → inline `[[wikilink]]`; subsequent appearances → plain text without link. The "Related Concepts" section at the bottom provides a thematic overview.

**Prerequisite**: Target concept files must exist before writing links. In Batch Ingest mode, they are pre-created by Phase 3 agents.

## License

MIT License