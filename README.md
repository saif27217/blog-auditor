# blog-auditor
Hermes skill: audit + rewrite blog posts for medical/healthcare website display (VDC and similar).

## What it does
- Checks heading hierarchy, evidence language, tone, India relevance, links, alt text, FAQs
- Rewrites in parallel chunks, assembles clean markdown
- Publishes to Google Doc via Composio MCP by default
- Falls back to local markdown if MCP unreachable

## Install
Put `SKILL.md` under `~/.hermes/skills/blog-auditor/`.
