# Search Strategies

Effective patterns for mining Claude Code conversations using ccs. Organized by search category with guidance on evaluating results.

## Contents

- [Recommended Search Sequence](#recommended-search-sequence)
- [Evaluating Results](#evaluating-results)
- [Prioritization](#prioritization)
- [Batch Processing](#batch-processing)


## Recommended Search Sequence

Run searches in this order for best coverage. Each category surfaces different types of shareable problems.

### 1. Error-Focused Searches

These find conversations where something broke. Highest hit rate for shareable fixes.

```bash
ccs -s "error" -j -n 30
ccs -s "fix" -j -n 30
ccs -s "bug" -j -n 20
ccs -s "TypeError" -j -n 20
ccs -s "cannot find" -j -n 20
ccs -s "failed" -j -n 20
ccs -s "not working" -j -n 20
ccs -s "module not found" -j -n 20
```

### 2. Framework-Specific Searches

These surface technology-specific issues that generalize well across projects.

```bash
ccs -s "nextjs" -j -n 20
ccs -s "tailwind" -j -n 20
ccs -s "prisma" -j -n 20
ccs -s "react" -j -n 20
ccs -s "typescript" -j -n 20
ccs -s "docker" -j -n 20
ccs -s "vercel" -j -n 20
ccs -s "eslint" -j -n 20
ccs -s "biome" -j -n 20
```

### 3. Problem-Type Searches

These find workarounds, migration issues, and configuration problems.

```bash
ccs -s "workaround" -j -n 20
ccs -s "breaking change" -j -n 20
ccs -s "deprecated" -j -n 20
ccs -s "migration" -j -n 20
ccs -s "config" -j -n 20
ccs -s "build" -j -n 20
ccs -s "deploy" -j -n 20
```

### 4. Specific Technical Searches

Use these when looking for particular problem categories.

```bash
ccs -s "hydration" -j -n 20
ccs -s "cors" -j -n 20
ccs -s "timeout" -j -n 20
ccs -s "memory" -j -n 20
ccs -s "performance" -j -n 20
ccs -s "cache" -j -n 20
ccs -s "auth" -j -n 20
ccs -s "websocket" -j -n 20
ccs -s "environment variable" -j -n 20
```

### 5. Quality Signal Searches

These find breakthrough moments and difficulty indicators. They surface conversations where the user solved a hard problem, which often produce the most valuable shares.

```bash
# Breakthrough markers
ccs -s "finally working" -j -n 20
ccs -s "that fixed it" -j -n 20
ccs -s "that worked" -j -n 20

# Difficulty indicators
ccs -s "still not working" -j -n 20
ccs -s "tried everything" -j -n 20
ccs -s "hours" -j -n 20
ccs -s "keeps failing" -j -n 20
ccs -s "why is" -j -n 20
```

When a quality signal search returns results, the prompt itself is a marker -- look at the surrounding conversation for the actual problem and fix to evaluate.

### 6. Full History Scan

When keyword searches miss things, scan recent prompts manually:

```bash
ccs -l -j -n 200
```

Look for prompts containing stack traces, error messages, or detailed problem descriptions.

## Evaluating Results

### Strong Candidates

A prompt is a strong share candidate when it contains:

- **An exact error message** with a unique, searchable string
- **Technology context** -- which framework, library, or tool
- **Evidence of resolution** -- the conversation led to a working fix
- **General applicability** -- any developer using that stack could hit this

Examples of strong candidates:

```text
"CssSyntaxError: tailwindcss: @custom-variant cannot be nested."
"sh: line 1: husky: command not found"
"TypeError: Invalid URL at new URL(process.env.NEXT_PUBLIC_APP_URL!)"
"NeonDbError: op ANY/ALL (array) requires array on right side"
```

### High-Value Candidates (Prioritize These)

A prompt is a high-value candidate when it shows:

- **Breakthrough language** -- "finally", "that fixed it", "been struggling with this"
- **Multiple failed attempts** -- references to things the user already tried
- **Cross-system interaction** -- problem involves two or more tools/libraries interacting
- **Migration or version context** -- upgrading from version X to Y

These prompts often score 5+ on the recall scoring rubric.

### Weak Candidates (Skip These)

- Generic prompts: "review my code", "test this", "help me"
- Prompts with no error message or specific symptom
- Business-logic-specific issues with no general applicability
- Simple typos or naming fixes
- Problems that were abandoned without resolution

## Prioritization

When you have many candidates, prioritize by:

1. **Breakthrough solutions** -- Conversations where the user expressed strong relief or gratitude after solving a hard problem. These had the highest time investment and produce the most valuable shares.
2. **Framework migration issues** -- Tailwind v3->v4, Prisma 6->7, Next.js upgrades. Many developers hit these simultaneously.
3. **Common deployment errors** -- CI/CD failures, Vercel/Docker issues. High search volume.
4. **Framework gotchas** -- hydration errors, CORS in dev, missing types. Developers hit these repeatedly.
5. **Security patterns** -- SSRF, XSS prevention. High impact when found.
6. **Configuration fixes** -- ESLint, TypeScript, build tool configs. Tedious to debug from scratch.

## Batch Processing

For efficient mining:

1. Run all searches first, collect results
2. Deduplicate -- the same problem often appears across multiple search queries
3. Group by technology for consistent share creation
4. Create shares in parallel batches of 3-5
5. Validate each batch with `npx shareful-ai check` before proceeding
