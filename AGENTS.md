# Agent Instructions

This repository is a collection of reusable skills for AI coding agents. Each skill lives in its own directory with a `SKILL.md` file containing YAML frontmatter and agent instructions.

## Available Skills

| Directory                 | Description                                                                     |
| ------------------------- | ------------------------------------------------------------------------------- |
| `conventional-commit/`    | Write commit messages in Conventional Commits 1.0.0 format                      |
| `hugo-tailscale-preview/` | Serve Hugo drafts through a Tailscale-only preview server                       |
| `obsidian-headless-sync/` | Sync the LLM Wiki vault at `/home/hermes/llm-wiki` using Obsidian Headless Sync |

## Setup

```bash
git clone https://github.com/dbrennand/skills.git
cd skills
```

### Hermes

```bash
hermes skills install https://raw.githubusercontent.com/dbrennand/skills/main/conventional-commit/SKILL.md
hermes skills install https://raw.githubusercontent.com/dbrennand/skills/main/hugo-tailscale-preview/SKILL.md
hermes skills install https://raw.githubusercontent.com/dbrennand/skills/main/obsidian-headless-sync/SKILL.md
```

### Codex

Inside a Codex session, use the `$skill-installer` tool with the GitHub directory URL for each skill:

```text
$skill-installer install https://github.com/dbrennand/skills/tree/main/conventional-commit
$skill-installer install https://github.com/dbrennand/skills/tree/main/hugo-tailscale-preview
$skill-installer install https://github.com/dbrennand/skills/tree/main/obsidian-headless-sync
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

### Ground the skill in real expertise

- Start with a real task, existing runbook, API specification, schema, code
  review, incident, or version-control change. Do not rely on generic model
  knowledge.
- Preserve the corrections, project conventions, non-obvious edge cases, and
  input/output formats that made the real task succeed.
- Treat the first draft as a hypothesis. Run it against representative tasks,
  inspect the agent's execution trace as well as its final answer, and revise
  based on missed cases, false positives, wasted exploration, and unused
  instructions.

### Optimize context and information architecture

- Assume the model knows general concepts. Include only information it would
  otherwise get wrong or need to discover; remove explanations that do not
  change behavior.
- Give the skill one coherent responsibility. Avoid skills so narrow that they
  require several skills for one task, or so broad that activation becomes
  ambiguous.
- Keep `SKILL.md` concise: target fewer than 500 lines and 5,000 tokens. Put
  detailed API material, domain references, examples, and long templates in
  clearly named supporting files, and link to them directly from `SKILL.md`.
- Use progressive disclosure: tell the agent exactly when to read each
  supporting file. Keep references one level deep; references longer than 100
  lines should begin with a table of contents.
- Use consistent terminology, descriptive filenames, and forward-slash paths.

### Make instructions actionable

- Write reusable procedures, not a one-off answer. Prefer numbered steps with
  checkable completion criteria.
- Match control to risk. Allow judgment where approaches are safe and variable.
  Give exact commands, order, and guardrails where operations are fragile or
  consistency is critical.
- Choose a recommended default instead of presenting a menu of equivalent
  tools. Mention alternatives only as explicit escape hatches.
- Put concrete gotchas and known corrections in the main skill so they are
  visible before the mistake occurs.
- Use short, concrete input/output examples or templates when format and style
  matter. Use checklists for multi-step workflows.
- For quality-critical work, require a validation loop: perform the work, run
  a validator or self-check, fix failures, and repeat until it passes.
- For batch, destructive, or high-stakes work, use the sequence plan, validate,
  execute, and verify. Create a structured intermediate plan where possible.

### Treat bundled code as production support

- Bundle a tested utility script when the same deterministic parsing,
  transformation, or validation logic would otherwise be regenerated each run.
  State clearly whether the agent should execute the script or read it as a
  reference.
- Scripts should solve problems rather than defer them: handle expected errors,
  emit actionable diagnostics, and justify non-obvious constants.
- Document and verify dependencies instead of assuming packages or tools are
  installed. For MCP tools, use fully qualified names such as
  `ServerName:tool_name`.

### Evaluate before sharing

- Create at least three representative evaluations before writing extensive
  instructions. Establish a no-skill baseline, write the minimum needed to
  address observed gaps, then compare results.
- Test real usage and every model/runtime combination the skill will support.
  Confirm activation, correct reference-file navigation, required constraints,
  and verification behavior.
- Keep time-sensitive material out of the main workflow; isolate genuinely
  historical or deprecated patterns in a clearly labeled section.
