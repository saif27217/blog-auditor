# blog-auditor
Hermes skills for auditing and correcting blog posts for medical/healthcare website display (VDC and similar).

## Skills

### blog-auditor (full rewrite)
Audit + rewrite blog posts with heading hierarchy, evidence language, tone, India relevance, links, alt text, FAQs.

**Modes:**
- Standard rewrite — full rewrite with hierarchy, evidence framing, India relevance, tone, links, images, FAQs
- Minimal-change mode — smallest necessary edits only; sentence rewrites prohibited unless needed for correctness
- Inline annotated mode — source text preserved with `[CHANGE: detail]` markers next to edits
- Exact-change-list mode — only the changes with before/after/reason in bullet format, no article body

**Install:** `~/.hermes/skills/blog-auditor/SKILL.md`

### direct-blog-auditor (apply directly)
Audit and correct blog content **directly in the source Google Doc** using `GOOGLEDOCS_REPLACE_ALL_TEXT`. No full rewrite — minimal, targeted edits with exact before/after change tracking.

**When to use:**
- User wants changes applied directly to the source document
- Doctor/lab-physician review lens requested
- Exact change list with before/after terms needed

**Workflow:**
1. Fetch source from Google Doc
2. Review through doctor/lab-physician lens
3. Build exact before/after change pairs
4. Apply via `GOOGLEDOCS_REPLACE_ALL_TEXT` (batches of 10)
5. Publish change list as new Google Doc

**Install:** `~/.hermes/skills/direct-blog-auditor/SKILL.md`

**Limitations:**
- No colored tracked changes via API (use Suggesting mode manually)
- `replaceAllText` replaces directly — old text is gone
- Exact text matching required

## Outputs
- Clean publication markdown
- Change log with reasons (exact before/after terms)
- Published Google Docs via Composio MCP

## Example artifacts
- `examples/exact-change-list.md` — change list format
- `examples/minimal-edit-changelog.md` — minimal edit changelog
- `examples/hypercalcemia-direct-audit.md` — direct audit example
