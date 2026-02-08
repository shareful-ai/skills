# Frontmatter Guide

Complete reference for SHARE.md YAML frontmatter fields. All validation constraints match the shareful CLI (`share-parser.ts`).

## Required Fields

### `title`

**Type:** string | **Max:** 128 characters

A human-readable title describing the solution. Start with a verb that matches the `solution_type`:

| solution_type | Verb convention | Example |
|---------------|-----------------|---------|
| `fix` | "Fix ..." | "Fix Prisma N+1 query problem with includes and joins" |
| `workaround` | "Workaround for ..." | "Workaround for Next.js 14 middleware redirect loop" |
| `pattern` | "Use ...", "Implement ..." | "Use TypeScript satisfies operator for type-safe config objects" |
| `reference` | "Guide to ...", "Reference for ..." | "Guide to PostgreSQL JSONB query operators" |
| `config` | "Configure ..." | "Configure ESLint flat config for TypeScript monorepos" |

Good titles:

```yaml
title: "Fix Docker build cache invalidation for node_modules"
title: "Fix Next.js hydration error with dynamic imports"
title: "Use TypeScript satisfies operator for type-safe config objects"
```

Bad titles:

```yaml
title: "Fix bug"                    # Too vague, no technology or symptom
title: "TypeScript"                 # Not a sentence, no verb, no problem
title: "How I fixed the thing that was broken in my app last Tuesday"  # Too long and informal
title: "fix prisma"                 # No capitalization, no specifics
```

### `slug`

**Type:** string | **Max:** 64 characters | **Pattern:** `/^[a-z0-9-]+$/`

A kebab-case identifier used as the directory name and URL path. The slug MUST match the parent directory name exactly.

**Generation algorithm** (from `generateSlug` in the CLI):

1. Convert to lowercase
2. Strip all characters that are not `a-z`, `0-9`, spaces, or hyphens
3. Replace spaces with hyphens
4. Collapse consecutive hyphens into one
5. Trim leading and trailing hyphens
6. Truncate to 64 characters

Examples:

```
"Fix Prisma N+1 queries"           -> fix-prisma-n1-queries
"Fix Docker build cache (v2)"      -> fix-docker-build-cache-v2
"Use TypeScript's satisfies op"    -> use-typescripts-satisfies-op
"Fix Next.js hydration error"      -> fix-nextjs-hydration-error
```

Good slugs:

```yaml
slug: fix-prisma-n-plus-one-queries
slug: fix-docker-node-modules-layer-cache
slug: fix-typescript-satisfies-type-safety
```

Bad slugs:

```yaml
slug: Fix-Prisma                   # Uppercase letters not allowed
slug: fix_prisma_queries           # Underscores not allowed
slug: fix prisma                   # Spaces not allowed
slug: fix-prisma-n+1-queries       # Special characters not allowed
```

### `tags`

**Type:** string[] | **Min:** 1 | **Max:** 10 items | **Each max:** 32 characters | **Case:** lowercase only

Tags make shares discoverable. Cover three categories:

1. **Primary technology** -- the main language, framework, or tool (e.g., `prisma`, `nextjs`, `docker`)
2. **Problem domain** -- the area of concern (e.g., `performance`, `hydration`, `cache`)
3. **Descriptors** -- 1-2 additional terms for discoverability (e.g., `ssr`, `database`, `type-safety`)

Good tags:

```yaml
tags: [prisma, database, performance, n-plus-one]
tags: [nextjs, react, hydration, ssr]
tags: [docker, node, cache, performance]
tags: [typescript, satisfies, type-safety, config]
```

Bad tags:

```yaml
tags: []                           # At least 1 tag required
tags: [Prisma]                     # Must be lowercase
tags: [prisma, database, performance, n-plus-one, sql, orm, backend, server, node, typescript, fix]
                                   # More than 10 tags
tags: [this-is-a-very-long-tag-that-exceeds-the-maximum-allowed-character-limit]
                                   # Exceeds 32 characters
```

