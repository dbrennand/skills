# Agent Instructions

This repository is a collection of reusable skills for AI coding agents. Each skill lives in its own directory with a `SKILL.md` file containing YAML frontmatter and agent instructions.

## Available Skills

| Directory              | Description                                                |
| ---------------------- | ---------------------------------------------------------- |
| `conventional-commit/` | Write commit messages in Conventional Commits 1.0.0 format |


## Setup

```bash
git clone https://github.com/dbrennand/skills.git
cd skills
```

### Hermes

```bash
hermes skills install https://raw.githubusercontent.com/dbrennand/skills/main/conventional-commit/SKILL.md
```

### Codex

Inside a Codex session, use the `$skill-installer` tool with the GitHub directory URL for each skill:

```text
$skill-installer install https://github.com/dbrennand/skills/tree/main/conventional-commit
```

The skill will be available on your next turn.

### Claude Code

```bash
# Symlink skills into ~/.claude/skills/ to make them available across all projects
ln -s "$(pwd)/conventional-commit" ~/.claude/skills/conventional-commit
ln -s "$(pwd)/hugo-tailscale-preview" ~/.claude/skills/hugo-tailscale-preview
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

## Skill authoring best practices

Apply these guidelines when creating or revising a skill. They combine the
[Agent Skills best practices](https://agentskills.io/skill-creation/best-practices)
with the [Claude skill authoring guidance](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices).
