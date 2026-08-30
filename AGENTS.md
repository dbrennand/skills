# Agent Instructions

This repository is a collection of reusable skills for AI coding agents. Each skill lives in its own directory with a `SKILL.md` file containing YAML frontmatter and agent instructions.

## Available Skills

| Directory | Description |
| --- | --- |
| `commit/` | Handle commit requests by directing to the conventional-commit-message skill |
| `conventional-commit-message/` | Write commit messages in Conventional Commits 1.0.0 format |
| `obsidian-headless-sync/` | Sync the LLM Wiki vault at `/home/hermes/llm-wiki` using Obsidian Headless Sync |
| `pr-body/` | Draft polished, reviewer-friendly GitHub pull request bodies |

## Setup

```bash
git clone https://github.com/dbrennand/skills.git
cd skills
```

### Hermes

```bash
hermes skills install ./commit
hermes skills install ./conventional-commit-message
hermes skills install ./obsidian-headless-sync
hermes skills install ./pr-body
```

### Codex

Inside a Codex session, use the `$skill-installer` tool with the GitHub directory URL for each skill:

```text
$skill-installer install https://github.com/dbrennand/skills/tree/main/commit
$skill-installer install https://github.com/dbrennand/skills/tree/main/conventional-commit-message
$skill-installer install https://github.com/dbrennand/skills/tree/main/obsidian-headless-sync
$skill-installer install https://github.com/dbrennand/skills/tree/main/pr-body
```

The skill will be available on your next turn.

### Claude Code

```bash
# Symlink skills into ~/.claude/skills/ to make them available across all projects
ln -s "$(pwd)/commit" ~/.claude/skills/commit
ln -s "$(pwd)/conventional-commit-message" ~/.claude/skills/conventional-commit-message
ln -s "$(pwd)/obsidian-headless-sync" ~/.claude/skills/obsidian-headless-sync
ln -s "$(pwd)/pr-body" ~/.claude/skills/pr-body
```

Claude automatically discovers skills in `~/.claude/skills/` on next launch. You can then invoke them with `/skill-name` (e.g. `/pr-body`) or Claude loads them automatically when relevant based on the skill's `description` frontmatter.

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