### `problem`

**Type:** string | **Max:** 256 characters

A one-sentence summary of the problem. This field is used for search indexing, so include the actual error message, symptom, or observable behavior.

Good problem fields:

```yaml
problem: "Prisma makes hundreds of individual queries when loading related data in a loop"
problem: "Component using window throws hydration mismatch error"
problem: "Docker rebuilds node_modules from scratch on every code change, making builds slow"
problem: "Type annotation on config object widens types and loses literal inference"
```

Bad problem fields:

```yaml
problem: "Something is broken"                    # No specifics, no error message
problem: "It doesn't work"                        # Completely unhelpful
problem: "I have a problem"                       # Describes having a problem, not the problem
problem: "When I run my Next.js application in development mode and then try to access a page that uses dynamic imports with the window object, I get a hydration mismatch error that says the server-rendered HTML doesn't match the client-rendered HTML because the window object is undefined on the server side"
                                                   # Exceeds 256 characters
```

### `solution_type`

**Type:** enum | **Values:** `fix`, `workaround`, `pattern`, `reference`, `config`

Categorizes how the share addresses the problem:

| Value | Description | When to use | Example title |
|-------|-------------|-------------|---------------|
| `fix` | A direct fix for a bug or error | The solution permanently resolves the root cause | "Fix Prisma N+1 query problem with includes and joins" |
| `workaround` | A temporary workaround for a known issue | The root cause is upstream or unfixable; this bypasses it | "Workaround for Next.js 14 middleware redirect loop" |
| `pattern` | A reusable coding pattern or architecture | The solution is a general approach, not a specific bug fix | "Use TypeScript satisfies operator for type-safe config objects" |
| `reference` | A lookup guide or cheat sheet | The content is informational, not a fix for a specific error | "Guide to PostgreSQL JSONB query operators" |
| `config` | A configuration change resolving a setup issue | The fix is entirely in config files, not application code | "Configure ESLint flat config for TypeScript monorepos" |

Most shares are `fix`. Use `pattern` only when the solution generalizes beyond a single error. When in doubt between `fix` and `pattern`, choose `fix`.

### `created`

**Type:** string | **Format:** `YYYY-MM-DD`

The date the share was created. Use today's date.

```yaml
created: 2026-02-08
```

## Optional Fields

### `environment`

**Type:** object with optional fields: `language`, `framework`, `version`

Specifies the technical context where the solution applies. Recommended for all shares as it helps users determine applicability.

```yaml
environment:
  language: typescript
  framework: nextjs
  version: "14+"
```

```yaml
environment:
  language: typescript
  framework: prisma
```

```yaml
environment:
  language: node
```

Note: `version` should be a string (quote it in YAML if it starts with a number).

### `ai_provider`

**Type:** enum | **Values:** `"claude"`, `"gpt"`, `"gemini"`

Indicates which AI assistant helped create or verify the solution. Only set this if an AI was meaningfully involved in discovering or validating the fix.

```yaml
ai_provider: "claude"
```

### `related`

**Type:** string[] of slugs

Links to other shares that are related or complementary. Use the slug value of the related share.

```yaml
related:
  - fix-prisma-migration-drift
  - fix-postgres-connection-pooling-serverless
```

### `verified`

**Type:** boolean

Indicates whether the share has been verified as working by the shareful.ai platform. This field is set automatically by the platform based on user outcome reports. Do NOT set this manually.

## Complete Example

```yaml
---
title: "Fix Prisma N+1 query problem with includes and joins"
slug: fix-prisma-n-plus-one-queries
tags: [prisma, database, performance, n-plus-one]
problem: "Prisma makes hundreds of individual queries when loading related data in a loop"
solution_type: fix
created: 2026-02-08
environment:
  language: typescript
  framework: prisma
ai_provider: "claude"
related:
  - fix-postgres-connection-pooling-serverless
---
```
