# CCS CLI Reference

Complete reference for claude-code-search (ccs), the CLI tool for searching past Claude Code conversations.

## Installation

```bash
npm install -g claude-code-search
```

Requires Node.js 18+. Data is read from `~/.claude/projects/` where Claude Code stores session history.

## Commands

### Interactive Mode

```bash
ccs
```

Launches an interactive TUI with fuzzy search. Keyboard shortcuts:

- `Ctrl+R` / `Shift+Tab` -- toggle between search input and results
- `j` / `k` or `Up` / `Down` -- navigate results
- `Enter` -- copy selected prompt to clipboard
- `Ctrl+C` / `q` -- quit

### Search (Non-Interactive)

```bash
ccs -s "query" [-n limit] [-j]
```

| Flag | Description | Default |
|------|-------------|---------|
| `-s, --search <query>` | Fuzzy search across all prompts | -- |
| `-n, --limit <num>` | Max results returned | 100 |
| `-j, --json` | Output as JSON array | plain text |
| `-p, --project <path>` | Filter by project path | all projects |

### List Recent Prompts

```bash
ccs -l [-n limit] [-j]
```

| Flag | Description | Default |
|------|-------------|---------|
| `-l, --list` | List prompts (most recent first) | -- |
| `-n, --limit <num>` | Max results | 100 |
| `-j, --json` | Output as JSON | plain text |

### Version and Help

```bash
ccs -v    # Show version
ccs -h    # Show help
```

## JSON Output Format

When using `-j`, each result is an object with:

```json
{
  "content": "the user's prompt text",
  "timestamp": "2026-02-09T10:14:39.916Z",
  "project": "owner/repo",
  "projectPath": "/Users/name/Code/project",
  "cwd": "/Users/name/Code/project/src",
  "gitBranch": "main"
}
```

## Common Search Examples

```bash
# Find error-related conversations
ccs -s "error" -j -n 30

# Find specific error types
ccs -s "TypeError" -j -n 20
ccs -s "Module not found" -j -n 20

# Find framework-specific issues
ccs -s "nextjs hydration" -j -n 20
ccs -s "prisma migration" -j -n 20

# Filter by project
ccs -s "error" -p /Users/name/Code/my-project -j

# Get all recent prompts for manual review
ccs -l -j -n 200
```
