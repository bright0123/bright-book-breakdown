# Bright-Book-Breakdown

**中文** | [English](README.md)

Narrative Book Breakdown Workflow: turn a book into reading notes + navigable concept network.

## What is this

A Claude Code skill that breaks down books into a **navigable knowledge network**, not just notes.

Core method: **Three-Round Cognitive Compression** + **Dual-Layer Notes** + **Wikilink Concept Network**, optimized for use with **Obsidian**.

## Obsidian Integration

This skill is designed for Obsidian, with output directly importable to your vault:

- **Reading notes** → `books/sources/` (narrative + analysis layers)
- **Concept pages** → `books/concepts/` (navigable knowledge nodes)
- **Wikilink** → Connected concept network, bi-directional navigation

Obsidian's bi-directional linking turns concept pages into a web — click any `[[wikilink]]` to navigate, layer by layer.

## Use Cases

- Read a non-fiction book and want to build a personal knowledge base
- Connect concepts from a book to your existing knowledge network
- Using Obsidian or similar tools

## Full Workflow

```
Receive book
  │
  ├── Step 1: Three-Round Cognitive Compression (internal only)
  │    ├── Skeleton Scan → "What is this book about"
  │    ├── Deep Dissection → "Why does the author say this"
  │    └── Soul Extraction → "What else can I do with this"
  │
  ├── Step 2: Write Narrative Reading Notes
  │    ├── Narrative Layer: chapter text (400-800 chars/chapter)
  │    └── Analysis Layer: critical analysis components
  │
  └── Step 3: Verify and Update Related Concepts
       └── First occurrence → [[wikilink]]
```

### Batch Ingest

For breaking down entire books at once, generating a concept network:

```
PDF/EPUB
  │
  ├── Text Extraction
  │    ├── Preferred: PyMuPDF/pdfplumber (embedded text)
  │    └── Fallback: PaddleOCR (scanned PDF)
  │
  ├── Parallel Agent Breakdown
  │    └── Each chapter → 3-8 concept pages
  │
  └── Wikilink Verification
       └── Dangling links → create concept page first, then link
```

## Text Extraction Rules

| Scenario | Tool | Notes |
|----------|------|-------|
| PDF with embedded text | PyMuPDF / pdfplumber | Direct extraction, fast and accurate |
| Garbled/misaligned text | pdfplumber as backup | Try different tool |
| Scanned PDF (no text layer) | PaddleOCR GPU | Render page-by-page + OCR |

**Fallback order**: PyMuPDF → pdfplumber → OCR

OCR is the last resort, not the default.

## Output Structure

```
vault/
├── books/
│   ├── sources/        # Reading notes (narrative + analysis)
│   ├── concepts/       # Concept page network
│   ├── entities/       # Person/organization entity pages
│   └── meta/
│       ├── index.md    # Knowledge base index
│       └── log.md      # Operation log
```

### Concept Page Format

```markdown
---
title: {Concept Name}
type: concept
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
tags: [tag1, tag2]
sources: [source file path]
---

# {Concept Name}

> One-sentence definition

## Core Content
(Key points extracted from the original text)

## Operational Guide
(How to apply this concept in practice)

## Related Concepts
- [[Related Concept 1]]
- [[Related Concept 2]]

## References
- [[Source Page]]
```

### Wikilink Rules

- **First occurrence**: Add `[[wikilink]]` in body text
- **Subsequent occurrences**: Plain text, no link
- **Related Concepts section**: Grouped by topic at bottom

**Validation**: Every `[[wikilink]]` must point to an existing file. No dangling links allowed.

## How to Use

### Via Claude Code

```
/bright-book-breakdown {Book Title}+{specific request}
```

Examples:
```
/bright-book-breakdown 《Thinking, Fast and Slow》
/bright-book-breakdown 《合同起草审查指南》+rewrite as narrative
/拆书 《企业合规指南》+Batch Ingest
```

### Integrate into Your Own Skills

1. Download `SKILL.md`
2. Place it in your Claude Code skills directory
3. Restart Claude Code

## Three-Round Cognitive Compression

| Round | Goal | Question Answered |
|-------|------|-------------------|
| Skeleton Scan | Build global structure | "What is this book about" |
| Deep Dissection | Understand argument chain | "Why does the author say this" |
| Soul Extraction | Go beyond the author | "What else can I do with this" |

## Comparison with Other Methods

| Method | Characteristics | Difference |
|--------|----------------|------------|
| Copy-paste notes | Transcribing passages | This skill extracts structure, not copy |
| Mind map | Tree-like diverging | This skill has narrative logic + critical analysis |
| Zettelkasten | Atomic cards | This skill emphasizes first-occurrence linking, forming a network |

## Dependencies

- [Claude Code](https://claude.ai/code)
- Obsidian (optional, for concept network)
- PyMuPDF / pdfplumber (text extraction)
- PaddleOCR (fallback for scanned PDFs)

## File Structure

```
bright-book-breakdown/
├── SKILL.md          # Skill definition (core)
├── README.md         # Chinese README
├── README_EN.md     # English README
└── examples/        # Example files
    ├── INDEX.md     # Example guide
    ├── 精益生产.md  # Concept page example
    └── 失去的制造业.md  # Reading notes example
```

## Examples

Two complete examples showing the full flow from reading to concept pages:

| File | Type | Description |
|------|------|-------------|
| [examples/失去的制造业.md](examples/失去的制造业.md) | Reading Notes | Narrative layer + Analysis layer |
| [examples/精益生产.md](examples/精益生产.md) | Concept Page | Standard format, reusable |

## Acknowledgments

Standing on the shoulders of giants, this skill draws inspiration from:

- [LLM Wiki](https://github.com/karpathy/llm-utils) by @karpathy — LLM + knowledge base integration
- [Im-wiki-obsidian-blink](https://github.com/iBlinkQ/Im-wiki-obsidian-blink) by @iBlinkQ — Obsidian + Claude Code integration
- [claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian) by @AgriciDaniel — Claude and Obsidian integration

## License

MIT