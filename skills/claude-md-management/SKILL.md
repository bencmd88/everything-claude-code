---
name: claude-md-management
description: Create, update, validate, and maintain CLAUDE.md files that give Claude Code accurate, useful project context.
origin: ECC
---

# CLAUDE.md Management

Best practices for creating and maintaining CLAUDE.md files — the primary mechanism for giving Claude Code persistent, project-specific context.

## When to Activate

- Starting a new project that lacks a CLAUDE.md
- Running `/update-claude-md` or asking to refresh project context
- Onboarding Claude Code to an unfamiliar codebase
- Project structure, tooling, or conventions have changed significantly
- CLAUDE.md feels stale or incomplete
- Before a major refactor or architectural change

## What CLAUDE.md Is For

CLAUDE.md is loaded automatically by Claude Code at session start. It provides:

- **Project overview** — what this project does and its purpose
- **Key commands** — how to build, test, lint, and run the project
- **Architecture** — directory layout, major modules, data flow
- **Conventions** — naming, style, patterns specific to this codebase
- **Gotchas** — known quirks, non-obvious decisions, things to avoid

It is NOT a general README. It is instructions *to Claude*, written in the imperative.

## CLAUDE.md Structure

```markdown
# CLAUDE.md

Brief one-line description of what this project is.

## Key Commands

\`\`\`bash
# Most-used commands go here
npm run dev          # Start dev server
npm test             # Run tests
npm run build        # Production build
npm run lint         # Lint and format
\`\`\`

## Architecture

Short description of how the codebase is organized.

- **src/components/** — React UI components
- **src/api/** — API layer and data fetching
- **src/store/** — State management (Zustand)
- **scripts/** — Build and utility scripts

## Conventions

- Use X pattern for Y
- Always do Z when working with W
- File naming: lowercase-with-hyphens

## Development Notes

- Note any non-obvious environment requirements
- Known issues or limitations
- External dependencies or services needed
```

## Rules for Good CLAUDE.md Files

### DO
- Write commands that actually work (verify before adding)
- Describe *why* things are structured a certain way, not just *what*
- List the most important files/modules prominently
- Include environment setup if non-trivial
- Keep it under 300 lines — ruthlessly prune fluff
- Update it when the project changes

### DON'T
- Repeat what's obvious from the code
- Include generic programming advice
- Add things that belong in README.md or docs/
- Let it go stale (>3 months without review)
- Duplicate content from other tool configs (.eslintrc, etc.)

## Validation Checklist

Before finalizing a CLAUDE.md, verify:

- [ ] All commands in "Key Commands" run without error
- [ ] Directory paths mentioned actually exist
- [ ] Architecture description matches actual structure
- [ ] Conventions reflect current code, not aspirational style
- [ ] No secrets or credentials included
- [ ] File is ≤300 lines

## Multi-Project / Monorepo

For monorepos, place a root CLAUDE.md with global conventions and a per-package CLAUDE.md for package-specific details:

```
repo/
├── CLAUDE.md            # Global: repo commands, overall architecture
├── packages/
│   ├── api/
│   │   └── CLAUDE.md    # API-specific: routes, DB, auth patterns
│   └── web/
│       └── CLAUDE.md    # Frontend-specific: components, state, routing
```

Root CLAUDE.md should use `@path/to/package/CLAUDE.md` imports if supported.

## Keeping CLAUDE.md Fresh

Run `/update-claude-md` periodically or after:
- Adding new packages or dependencies
- Changing the build system or test runner
- Restructuring directories
- Establishing new team conventions
- Completing a major feature that changes architecture
