# Bright-Book-Breakdown

**中文** | [English](README.md)

Narrative Book Breakdown Workflow: turn a book into a **navigable knowledge network** with Obsidian.

## Core Use Case

This skill is designed for **Obsidian users** — output directly imports to your Obsidian vault, forming a web of connected knowledge.

```
PDF/EPUB  ──read──→  Reading notes (narrative + analysis)
                          │
                    Extract from book
                          │
                          ↓
                   Concept pages (wikilink connected)
                          │
                    Import to Obsidian
                          │
                          ↓
                   Navigable knowledge network
```

Click any `[[wikilink]]` to navigate, layer by layer. Concepts are never isolated.

## Three-Round Cognitive Compression

| Round | Goal | Question Answered |
|-------|------|-------------------|
| Skeleton Scan | Build global structure | "What is this book about" |
| Deep Dissection | Understand argument chain | "Why does the author say this" |
| Soul Extraction | Go beyond the author | "What else can I do with this" |

## Workflow

### Single Book Reading

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

Break down entire books at once, generating a concept network:

```
PDF/EPUB
  │
  ├── Text extraction (PyMuPDF/pdfplumber preferred, OCR as fallback)
  ├── Parallel agent breakdown (each chapter → 3-8 concept pages)
  └── Wikilink verification (dangling links → create concept page first)
```

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

## Text Extraction Rules

| Scenario | Tool | Notes |
|----------|------|-------|
| PDF with embedded text | PyMuPDF / pdfplumber | Direct extraction, fast and accurate |
| Garbled/misaligned text | pdfplumber as backup | Try different tool |
| Scanned PDF (no text layer) | PaddleOCR GPU | Render page-by-page + OCR |

**Fallback order**: PyMuPDF → pdfplumber → OCR

OCR is the last resort, not the default.

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

## Comparison with Other Methods

| Method | Characteristics | Difference |
|--------|----------------|------------|
| Copy-paste notes | Transcribing passages | This skill extracts structure, not copy |
| Mind map | Tree-like diverging | This skill has narrative logic + critical analysis |
| Zettelkasten | Atomic cards | This skill emphasizes first-occurrence linking, forming a network |

## Dependencies

- [Claude Code](https://claude.ai/code)
- [Obsidian](https://obsidian.md/) (core tool, for concept network)
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