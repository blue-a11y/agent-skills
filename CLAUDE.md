# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A collection of Claude Code agent skills published by [blue-a11y](https://github.com/blue-a11y). Skills are installed via `npx skills add blue-a11y/agent-skills [--skill <name>]`.

## Repo Structure

```
skills/
  <skill-name>/
    SKILL.md          # Required — skill definition with frontmatter + instructions
    references/       # Optional — supplementary reference docs
```

Each skill is self-contained in its own directory under `skills/`. The `SKILL.md` frontmatter defines `name`, `description`, and `allowed-tools`.

## Current Skills

| Skill | Purpose |
|-------|---------|
| `ai-commit` | AI-powered git commit message generator (Conventional Commits format) |
| `ahooks-userequest` | ahooks `useRequest` best practices guide with scenario examples |

## Adding a New Skill

1. Create `skills/<skill-name>/SKILL.md` with frontmatter:
   ```yaml
   ---
   name: skill-name
   description: One-line description shown in skill listings
   allowed-tools:
     - Bash
     - Read
   ---
   ```
2. Write skill instructions in the body below the frontmatter.
3. Optionally add a `references/` subdirectory for supplementary docs.
4. Add the skill entry to the table in `README.md`.

## Conventions

- Skill instructions are written in English.
- Frontmatter `name` must match the directory name (kebab-case).
- `allowed-tools` lists only the tools the skill actually needs.
- `references/` files are linked from `SKILL.md` for detailed content that would bloat the main instructions.
