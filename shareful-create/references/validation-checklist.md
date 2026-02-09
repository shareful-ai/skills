# Validation Checklist

Final validation checklist for `SHARE.md` quality.

## Frontmatter

- [ ] `title` is present, specific, and under 128 chars
- [ ] `slug` is kebab-case and under 64 chars
- [ ] `slug` matches directory name (`shares/<slug>/SHARE.md`)
- [ ] `tags` has 1-10 lowercase values, each under 32 chars
- [ ] `problem` is specific and under 256 chars
- [ ] `solution_type` is one of: `fix`, `workaround`, `pattern`, `reference`, `config`
- [ ] `created` uses `YYYY-MM-DD`

## Body Structure

- [ ] `## Problem` exists and shows the broken state
- [ ] `## Solution` exists and shows working code
- [ ] `## Why It Works` exists and explains mechanism
- [ ] `## Context` exists and lists requirements/gotchas

## Code Quality

- [ ] All code blocks include a language label
- [ ] Code snippets are complete enough to apply
- [ ] No placeholder comments remain (`TODO`, `<!-- ... -->`)

## Length and Clarity

- [ ] Body is under 300 lines
- [ ] Problem statement is concrete and searchable
- [ ] Why It Works explains root cause, not just steps
- [ ] Context bullets add non-duplicate details

## Final Gate

- [ ] Share is accurate for the user's stack and versions
- [ ] Share can be applied without guessing missing pieces
