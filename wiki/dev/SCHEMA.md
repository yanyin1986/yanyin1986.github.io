# Wiki Schema

## Domain
日常开发笔记：用于持续积累软件开发相关的知识、实践、踩坑记录、工具经验、架构思考、调试方案与工作流总结。

## Conventions
- File names: lowercase, hyphens, no spaces (e.g., `python-debugging.md`)
- Every wiki page starts with YAML frontmatter
- Use `[[wikilinks]]` to link between pages (minimum 2 outbound links per page)
- When updating a page, always bump the `updated` date
- Every new page must be added to `index.md` under the correct section
- Every action must be appended to `log.md`
- On pages synthesizing 3+ sources, append provenance markers like `^[raw/articles/source-file.md]` to sourced paragraphs
- Prefer concise, reusable notes over long narrative dumps
- Store immutable source material only under `raw/`; synthesized knowledge belongs in wiki pages

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
- Use `confidence: medium` or `low` for single-source, evolving, or opinion-heavy notes.
- `sources` may be empty (`[]`) for hand-written seed pages, templates, or navigation notes.

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

- Languages: python, javascript, typescript, go, rust, shell
- Frameworks: frontend, backend, api, database, testing
- Infrastructure: devops, docker, kubernetes, ci-cd, cloud
- Engineering Work: debugging, performance, security, architecture, tooling, workflow
- Knowledge Types: pattern, incident, checklist, reference, comparison, note
- Scope: team, personal, project, learning

Rule: every tag on a page must appear in this taxonomy.

## Page Thresholds
- Create a page when a concept, tool, workflow, or recurring issue is central to one source or appears in 2+ sources
- Add to existing pages when new material extends an existing concept, tool, or incident pattern
- Do not create pages for passing mentions or one-off trivia
- Split a page when it exceeds ~200 lines
- Archive a page when fully superseded, moving it to `_archive/`

## Entity Pages
Use `entities/` for notable tools, libraries, frameworks, services, teams, or repositories.
Include:
- What it is
- Why it matters in the development workflow
- Relationships to other pages via [[wikilinks]]
- Relevant sources and caveats

## Concept Pages
Use `concepts/` for techniques, workflows, patterns, debugging methods, architecture ideas, and coding conventions.
Include:
- Definition / explanation
- Practical guidance
- Trade-offs or pitfalls
- Related concepts via [[wikilinks]]

## Comparison Pages
Use `comparisons/` for tool decisions, framework trade-offs, workflow alternatives, and architectural comparisons.
Include:
- What is being compared and why
- Dimensions of comparison (table preferred)
- Recommendation or synthesis
- Sources

## Query Pages
Use `queries/` only for non-trivial answers worth keeping, such as deep dives, decision records, or reusable troubleshooting synthesis.

## Update Policy
When new information conflicts with existing content:
1. Check dates; newer information usually supersedes older advice
2. If both positions are still relevant, document the context for each
3. Mark contradictions in frontmatter when unresolved
4. Flag the issue in future lint results
