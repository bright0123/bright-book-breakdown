# Bright Book Breakdown

**中文文档：** [README.zh-CN.md](README.zh-CN.md)

A narrative-style book deconstruction workflow for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Core features: three-round cognitive compression + dual-layer reading notes + "first occurrence = link" wikilink convention.

## Features

- **Format-agnostic input**: Supports virtually any book format — not limited to PDF or EPUB
  - Condition: book content must be converted to a text file (.txt, .md) that Claude Code can read
  - Examples: PDF → OCR to .txt; EPUB/azw3/mobi → parse to .md; scanned books → photo + OCR
- Three-round cognitive compression: Skeleton Scan → Deep Dissection → Soul Extraction
- Narrative reading notes: readable chapter-by-chapter prose + critical analysis layer
- "First occurrence = link" wikilink: links appear inline in prose, full thematic overview at bottom
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

```
vault/
├── raw/
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
│   │   ├── index.md              # Global index
│   │   ├── log.md                # Operation log
│   │   └── hot.md                # High-value page markers
│   └── sources/                  # Categorized book directories
└── wiki/                         # Thematic overview notes (optional)
```

## Wikilink Convention

**First occurrence = link**: When a concept first appears in the narrative → inline `[[wikilink]]`; subsequent appearances → plain text without link.

**Prerequisite**: Target concept files must exist before writing links.

## Acknowledgements

This skill draws inspiration from:

- **Karpathy's LLM Wiki** — core concept of using LLM as a local knowledge base
- [claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian) — Claude Code + Obsidian integration workflow
- [Im-wiki-obsidian-blink](https://github.com/iBlinkQ/Im-wiki-obsidian-blink) — Obsidian bidirectional linking + AI

## License

MIT License