---
description: Create or update CLAUDE.md with accurate, current project context.
---

# /update-claude-md

Scan the project and create or refresh the CLAUDE.md file with accurate, useful context for Claude Code.

## Step 1: Audit Existing CLAUDE.md

If a CLAUDE.md already exists:
1. Read it fully
2. Note which sections exist and which are missing
3. Flag any commands, paths, or descriptions that may be stale

If no CLAUDE.md exists, proceed to Step 2 with a blank slate.

## Step 2: Discover Project Shape

Run the following discovery in parallel:

**Package manager & scripts**
- Read `package.json`, `Makefile`, `pyproject.toml`, `Cargo.toml`, `go.mod`, or equivalent
- Extract the most-used commands: dev server, test runner, build, lint/format

**Directory structure**
- List top-level directories (skip `node_modules`, `.git`, `dist`, `build`, `.next`, `__pycache__`, `.venv`)
- Identify key source directories and their roles

**Tech stack**
- Identify language(s), frameworks, and major libraries from manifests and imports
- Note database, cache, queue, or external service dependencies

**Test setup**
- Find test config files (jest.config, pytest.ini, vitest.config, etc.)
- Identify how to run tests and where test files live

**Lint/format**
- Find .eslintrc, .prettierrc, ruff.toml, golangci.yml, or equivalent
- Extract the lint/format command

**Environment**
- Check for `.env.example`, `.env.template`, or `README` sections on env setup
- List required environment variables (not their values)

## Step 3: Verify Commands

Before writing, run (or dry-run) the key commands found in Step 2 to confirm they are valid in the current environment. Flag any that fail.

## Step 4: Write CLAUDE.md

Produce a CLAUDE.md with these sections (omit any that genuinely don't apply):

```markdown
# CLAUDE.md

[One sentence: what is this project and what does it do.]

## Key Commands

\`\`\`bash
[verified commands for: dev, test, build, lint]
\`\`\`

## Architecture

[2–5 sentences on overall structure and data flow.]

- **dir/** — what lives here
- **dir/** — what lives here

## Conventions

- [Naming conventions, patterns, non-obvious rules]
- [Any project-specific DO/DON'T rules]

## Development Notes

- [Non-obvious environment requirements]
- [Known quirks or limitations]
- [Important context Claude should always keep in mind]
```

## Step 5: Review & Confirm

Show the user a diff (or the full file if new) and ask for confirmation before writing. After writing:

```
CLAUDE.md updated
──────────────────────────────
Sections: Key Commands, Architecture, Conventions, Development Notes
Commands verified: npm run dev, npm test, npm run build, npm run lint
Lines: 87
──────────────────────────────
```

## Rules

- **Be specific**: prefer `npm run test:unit` over generic "run tests"
- **Be accurate**: only include commands and paths that were verified
- **Be concise**: target 80–150 lines; hard cap at 300
- **Don't invent**: if you can't verify something, omit it or mark it with `# TODO: verify`
- **Don't duplicate**: if content belongs in README or docs, leave it there
- **Don't include secrets**: never write actual credentials or tokens
