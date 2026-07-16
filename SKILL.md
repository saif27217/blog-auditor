---
name: blog-auditor
description: >
  Audit blog content for website display. Rewrites for consistent heading hierarchy,
  direct evidence-based language, India-relevant medical context, patient-friendly
  tone, clean link hygiene, explicit image-placeholder alt text, and factual accuracy.
  Also supports minimal-change mode, inline change annotations, and a standalone
  change list. Use when asked to: review blog copy, fix blog formatting, prepare
  medical blog for website, audit content for VDC/health website.
version: 1.1.0
author: Lazer
license: MIT
platforms: [linux]
metadata:
  hermes:
    tags: [content, blog, medical, web, audit, rewrite, india, healthcare]
    related_skills: [composio-mcp, scientific-writing, content]
---

# Blog Auditor

Audit + rewrite blog posts for medical/healthcare website display.

## When to use

- "review this blog for our website"
- "audit blog content"
- "fix blog formatting/headings"
- "prepare medical blog for site"
- "make minimal changes"
- "mention all changes beside each change"
- "add changelog for derogatory/incorrect/repeated statements"
- source is a Google Doc, markdown file, or pasted text

## Modes

### Standard rewrite
Full rewrite for consistent hierarchy, evidence framing, India relevance, tone, links, images, and FAQs.

### Minimal-change mode
Use when user asks for minimal changes. Rules:
- Do not rewrite paragraphs or sentences unless correctness demands it.
- Do not change headings unless hierarchy compliance requires it.
- Make the smallest possible change to satisfy the requirement.
- Prefer single-token replacements over sentence rewrites.
- Keep observational qualifiers unless removal is required for accuracy.
- After edits, diff against original and ensure added `+` lines are fewer than 20 unless user explicitly authorizes broader changes.

Allowed minimal changes:
- H1/H2/H3 level fixes only
- Single-token replacements: raw URL → clean link text; image stubs → `[IMAGE: TBD]`
- Removing decorative rules (`---`) and excess blank lines
- Correcting obvious grammar/typo defects in place
- Removing exact duplicate sentences or duplicate list items

### Inline annotated mode
When user wants change location clearly visible:
- Append bracketed marker on same or next line: `[CHANGE: detail]`
- For changelog-only request, produce a concise change list with `[CHANGE]` tags, no article body.

### Changelog detail mode
When user requests a detailed change log, include:
- Derogatory or improper terms
- Incorrect or inaccurate statements
- Repeated statements
- Minor grammar / style notes marked `[NOTE: ...]` without applying them automatically

## Workflow

### 1. Ingest
Accept blog content from:
- Google Doc share URL
- Local markdown/text file path
- Pasted text in message

If Google Doc: fetch export text immediately:
```
https://docs.google.com/document/d/{id}/export?format=txt
```

### 2. Context confirm
Ask only if missing:
- Target platform / CMS (VDC, WordPress, etc.)
- Mode preference: standard / minimal / annotated / change-list only
- Special sections to preserve (FAQs, test info, citations)

Do not ask questions about workflow mechanics.

### 3. Audit checklist
Run every item. Mark pass/fail.

| Check | Rule | Fix |
|-------|------|-----|
| H1 count | Exactly one `# Title` | Remove extras, promote/demote |
| H2/H3/H4 order | No skipped levels | `## → ### → ####` |
| Evidence hedging | Strip hedging when source states facts directly | State directly if source supports |
| Promotional tone | Remove fluff, superlatives, "comprehensive" | Neutral medical prose |
| India relevance | Keep explicit India lines, doses, diseases | Do not nuke regional context |
| Link hygiene | No raw URLs; descriptive text only | Replace inline URL fragments |
| Patient friendly | Short sentences, define terms, avoid jargon | Keep "adipokine" if needed; explain once |
| Factual accuracy | Numbers, units, mechanisms match source | Verify against source before change |
| Image placeholders | Replace Base64/removed with `[IMAGE: concise alt text]` or `[IMAGE: TBD]` per mode | Alt text describes concept/placement |
| FAQ structure | H3 questions, crisp H3→body answers | No hedging, no filler intro |
| Derogatory/improper terms | No insensitive or colloquial terms | Flag in changelog |
| Repeated statements | No identical body sentences twice | Remove duplicates or mark intentional |
| Grammar/style notes | Log preferred corrections as notes | Do not auto-apply in minimal mode |

### 4. Chunk + rewrite

For large posts in standard mode, split into 3 chunks and run in parallel via `delegate_task`:
- Chunk A: title through functions / key mechanisms
- Chunk B: production, normal ranges, interpretation, lifestyle
- Chunk C: lab test info, FAQs, image placeholders

Assemble into single markdown:
- One H1 only
- Consistent blank line around headings
- No horizontal rules / decorative dividers

### 5. Annotate changes

For inline annotated mode:
- Insert `[CHANGE: detail]` adjacent to each modified line
- Preserve non-changed lines verbatim from source
- Add disclaimer header: "Only lines tagged with [CHANGE: ...] were modified."

### 6. Deliver

Default deliverable is a **single published Google Doc** created via `GOOGLEDOCS_CREATE_DOCUMENT_MARKDOWN`.

Upload path:
1. Assemble final `/home/sak/{slug}-vdc-final.md` or minimal/annotated variant
2. Run direct Composio MCP JSON-RPC script (see below) against `https://connect.composio.dev/mcp`
3. Verify response includes `documentId` + `display_url`
4. Share doc link to user

If Composio unreachable, offer local markdown file as fallback.

## Quick Caveman Mode

When invoked with caveman intent: audit checks stay same. Output stays markdown. Explanations get compressed only in user-facing text.

## Dependencies

- `composio-mcp` skill for Drive/Docs actions
- OAuth token at `~/.hermes/mcp-tokens/composio.json`

## References

- `references/heading-hierarchy.md` — heading rules by platform
- `references/evidence-hedging.md` — phrases to strip / keep
- `references/minimal-change-mode.md` — allowed/prohibited edits in minimal mode
- `references/editorial-review.md` — derogatory terms, accuracy, repeats, style notes

## Examples

- `examples/minimal-edit-changelog.md` — change-list-only format

## Additional notes

### High-value use case: batch-audit pipeline
Input: 10 blog docs → output: 3 artifacts per post:
- clean markdown
- change log
- annotated-track-changes doc

This should be exercised end-to-end with another healthcare-style post and packaged as a repeatable pattern.
