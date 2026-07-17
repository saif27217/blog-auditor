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

## Review Factors

All changes are evaluated through these factors:

### Medical accuracy (doctor/lab-physician lens)
1. Physiological mechanisms — correct pathways (e.g., lithium shifts CaSR set-point, not "increases production")
2. Food classifications — verify claims (e.g., chicken/fish are NOT calcium-rich)
3. Alternative recommendations — avoid misleading suggestions (e.g., carrots are NOT calcium-free)
4. Clinical terminology — use standard terms (e.g., "chronic kidney disease" not "renal inflammation")
5. Pathophysiology completeness — include missing mechanisms (e.g., macrophage calcitriol in sarcoidosis)
6. Mnemonics/frameworks — include standard clinical tools (e.g., "Psychic Moans" in Stones/Bones/Groans)
7. Meaning accuracy — verify statements aren't reversed (e.g., "difficulty remembering" not "remembering longer")
8. Pathway mechanisms — correct molecular/cellular pathways (e.g., PTHrP in cancer)
9. Terminology standardization — consistent spelling/formatting (e.g., "milk-alkali syndrome" hyphenated)

### Clinical precision
10. Qualifier language — add "potentially" for non-universal outcomes
11. Unit/concept accuracy — "concentration" not "volume" for minerals
12. Standard terminology — "gut motility" not "digestive signalling"
13. Precise language — "circulating fluid" not "essential fluids"
14. Context accuracy — "clinical context" not "severity of inflammation" for non-inflammatory conditions

### Promotional language
15. Remove specific marketing claims (branch counts, city counts)
16. Remove self-referential promotion ("One such provider is...")
17. Remove urgency language ("At your earliest convenience")
18. Remove direct booking prompts ("book an appointment with a doctor")

### Grammar/style
19. Fix sentence fragments
20. Correct comma splices
21. Fix tense errors
22. Standardize spelling (British/American)
23. Consistent formatting (lowercase bullets, heading case)

### Structural
24. Image stubs → `[IMAGE: TBD]`
25. Tracking URLs → clean links
26. Heading hierarchy maintained

## Outputs
- Clean publication markdown
- Change log with reasons (exact before/after terms)
- Published Google Docs via Composio MCP

## Example artifacts
- `examples/exact-change-list.md` — change list format
- `examples/minimal-edit-changelog.md` — minimal edit changelog
- `examples/hypercalcemia-direct-audit.md` — direct audit example
