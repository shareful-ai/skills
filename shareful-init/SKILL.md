---
name: shareful-init
description: Guides setup of a shareful.ai shares repository. Runs npx shareful-ai init to create the directory structure, explains the repo layout, and walks through next steps. Use when the user wants to "set up shareful", "create a shares repo", "start sharing solutions", or "initialize shareful".
---

# Shareful Init

Set up a shareful.ai shares repository so you can start capturing and sharing coding solutions.

## When to Use This Skill

Use this skill when the user:

- Wants to set up a shares repository for the first time
- Asks "how do I start sharing solutions" or "set up shareful"
- Wants to contribute fixes back to the community
- Needs a repo to store SHARE.md files

## What Init Creates

`npx shareful-ai init [name]` creates a ready-to-use shares repository:

```
my-shares/
  .gitignore
  README.md
  AGENTS.md
  shares/
    example-share/
      SHARE.md
```

- **`shares/`** -- directory for all SHARE.md solution files
- **`AGENTS.md`** -- documents the SHARE.md format for AI agents working in the repo
- **`README.md`** -- repo overview with quick start instructions
- **`example-share/SHARE.md`** -- template share to get started

The command also initializes a git repository and saves the repo path to `~/.shareful/config.json`.

## Setup Workflow

### Step 1: Create the Repository

```bash
npx shareful-ai init my-shares
```

The name must be alphanumeric with dots, hyphens, or underscores (max 128 characters). Defaults to `shares` if not provided.

### Step 2: Push to GitHub

```bash
cd my-shares
gh repo create my-shares --source . --public --push
```

Or create the repository manually on GitHub and push:

```bash
cd my-shares
git remote add origin git@github.com:username/my-shares.git
git push -u origin main
```

### Step 3: Create Your First Share

```bash
npx shareful-ai create
```

This walks through creating a SHARE.md interactively -- prompting for title, problem description, solution type, and tags. The file is created in `shares/<slug>/SHARE.md`.

### Step 4: Register for Indexing

After pushing to GitHub, register the repo so solutions appear on shareful.ai:

```bash
npx shareful-ai register
```

## Name Validation Rules

- Letters, numbers, dots, hyphens, and underscores only
- Max 128 characters
- Examples: `my-shares`, `company.solutions`, `team_fixes`

## Related Skills

- `shareful-create` for writing high-quality SHARE.md files
- `shareful-search` for finding existing solutions on shareful.ai
