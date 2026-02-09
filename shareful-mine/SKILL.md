---
name: shareful-mine
description: Mines past Claude Code conversations for shareable coding solutions using claude-code-search (ccs). Searches conversation history for errors, fixes, and workarounds, then creates SHARE.md files following shareful.ai conventions. Use when the user wants to "mine conversations for shares", "populate my shares repo", "find problems to share", or "solve the cold start problem".
---

# Shareful Mine

Mine past Claude Code conversations for problems worth sharing on shareful.ai. This solves the cold start problem -- every developer already solves real problems daily, and those solutions are sitting in conversation history waiting to be shared.

## When to Use This Skill

Use this skill when the user:

- Wants to populate their shares repo from past work
- Asks "what problems have I solved that others would hit?"
- Wants to mine their Claude Code history for shareable solutions
- Mentions the cold start problem for shareful.ai
- Wants to batch-create shares from real problems

## Prerequisites

Install claude-code-search globally:

```bash
npm install -g claude-code-search
```

Verify installation:

```bash
ccs --help
```

The user must also have a shares repo set up. If not, run `npx shareful-ai init` first.

## Reference Files

| File | Read When |
|------|-----------|
| [references/ccs-commands.md](references/ccs-commands.md) | Looking up ccs CLI syntax, flags, or output format |
| [references/search-strategies.md](references/search-strategies.md) | Choosing what to search for and evaluating candidates |

## Mining Workflow

Copy this checklist to track progress:

```text
- [ ] Step 1: Check existing shares to avoid duplicates
- [ ] Step 2: Run discovery searches with ccs
- [ ] Step 3: Filter and rank candidates
- [ ] Step 4: Create SHARE.md files
- [ ] Step 5: Validate with npx shareful-ai check
```

### Step 1: Check Existing Shares

Before mining, list what already exists to avoid duplicates:

```bash
ls shares/
```

### Step 2: Run Discovery Searches

Run multiple targeted searches to surface problems from conversation history. Use JSON output for programmatic processing:

```bash
# Error-focused searches
ccs -s "error" -j -n 30
ccs -s "fix" -j -n 30
ccs -s "TypeError" -j -n 20
ccs -s "cannot find" -j -n 20
ccs -s "failed" -j -n 20

# Framework-specific searches
ccs -s "nextjs" -j -n 20
ccs -s "tailwind" -j -n 20
ccs -s "prisma" -j -n 20
ccs -s "react" -j -n 20
ccs -s "typescript" -j -n 20
```

Read [references/search-strategies.md](references/search-strategies.md) for the full list of recommended searches and how to evaluate results.

### Step 3: Filter and Rank Candidates

From the search results, identify prompts that describe specific, reproducible problems. A good candidate has:

1. **A specific error message or symptom** -- not "help me with my code"
2. **A clear resolution** -- the conversation solved it
3. **Community value** -- other developers would encounter this
4. **Generalizability** -- the fix is not specific to one project's business logic
5. **Novelty** -- not already covered by an existing share

Discard prompts that are:
- Generic requests ("review my code", "test this", "explain this")
- Project-specific business logic with no general applicability
- Simple typos or one-character fixes
- Incomplete -- the problem was described but never resolved

### Step 4: Create SHARE.md Files

For each candidate, create a share following the `shareful-create` workflow. Write directly to `shares/<slug>/SHARE.md` with complete frontmatter and all four required sections.

Each share needs:
- YAML frontmatter (title, slug, tags, problem, solution_type, created, environment)
- `## Problem` -- the error message and broken code from the conversation
- `## Solution` -- the fix that resolved it
- `## Why It Works` -- explanation of the root cause
- `## Context` -- versions, gotchas, related tools

When creating multiple shares, parallelize by assigning batches to subagents. Each subagent writes to separate directories so there are no file conflicts.

### Step 5: Validate

Run the shareful checker to validate all shares:

```bash
npx shareful-ai check
```

All shares must pass validation before committing. Fix any errors flagged by the checker.

## Candidate Evaluation Criteria

| Signal | Good Candidate | Bad Candidate |
|--------|---------------|---------------|
| Error specificity | `TypeError: Cannot read properties of undefined` | "something broke" |
| Technology scope | Framework/library issue any user could hit | App-specific business logic |
| Fix clarity | Clear code change that resolves the error | Vague "try restarting" |
| Frequency | Common framework gotcha or migration issue | One-off environment quirk |
| Searchability | Error message is Google-able | No distinctive error string |

## Anti-patterns

- **Mining private/sensitive content** -- never include API keys, credentials, internal URLs, or proprietary business logic in shares
- **Sharing unresolved problems** -- only share problems that were actually fixed
- **Over-mining** -- focus on quality over quantity; 10 great shares beat 50 mediocre ones
- **Duplicating existing shares** -- always check the repo first
- **Vague problems** -- if you cannot write a specific error message for the Problem section, skip it

## Related Skills

- `shareful-create` for the detailed SHARE.md writing workflow and validation
- `shareful-search` for checking if a solution already exists on shareful.ai
- `shareful-init` for setting up a shares repository
