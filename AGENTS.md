# Agent Instructions

This repository is a collection of reusable skills for AI coding agents. Each skill lives in its own directory with a `SKILL.md` file containing YAML frontmatter and agent instructions.

## Available Skills

| Directory | Description |
| --- | --- |
| `conventional-commit/` | Write commit messages in Conventional Commits 1.0.0 format |
| `obsidian-headless-sync/` | Sync the LLM Wiki vault at `/home/hermes/llm-wiki` using Obsidian Headless Sync |

## Setup

```bash
git clone https://github.com/dbrennand/skills.git
cd skills
```

### Hermes

```bash
hermes skills install ./conventional-commit
hermes skills install ./obsidian-headless-sync
```

### Codex

Inside a Codex session, use the `$skill-installer` tool with the GitHub directory URL for each skill:

```text
$skill-installer install https://github.com/dbrennand/skills/tree/main/conventional-commit
$skill-installer install https://github.com/dbrennand/skills/tree/main/obsidian-headless-sync
```

The skill will be available on your next turn.

### Claude Code

```bash
# Symlink skills into ~/.claude/skills/ to make them available across all projects
ln -s "$(pwd)/conventional-commit" ~/.claude/skills/conventional-commit
ln -s "$(pwd)/obsidian-headless-sync" ~/.claude/skills/obsidian-headless-sync
```

Claude automatically discovers skills in `~/.claude/skills/` on next launch. You can then invoke them with `/skill-name` or Claude loads them automatically when relevant based on the `description` frontmatter.

For per-project use, symlink into a project's `.claude/skills/` directory instead.

## Validation

Markdown linting lives in `.github/workflows/markdownlint.yml` and `.markdownlint.jsonc`. Run locally:

```bash
uvx prek run --all-files
```

## Style

- Each skill is a directory containing a single `SKILL.md` file.
- `SKILL.md` starts with YAML frontmatter: `name` (matches directory name) and `description`.
- Keep instructions direct and trigger-aware.
- When adding or removing skills, update this table and the `README.md`.
