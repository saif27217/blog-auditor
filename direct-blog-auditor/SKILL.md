---
name: direct-blog-auditor
description: >
  Audit and correct blog content directly in Google Docs using replaceAllText.
  Reviews through doctor/lab-physician lens. Applies minimal changes (grammar,
  medical accuracy, promotional language) directly to the source document without
  rewriting the whole article. Generates exact before/after change list with reasons.
category: blog-auditor
tags: [medical, blog, review, google-docs, composio, doctor-lens]
---

# Direct Blog Auditor

Audit blog content and apply corrections **directly in the source Google Doc** using `GOOGLEDOCS_REPLACE_ALL_TEXT`. No full rewrite — minimal, targeted edits with exact before/after change tracking.

## When to use

- User asks to review and correct a blog in a Google Doc
- User wants changes applied directly to the source document
- User wants an exact change list with before/after terms and reasons
- Doctor/lab-physician review lens is requested

## Workflow

### 1. Fetch source content

```python
# Extract raw text from Google Doc
url = f"https://docs.google.com/document/d/{DOC_ID}/export?format=txt"
# Use web_extract or Composio GOOGLEDOCS_GET_DOCUMENT_BY_ID
```

### 2. Review through doctor/lab-physician lens

Scan for:

**Medical accuracy:**
- Incorrect mechanisms (e.g., "lithium increases production" vs "shifts set-point")
- Wrong food classifications (e.g., chicken/fish as "calcium-rich")
- Misleading alternatives (e.g., "carrots are calcium-free")
- Incorrect clinical terms (e.g., "renal inflammation" vs "chronic kidney disease")
- Missing mnemonics or frameworks (e.g., "Psychic Moans" in Stones/Bones/Groans)
- Reversed meanings (e.g., "remembering things longer" when should be "difficulty remembering")

**Promotional language:**
- "One such provider is..."
- "200 branches in 25+ cities"
- "At your earliest convenience"
- Excessive self-promotion

**Grammar/style:**
- Sentence fragments
- Filler phrases ("On that note", "In this regard", "Let's have a quick look")
- Comma splices
- Tense errors
- British/American spelling inconsistency

**Structural:**
- Heading hierarchy (single H1, proper H2/H3 nesting)
- Broken image stubs → `[IMAGE: TBD]`
- Tracking URLs → clean links

### 3. Build change list

Create exact before/after pairs:

```python
changes = [
    ("exact original text", "corrected text"),
    # ...
]
```

### 4. Apply via GOOGLEDOCS_REPLACE_ALL_TEXT

Execute in batches of 10 via `COMPOSIO_MULTI_EXECUTE_TOOL`:

```python
# For each change:
{
    "tool_slug": "GOOGLEDOCS_REPLACE_ALL_TEXT",
    "arguments": {
        "document_id": DOC_ID,
        "find_text": "exact original text",
        "replace_text": "corrected text",
        "match_case": True
    }
}
```

**Critical rules:**
- Always use `match_case: true`
- Execute in batches of 10 (API limit)
- Changes execute sequentially (each shifts indices)
- Text must match EXACTLY (including punctuation, apostrophes)
- Process bottom-to-top for index-based operations (not needed for replaceAllText)

### 5. Generate change list document

Create a new Google Doc with the change list:

```markdown
# {Blog Title} — Exact Change Terms with Reasons

## Changes
- [CHANGE] Short description
  - Before: `exact old text`
  - After: `exact new text`
  - Reason: why the edit was made.

## Editorial Review
### Derogatory or improper terms
- None found.

### Incorrect or inaccurate statements
1. [description] — **Corrected.**

### Repeated statements
- None found.

### Grammar / style notes
- [note] — **Corrected.** (or "retained" if not edited)
```

### 6. Publish change list

```python
# Via Composio MCP
GOOGLEDOCS_CREATE_DOCUMENT_MARKDOWN:
    title: "{Blog Title} — Exact Change Terms with Reasons"
    markdown_text: change_list_content
```

## API approach notes

| Approach | When to use |
|---|---|
| `GOOGLEDOCS_REPLACE_ALL_TEXT` | Direct edits to source doc (preferred) |
| `GOOGLEDOCS_CREATE_DOCUMENT_MARKDOWN` | Publish change list as new doc |
| `GOOGLEDOCS_UPDATE_DOCUMENT_MARKDOWN` | Full rewrite (only if user requests) |

## Limitations

- **No colored tracked changes via API**: Google Docs API cannot create red/blue tracked changes programmatically. Use Suggesting mode manually if needed.
- **replaceAllText replaces directly**: Old text is gone after replacement. Change list document serves as the audit trail.
- **Exact text matching required**: Apostrophes, em-dashes, and special characters must match exactly.

## Pitfalls

- Always verify the source text matches before executing replacements
- Some Google Docs have keyword tables or meta sections at the top — skip those
- Image stubs (`![](<Base64-Image-Removed>)`) cannot be replaced via replaceAllText
- Large replacements may hit API limits — split into batches of 10

## Dependencies

- Composio MCP with Google Docs connection active
- `GOOGLEDOCS_REPLACE_ALL_TEXT`
- `GOOGLEDOCS_CREATE_DOCUMENT_MARKDOWN`
- `GOOGLEDOCS_GET_DOCUMENT_BY_ID` (for structure verification)

## Example

See `examples/hypercalcemia-direct-audit.md` for a complete worked example.
