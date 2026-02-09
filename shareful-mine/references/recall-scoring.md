# Recall Scoring

Quality signal detection for mining the best share candidates from conversation history. Scores candidates on breakthrough signals, problem quality, and community value.

## Contents

- [Quality Signal Searches](#quality-signal-searches)
- [Scoring Rubric](#scoring-rubric)
- [Difficulty Tiers](#difficulty-tiers)
- [The Stack Overflow Quality Model](#the-stack-overflow-quality-model)
- [Scoring Walkthrough](#scoring-walkthrough)

## Quality Signal Searches

Run these searches alongside the standard error and framework searches. They surface conversations where hard problems were solved.

### Breakthrough Markers

Search for user prompts that express relief, gratitude, or excitement after solving something difficult. These indicate the preceding conversation contains a valuable fix.

```bash
ccs -s "finally working" -j -n 20
ccs -s "that fixed it" -j -n 20
ccs -s "that worked" -j -n 20
ccs -s "genius" -j -n 20
ccs -s "exactly what I needed" -j -n 20
ccs -s "nailed it" -j -n 20
```

Important: these prompts themselves are not the share content. The value is in the conversation BEFORE and AROUND these prompts -- the problem description, the failed attempts, and the working solution. When you find a breakthrough marker, examine the surrounding conversation context for the actual problem and fix.

### Difficulty Indicators

Search for prompts suggesting the user was deep in a debugging session -- multiple attempts, frustration, long context.

```bash
ccs -s "still not working" -j -n 20
ccs -s "tried everything" -j -n 20
ccs -s "tried that" -j -n 20
ccs -s "hours debugging" -j -n 20
ccs -s "days trying" -j -n 20
ccs -s "keeps failing" -j -n 20
ccs -s "another attempt" -j -n 20
ccs -s "why is" -j -n 20
ccs -s "what am I missing" -j -n 20
ccs -s "I don't understand why" -j -n 20
```

### Integration and Architecture Searches

Problems involving multiple systems interacting are typically harder and more valuable.

```bash
ccs -s "integration" -j -n 20
ccs -s "conflict" -j -n 20
ccs -s "incompatible" -j -n 20
ccs -s "version mismatch" -j -n 20
```

## Scoring Rubric

Score each candidate on three dimensions. Total range: 0-8 points.

### Breakthrough Signal (0-2 points)

| Score | Criteria |
|-------|----------|
| 0 | No breakthrough language found; standard request/response |
| 1 | Mild positive signal ("thanks", "that worked") |
| 2 | Strong breakthrough signal ("finally working after hours", "been stuck on this for days", "tried 5 different approaches") |

### Problem Quality (0-3 points)

| Score | Criteria |
|-------|----------|
| 0 | Vague problem, no error message, no reproduction steps |
| 1 | Has an error message OR technology context, but not both |
| 2 | Specific error message + technology context + the conversation resolved it |
| 3 | Specific error + minimal reproduction + root cause identified + multiple failed approaches documented |

### Community Value (0-3 points)

| Score | Criteria |
|-------|----------|
| 0 | Entirely project-specific; no other developer would hit this |
| 1 | Somewhat generalizable but narrow audience (obscure library, rare config) |
| 2 | Framework-level issue that many developers using that stack would encounter |
| 3 | Cross-framework or ecosystem-wide issue (migration, breaking change, common pattern interaction) + the error message is searchable |

### Score Interpretation

| Total Score | Action |
|-------------|--------|
| 6-8 | High priority -- create a share immediately |
| 4-5 | Good candidate -- create if time allows |
| 2-3 | Marginal -- skip unless nothing better exists |
| 0-1 | Skip |

## Difficulty Tiers

Classify problems by difficulty to prioritize harder ones. Harder problems produce more valuable shares because developers spend more time searching for solutions.

| Tier | Description | Examples | Share Priority |
|------|-------------|----------|---------------|
| **Tier 1: Trivial** | Typos, missing imports, wrong file path | Missing semicolon, forgot to install package | Low -- skip unless the error message is exceptionally misleading |
| **Tier 2: Standard** | Single-cause bugs with clear error messages | Null reference, wrong argument type, missing config key | Medium -- share if the error message is common and the fix is non-obvious |
| **Tier 3: Complex** | Multi-step debugging, interaction between systems | Race conditions, hydration mismatches, build tool conflicts | High -- these are where developers get stuck |
| **Tier 4: Architectural** | Requires understanding system design to resolve | N+1 queries, memory leaks, auth flow redesigns, migration strategies | Highest -- these save developers hours |

Focus mining effort on Tier 3 and Tier 4. Tier 2 is acceptable if the error message is highly searchable and the fix is surprising.

## The Stack Overflow Quality Model

The best shares mirror what made great Stack Overflow content. Use this as a mental model when evaluating candidates.

**What made great SO questions (maps to Problem section):**
- Specific error message with exact text
- Minimal reproducible example (not an entire project)
- Version and environment details
- Evidence the author tried to solve it themselves

**What made great SO answers (maps to Solution + Why It Works sections):**
- Working code that directly addresses the question
- Explains WHY, not just WHAT -- the root cause mechanism
- Addresses edge cases and common variations
- Appropriate scope -- not a tutorial, not a one-liner

**The shareable sweet spot:**
- Hard enough to be worth sharing (Tier 2+ difficulty)
- Common enough that others will hit it (community value 2+)
- Specific enough to be findable via search (contains a searchable error string or technology + symptom combination)
- Resolved cleanly enough to explain (problem quality 2+)

A candidate in the shareable sweet spot typically scores 5+ on the rubric.

## Scoring Walkthrough

### Example A: High Score (7/8)

Prompt found via `ccs -s "finally working"`:
> "The Tailwind v4 migration finally working -- the @custom-variant nesting error was because PostCSS was processing Tailwind directives before the new engine. Had to restructure the entire PostCSS config."

- Breakthrough: 2 (strong -- "finally working", implies long struggle)
- Problem quality: 3 (specific error -- @custom-variant nesting, clear root cause -- PostCSS ordering, migration context)
- Community value: 2 (Tailwind v3-to-v4 migration, many developers hit this)
- **Total: 7** -- create this share immediately

### Example B: Low Score (2/8)

Prompt found via `ccs -s "fix"`:
> "Can you fix the import path? It should be ../components not ./components"

- Breakthrough: 0 (no signal)
- Problem quality: 1 (has the symptom but it is trivial)
- Community value: 1 (too project-specific, not a generalizable issue)
- **Total: 2** -- skip
