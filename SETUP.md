# Agent Setup Instructions

This file is for Claude Code (or any AI coding agent). When a user asks you to install or set up this harness, follow these steps in order.

## Prerequisites

Verify these exist before proceeding. If any are missing, tell the user what to install and stop.

- `jq` — run `jq --version`
- `git` — run `git --version`
- `bash` — run `bash --version`
- The current working directory must be inside a git repository — run `git rev-parse --show-toplevel`

## Step 1: Run the setup script

```bash
bash /path/to/harness/setup.sh
```

Replace `/path/to/harness/` with the actual path where this repo was cloned. If you cloned it to a temp directory, use that path.

This script installs hooks, skills, templates, memory directory, and analytics. It is safe to re-run — it skips files that already exist.

## Step 2: Fill in template variables

The setup script copies template files with `{{PLACEHOLDER}}` values. You must replace these with real values. Ask the user for any value you cannot infer from the repository.

### CLAUDE.md

| Placeholder | What to fill in | How to infer |
|-------------|----------------|--------------|
| `{{PROJECT_NAME}}` | The project name | Use the repo directory name or ask the user |
| `{{service}}` | Each service/component in the project | Scan the repo structure for directories like `api/`, `www/`, `app/`, `infrastructure/`, `worker/`, etc. Create one row per service found. |

Also remove the instruction line that says "Replace `{{PLACEHOLDERS}}` with your values. Delete this line when done."

### MAINTENANCE.md

| Placeholder | What to fill in |
|-------------|----------------|
| `{{YYYY-MM-DDTHH:MM:SSZ}}` | Replace all instances with the current UTC timestamp in ISO 8601 format |

### .claude-memory/MEMORY.md

| Placeholder | What to fill in |
|-------------|----------------|
| `{{YYYY-MM-DD}}` | Replace with today's date |

### MILESTONES.md

| Placeholder | What to fill in |
|-------------|----------------|
| `{{Phase Name}}` | Ask the user what their current milestone or project phase is. If they don't have one, use "Phase 1 — Initial Setup" |

## Step 3: Verify installation

Run these checks and confirm they pass:

1. `.claude/hooks/` exists and contains 5 `.sh` files
2. `.claude/skills/` exists and contains 5 skill directories
3. `.claude-memory/MEMORY.md` exists
4. `CLAUDE.md` exists at repo root with no remaining `{{` placeholders
5. `MAINTENANCE.md` exists at repo root with no remaining `{{` placeholders
6. `scripts/cc-budget.sh` exists and is executable
7. `~/.claude/settings.json` contains hooks for `SessionStart`, `SessionEnd`, and `Stop`

## Step 4: Confirm to the user

Tell the user:
- Setup is complete
- They can type `/commit`, `/done`, `/queue`, `/nu`, or `/preview` in any Claude Code session
- Session tracking and token budgeting are active
- They should review `CLAUDE.md` and adjust the Architecture Map table to match their project

## Notes

- If `CLAUDE.md` or other template files already exist, the setup script skips them. If the user wants to overwrite, they must delete the existing file first.
- The setup script creates symlinks (or Windows directory junctions) from `~/.claude/hooks/` and `~/.claude/skills/` to the repo's `.claude/` directory. If the user works across multiple repos, only the last-installed repo's hooks/skills will be active globally.
- Template variable replacement is a one-time operation. After filling in placeholders, these files become living documents maintained by the agent during normal work.
