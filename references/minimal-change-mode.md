# Minimal-Change Mode

Use this mode when the user explicitly asks for minimal changes to the source content.

## Rules
- Do not rewrite paragraphs or sentences unless required for correctness.
- Do not change headings unless required for hierarchy compliance.
- Make the smallest possible change to satisfy the requirement.
- Prefer deleting/adding fewer than 10 words over sentence-level rewrites.
- Never insert new clinical claims or new India-specific context not present in source.
- Keep observational qualifiers if removing them would strengthen a claim beyond the source.

## Allowed minimal changes
- H1/H2/H3 level fixes only
- Single-token replacements: raw URL → clean link text; image stubs → `[IMAGE: TBD]`
- Removing decorative rules (`---`) and excess blank lines
- Correcting obvious grammar/typo defects in place
- Removing exact repeated sentences or duplicate list items

## Prohibited in minimal mode
- Sentence rewrites
- Heading rewrites
- Tone changes
- Adding India context not already in source
- FAQ trimming beyond removing duplicate sentences

## Verification
After edits, run a diff against original and ensure `+` lines are fewer than 20 unless user approves broader change.
