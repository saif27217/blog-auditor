---
name: blog-auditor
description: >
  Audit blog content for website display. Rewrites for consistent heading hierarchy,
  direct evidence-based language, India-relevant medical context, patient-friendly
  tone, clean link hygiene, explicit image-placeholder alt text, and factual accuracy.
  Use when asked to: review blog copy, fix blog formatting, prepare medical blog for
  website, audit content for VDC/health website.
version: 1.0.0
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
- source is a Google Doc, markdown file, or pasted text

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
- Brand tone defaults (keep provided, no AI enthusiasm)
- Special sections to preserve (FAQs, test info, citations)

Do not ask questions about workflow mechanics.

### 3. Audit checklist

Run every item. Mark pass/fail.

| Check | Rule | Fix |
|-------|------|-----|
| H1 count | Exactly one `# Title` | Remove extras, promote/ demote |
| H2/H3/H4 order | No skipped levels | `## → ### → ####` |
| Evidence hedging | Strip may/often shows/is studied for when not needed | State directly if source supports |
| Promotional tone | Remove fluff, superlatives, "comprehensive" | Neutral medical prose |
| India relevance | Keep explicit India lines, doses, diseases | Do not nuke regional context |
| Link hygiene | No raw URLs; descriptive text only | Replace inline URL fragments |
| Patient friendly | Short sentences, define terms, avoid jargon | Keep "adipokine" if needed; explain once |
| Factual accuracy | Numbers, units, mechanisms match source | Verify against source before change |
| Image placeholders | Replace Base64/removed with `[IMAGE: concise alt text]` | Alt text describes concept/placement |
| FAQ structure | H3 questions, crisp H3→body answers | No hedging, no filler intro |

### 4. Chunk + rewrite

For large posts, split into 3 chunks and run in parallel via `delegate_task`:
- Chunk A: title through functions / key mechanisms
- Chunk B: production, normal ranges, interpretation, lifestyle
- Chunk C: lab test info, FAQs, image placeholders

Assemble into single markdown:
- One H1 only
- Consistent blank line around headings
- No horizontal rules / decorative dividers

### 5. Deliver

Default deliverable is a **single published Google Doc** created via `GOOGLEDOCS_CREATE_DOCUMENT_MARKDOWN`.

Upload path:
1. Assemble final `/home/sak/{slug}-vdc-final.md`
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
