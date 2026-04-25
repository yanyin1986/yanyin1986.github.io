# Wiki Schema

## Domain
日语学习知识库：用于持续整理日语词汇、语法、表达、听说读写训练方法、教材笔记、文化背景、考试准备与学习策略。

## Conventions
- File names: lowercase, hyphens, no spaces (e.g., `te-form.md`)
- Every wiki page starts with YAML frontmatter
- Use `[[wikilinks]]` to link between pages (minimum 2 outbound links per page)
- When updating a page, always bump the `updated` date
- Every new page must be added to `index.md` under the correct section
- Every action must be appended to `log.md`
- On pages synthesizing 3+ sources, append provenance markers like `^[raw/articles/source-file.md]` to sourced paragraphs
- Prefer compact, reusable notes over long textbook-style prose
- Use examples generously, but keep each page focused on one entity or concept

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
confidence: high | medium | low
contested: true
contradictions: [other-page-slug]
---
```

Notes:
- `confidence`, `contested`, and `contradictions` are optional.
- Use `confidence: medium` or `low` for single-source notes, evolving interpretations, or subjective usage guidance.
- `sources` may be empty (`[]`) for seed pages or hand-written study summaries.

## raw/ Frontmatter
Raw sources should include:

```yaml
---
source_url: https://example.com/article
ingested: YYYY-MM-DD
sha256: <hex digest of body>
---
```

## Tag Taxonomy
Add new tags here before using them on pages.

- Language Areas: vocabulary, grammar, kanji, reading, listening, speaking, writing, pronunciation
- Learning Scope: beginner, intermediate, advanced, jlpt, conversation, immersion
- Knowledge Types: pattern, example, checklist, reference, comparison, note
- Context: textbook, media, culture, nuance, mistake, strategy
- Focus: verb, adjective, particle, sentence, expression, honorific

Rule: every tag on a page must appear in this taxonomy.

## Page Thresholds
- Create a page when a grammar point, expression, study pattern, resource, or recurring mistake is central to one source or appears in 2+ sources
- Add to existing pages when new material expands an existing grammar point, word family, or learning strategy
- Do not create pages for passing mentions or isolated vocabulary with no reusable value
- Split a page when it exceeds ~200 lines
- Archive a page when fully superseded, moving it to `_archive/`

## Entity Pages
Use `entities/` for notable textbooks, dictionaries, tools, media sources, exams, apps, or named language resources.
Include:
- What it is
- Why it matters for learning
- Relationships to other pages via [[wikilinks]]
- Relevant sources and caveats

## Concept Pages
Use `concepts/` for grammar points, pronunciation rules, study methods, error patterns, expression families, and language-learning techniques.
Include:
- Definition / explanation
- Core usage or learning guidance
- Common pitfalls or nuance
- Related concepts via [[wikilinks]]

## Comparison Pages
Use `comparisons/` for comparing grammar patterns, similar expressions, textbooks, apps, or study strategies.
Include:
- What is being compared and why
- Dimensions of comparison (table preferred)
- Recommendation or synthesis
- Sources

## Query Pages
Use `queries/` for substantial answers worth preserving, such as deep dives into confusing grammar contrasts, study plans, or curated resource maps.

## Update Policy
When new information conflicts with existing content:
1. Check dates and source quality; newer or more authoritative sources usually supersede older advice
2. If both positions remain context-dependent, document the context explicitly
3. Mark contradictions in frontmatter when unresolved
4. Flag the issue in future lint results
