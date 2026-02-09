# Quality Examples

Annotated examples of good and bad shares. Use these to calibrate quality when writing or reviewing a SHARE.md.

## Contents

- [Good Example: Fix Type (Prisma N+1 Queries)](#good-example-fix-type-prisma-n1-queries)
- [Good Example: Pattern Type (TypeScript satisfies)](#good-example-pattern-type-typescript-satisfies)
- [Bad Share Patterns](#bad-share-patterns)
- [Section Length Guidelines](#section-length-guidelines)
- [Pre-submission Quality Checklist](#pre-submission-quality-checklist)

## Good Example: Fix Type (Prisma N+1 Queries)

Source: `starter-shares/shares/fix-prisma-n-plus-one-queries/SHARE.md`

### Frontmatter

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
---
```

What makes this good:

- **Title** starts with "Fix" matching `solution_type: fix`, names the technology (Prisma), and describes the specific problem (N+1) and solution approach (includes and joins)
- **Slug** matches the directory name, uses only lowercase letters and hyphens
- **Tags** cover the technology (`prisma`), domain (`database`, `performance`), and specific pattern (`n-plus-one`)
- **Problem** includes the observable symptom ("hundreds of individual queries") making it searchable by developers who notice excessive queries
- **Environment** specifies the language and framework

### Problem Section

```markdown
## Problem

Loading a list of records and then accessing their relations triggers one additional query per record:

\`\`\`typescript
const posts = await prisma.post.findMany();
for (const post of posts) {
  const author = await prisma.user.findUnique({
    where: { id: post.authorId },
  });
}
\`\`\`

With 100 posts, this generates 101 database queries.
```

What makes this good:

- Shows the broken code with a real, minimal reproduction
- Includes a comment explaining the issue inline
- Quantifies the impact ("101 database queries")
- Does not explain the fix -- only shows the broken state

### Solution Section

```markdown
## Solution

**Option 1: Use `include` to eager-load relations (recommended)**
[code block with inline comments]

**Option 2: Use `select` for specific fields (better performance)**
[code block]

**Option 3: Use `relationLoadStrategy: "join"` for a single SQL query (Prisma 5.9+)**
[code block]
```

What makes this good:

- Three options, each clearly labeled with when to use it
- Option 1 marked as "(recommended)" so the reader has a clear default
- Option 3 includes a version requirement in the label
- Each code block has the `typescript` language label
- Code blocks are complete and runnable (not fragments)

### Why It Works Section

The share explains that Prisma does NOT load relations by default, that the N+1 arises from manual looping, and that `include` changes the behavior to use an `IN` clause. It also explains the `JOIN` alternative.

What makes this good:

- Explains the root cause (default lazy behavior) not just "add include"
- Describes the actual SQL mechanism (`IN` clause vs `JOIN`)
- Connects each option back to how it changes the underlying behavior
- Written as prose, not code

### Context Section

```markdown
## Context

- Prisma 4.x+ for `include`, Prisma 5.9+ for `relationLoadStrategy: "join"`
- Use Prisma's query logging to detect N+1: `new PrismaClient({ log: ["query"] })`
- For GraphQL resolvers, consider `prisma-graphql-fields-optimizer` or DataLoader pattern
- `select` is more efficient than `include` when you do not need all columns
```

What makes this good:

- Version requirements listed first
- Includes a debugging tip (query logging)
- Mentions related patterns for specific use cases (GraphQL)
- Each bullet adds unique information
- Bulleted list for quick scanning

## Good Example: Pattern Type (TypeScript satisfies)

Source: `starter-shares/shares/fix-typescript-satisfies-type-safety/SHARE.md`

### Frontmatter

```yaml
---
title: "Use TypeScript satisfies operator for type-safe config objects"
slug: fix-typescript-satisfies-type-safety
tags: [typescript, satisfies, type-safety, config]
problem: "Type annotation on config object widens types and loses literal inference"
solution_type: pattern
created: 2026-02-08
environment:
  language: typescript
  version: "4.9+"
---
```

What makes this good:

- **Title** starts with "Use" matching `solution_type: pattern`, names the feature (`satisfies`), and describes what it achieves (type-safe config)
- **Problem** describes the technical symptom ("widens types and loses literal inference") that a developer would recognize
- **Version** is quoted as `"4.9+"` since it starts with a number (proper YAML)

### Problem Section

The share shows a concrete `Routes` type and demonstrates how a type annotation widens the inferred type, with comments showing the exact type that TypeScript infers.

What makes this good:

- Uses a realistic example (route definitions) that developers encounter in real projects
- Shows the exact TypeScript types in comments so the reader can verify the behavior
- Does not require running the code to understand the problem

### Solution Section

Provides the `satisfies` approach, then a `satisfies` + `as const` combination, then a practical example with a theme config.

What makes this good:

- Builds from simple to advanced (satisfies alone, then satisfies + as const)
- Includes a second practical example (theme config) showing the pattern generalizes
- Comments show the inferred types so readers can see the difference

### Why It Works Section

Explains the difference between type annotation (forces the variable type) and satisfies (checks compatibility but preserves inference).

What makes this good:

- Concise: two short paragraphs
- Explains the mechanism (annotation forces type vs. satisfies checks compatibility)
- Uses precise terminology ("validation" and "inference") that connects to TypeScript concepts

### Context Section

Lists the version requirement, ideal use cases, the most powerful combination (`as const satisfies`), and a limitation (`let` declarations).

What makes this good:

- Includes a practical limitation (does not work with `let`) that prevents misuse
- Lists specific use cases where the pattern applies

## Bad Share Patterns

### Bad Frontmatter

```yaml
---
title: "fix stuff"
slug: Fix_Stuff
tags: [Fix, stuff, things, misc, general, code, programming, software, development, engineering, TypeScript]
problem: "broken"
solution_type: pattern
created: Feb 8
---
```

Problems:

- **Title**: no capitalization, vague ("stuff"), no technology or specific problem
- **Slug**: uppercase and underscores violate the `/^[a-z0-9-]+$/` pattern
- **Tags**: uppercase ("Fix"), more than 10 items, vague terms ("things", "misc")
- **Problem**: single word, does not describe any actual symptom or error
- **Solution type**: labeled `pattern` but the title says "fix" -- mismatched
- **Created**: wrong format, must be `YYYY-MM-DD`

### Bad Problem Section

```markdown
## Problem

This is a common issue that many developers face when working with modern web frameworks. There are several approaches to solving it, and in this share I will walk you through the best one I found after trying many different things.
```

Problems:

- No code shown
- No error message or symptom
- No quantified impact
- Reads like a blog introduction, not a problem description
- Already talks about the solution

### Bad Solution Section

```markdown
## Solution

Just add `include` to your query. This should fix it.

```
prisma.post.findMany({ include: { author: true } })
```
```

Problems:

- Code block has no language label (should be ````typescript`)
- Incomplete snippet (no `await`, no `const`, no surrounding context)
- No recommended option labeling
- Explanation mixed into solution text instead of code comments
- Does not show the full working code

### Bad Why It Works Section

```markdown
## Why It Works

It works because we added `include: { author: true }` to the query. This tells Prisma to include the author in the results. See the Prisma documentation for more information.
```

Problems:

- Restates the solution ("we added include") instead of explaining the mechanism
- Does not explain WHY include fixes the N+1 problem
- Defers to external docs instead of explaining here
- Does not mention the underlying SQL behavior

### Bad Context Section

```markdown
## Context

This solution uses Prisma's include feature to eager-load relations. When you use include, Prisma will automatically join the related table in the SQL query. This is much more efficient than loading each relation individually. The include feature has been available since Prisma 2.0 and is the recommended approach for loading related data.
```

Problems:

- Written as a paragraph instead of a bulleted list
- Repeats the solution explanation (belongs in Why It Works)
- No specific version requirements with version numbers
- No gotchas or limitations
- No related tools or alternatives

## Section Length Guidelines

| Section | Target lines | Maximum lines | Notes |
|---------|-------------|---------------|-------|
| Problem | 5-20 | 40 | Minimal reproduction, not a full app |
| Solution | 15-60 | 120 | Multiple options increase length |
| Why It Works | 5-15 | 30 | Prose, not code |
| Context | 3-8 | 15 | Bulleted list |
| **Total body** | **60-150** | **300** | Enforced by CLI validation |

## Pre-submission Quality Checklist

Run through this checklist before publishing:

```text
Frontmatter:
- [ ] Title starts with a verb matching solution_type (Fix, Use, Configure, etc.)
- [ ] Title is specific (names technology and problem)
- [ ] Title is under 128 characters
- [ ] Slug matches the parent directory name
- [ ] Slug contains only lowercase letters, numbers, and hyphens
- [ ] Slug is under 64 characters
- [ ] Tags are 1-10 items, all lowercase, each under 32 characters
- [ ] Tags cover: technology, domain, and 1-2 descriptors
- [ ] Problem field includes the error message or observable symptom
- [ ] Problem is under 256 characters
- [ ] solution_type matches the title verb and content
- [ ] created is YYYY-MM-DD format

Problem section:
- [ ] Shows broken code with a minimal reproduction
- [ ] Includes exact error message or quantified impact
- [ ] Does not explain the fix

Solution section:
- [ ] All code blocks have language labels
- [ ] Code is complete and runnable (not fragments)
- [ ] Multiple options are labeled with (recommended) on the default
- [ ] No placeholder content (<!-- ... --> or TODO)

Why It Works section:
- [ ] Explains the root cause, not just restates the fix
- [ ] Written as prose (1-3 paragraphs)
- [ ] Covers why the default behavior causes the problem

Context section:
- [ ] Formatted as a bulleted list
- [ ] Version requirements listed first
- [ ] Includes at least one gotcha or limitation
- [ ] Each bullet adds unique information

Overall:
- [ ] Body is under 300 lines
- [ ] No placeholder content anywhere
- [ ] All four required sections present in order
```
