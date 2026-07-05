# Agent Instructions

This repository is a repo-local Codex plugin marketplace for personal skills. It currently publishes a `git` plugin with skills for conventional commit messages and pull request descriptions.

## Repository Layout

- `README.md` documents installation and local hook usage.
- `.agents/plugins/marketplace.json` registers the local marketplace and points at `./plugins/git`.
- `plugins/git/.codex-plugin/plugin.json` is the plugin manifest.
- `plugins/git/skills/<skill-name>/SKILL.md` contains each skill's metadata and instructions.
- `.pre-commit-config.yaml`, `.markdownlint.jsonc`, and `.github/workflows/markdownlint.yml` define Markdown linting.

## Skill Editing

- Keep each skill in its own directory with a `SKILL.md` file.
- Preserve the YAML front matter at the top of each `SKILL.md`; `name` should match the skill directory name.
- Write skill instructions as direct, reusable guidance for Codex, with concrete trigger examples and output rules.
- Prefer small, explicit edits to skill behavior over broad rewrites.
- When adding or removing skills, update the plugin manifest and README so the published plugin description stays accurate.
- For the PR description skill, PR titles must not include `[codex]`, `Codex`, or generated-by labels.

## Validation

Run the project hooks after changes:

```bash
uvx prek run --all-files
```

The current hook suite runs `markdownlint-cli2`. If you change lint coverage, keep `.pre-commit-config.yaml` and `.github/workflows/markdownlint.yml` aligned.

## Release Workflow

- After changes to this repository are committed, re-install the affected Codex plugin(s) so the local Codex environment picks up the committed skill updates.

## Style

- Use Markdown headings and bullets consistently.
- Keep wording concise and reviewer-facing.
- Do not add generated artifacts or dependency directories to the repository.
