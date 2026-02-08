# Writing the Four Body Sections

Every SHARE.md requires exactly four sections in this order: Problem, Solution, Why It Works, Context. The body (everything after the frontmatter `---`) must be under 300 lines total. Aim for 60-150 lines.

## 1. Problem

### Purpose

Show the broken state so readers can confirm they have the same issue. The Problem section is the "hook" -- if this does not match what the reader is experiencing, they move on.

### Must Include

- **Broken code** -- a minimal code snippet that reproduces the issue
- **Error message or symptom** -- the exact error text, incorrect output, or observable bad behavior
- **Impact** -- quantify when possible ("101 queries instead of 2", "builds take 5 minutes instead of 10 seconds")

### Template

```markdown
## Problem

[1-2 sentences describing when this problem occurs.]

\`\`\`[language]
// Code that demonstrates the broken behavior
\`\`\`

[Error message, incorrect output, or quantified impact.]
```

### Anti-patterns

- **No code shown** -- "When you use Prisma wrong, it's slow" (show the actual broken code)
- **No error message** -- if there is a specific error, include the exact text
- **Too much context** -- do not explain your entire application; show only the minimal reproduction
- **Explaining the solution here** -- the Problem section shows only the broken state, not the fix

## 2. Solution

### Purpose

Provide the working code. The reader should be able to copy this into their project and have it work (with minor adaptation).

### Must Include

- **Complete, runnable code blocks** -- each with a language label
- **Multiple options when applicable** -- label with "Option 1", "Option 2", etc.
- **Recommended hint** -- mark the best default option with "(recommended)"
- **Comments in code** -- explain non-obvious lines inline

### Template

```markdown
## Solution

**Option 1: [approach name] (recommended)**

\`\`\`[language]
// Working code with inline comments
\`\`\`

**Option 2: [alternative approach] ([when to prefer this])**

\`\`\`[language]
// Alternative working code
\`\`\`
```

For single-option solutions:

```markdown
## Solution

\`\`\`[language]
// Working code with inline comments
\`\`\`

[Optional: brief setup note, e.g., "Add a `.dockerignore` to avoid copying unnecessary files:"]

\`\`\`[language]
// Supporting configuration
\`\`\`
```

### Anti-patterns

- **No language label on code blocks** -- always use ````typescript`, ````dockerfile`, ````bash`, etc.
- **Incomplete snippets** -- "just add `include`" without showing where; show the full query
- **No recommended option** -- if you show multiple options, indicate which one to use by default
- **Mixing explanation into code** -- put prose outside code blocks; use short inline comments only
- **Placeholder code** -- `// Add your code here` belongs in templates, not in shares

## 3. Why It Works

### Purpose

Explain the underlying mechanism so the reader understands the root cause, not just the fix. This section answers "why was it broken?" and "why does the fix resolve it?".

### Must Include

- **Root cause explanation** -- what was actually happening under the hood
- **Mechanism of the fix** -- why the change resolves the root cause
- **1-3 paragraphs of prose** -- this section is primarily text, not code

### Template

```markdown
## Why It Works

[First paragraph: explain the default behavior that causes the problem. What does the framework/library/tool do under the hood?]

[Second paragraph: explain why the fix changes that behavior. Connect the solution back to the root cause.]

[Optional third paragraph: additional nuance, tradeoffs, or how different options compare.]
```

### Anti-patterns

- **Restating the solution** -- "It works because we added `include`" (explain WHY include fixes it)
- **Just linking docs** -- "See the Prisma docs" (explain it here; link in Context if needed)
- **Too short** -- "This fixes the bug." (explain the mechanism)
- **Too long** -- keep it to 1-3 focused paragraphs; this is not a blog post
- **Code-heavy** -- this section should be prose; save code for the Solution section

## 4. Context

### Purpose

Help the reader determine if the solution applies to their environment and warn them about edge cases.

### Must Include

- **Version requirements** -- list first; which versions of the language/framework/tool are needed
- **Gotchas** -- edge cases, limitations, or scenarios where the solution does not apply
- **Related tools or alternatives** -- other approaches, libraries, or debugging aids

### Template

```markdown
## Context

- [Framework/tool] [version]+ required for [feature used in solution]
- [Additional version or environment requirement]
- [Gotcha or limitation]
- [Related tool, alternative approach, or debugging tip]
```

### Anti-patterns

- **No version info** -- always specify the minimum version where the solution works
- **Paragraph form** -- use a bulleted list; this section is for quick scanning
- **Repeating the solution** -- do not re-explain the fix; note prerequisites and limitations
- **Too many bullets** -- keep it to 3-6 items; every bullet should add unique information

## Section Length Guidelines

| Section | Target lines | Maximum lines | Notes |
|---------|-------------|---------------|-------|
| Problem | 5-20 | 40 | Minimal reproduction, not a full app |
| Solution | 15-60 | 120 | Multiple options increase length |
| Why It Works | 5-15 | 30 | Prose, not code |
| Context | 3-8 | 15 | Bulleted list |
| **Total body** | **60-150** | **300** | Enforced by CLI validation |

If you find yourself exceeding these targets, look for:

- Code blocks that can be shortened (remove boilerplate, focus on the changed lines)
- Prose that repeats information already in other sections
- Context bullets that duplicate version info from the environment frontmatter field
