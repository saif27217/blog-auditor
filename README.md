# blog-auditor
Hermes skill: audit + rewrite blog posts for medical/healthcare website display (VDC and similar).

## What it does
- Checks heading hierarchy, evidence language, tone, India relevance, links, alt text, FAQs
- Rewrites in parallel chunks, assembles clean markdown
- Publishes to Google Doc via Composio MCP by default
- Supports minimal-change mode, inline annotated mode, and exact-change-list mode
- Falls back to local markdown if MCP unreachable

## Install
Put `SKILL.md` under `~/.hermes/skills/blog-auditor/`.

## Modes
- Standard rewrite — full rewrite with hierarchy, evidence framing, India relevance, tone, links, images, FAQs
- Minimal-change mode — smallest necessary edits only; sentence rewrites are prohibited unless needed for correctness
- Inline annotated mode — source text preserved with `[CHANGE: detail]` markers next to edits
- Exact-change-list mode — only the changes with before/after/reason in bullet format, no article body

## Outputs
- Clean publication markdown
- Change log with reasons
- Published Google Doc when Composio is available

## Example artifacts
See `examples/exact-change-list.md` and `examples/minimal-edit-changelog.md`.
