# Editorial Review Checklist

Run after every rewrite and again after minimal-change pass.

## Derogatory / improper terms
Scan for insensitive, colloquial, derogatory, or stigmatising language.
Flag examples with line number and term. Do not auto-fix without user direction.

## Incorrect / inaccurate statements
Verify:
- Units are consistent (`µg/mL`, not mixed units)
- Mechanism descriptions match source framing; do not upgrade observational claims to causal claims unless source does so
- No dosing or treatment recommendations beyond source wording
- No new disease assertions not in source

If inaccurate, replace with source-accurate wording and mark `[CHANGE: factual correction]`.

## Repeated statements
Detect exact duplicate sentences across the document.
Allowed duplicates:
- Intentional section headers
- Intentional `[IMAGE: TBD]` markers at distinct locations
Disallowed duplicates:
- Identical body sentences appearing twice

## Grammar / style notes
Do not auto-edit style preferences. Log them as notes with exact quote and suggested alternative, marked `[NOTE: ...]`.

## Output format
Summarise under these headings in the changelog:
- Derogatory or improper terms
- Incorrect or inaccurate statements
- Repeated statements
- Grammar / style notes (not edits)
