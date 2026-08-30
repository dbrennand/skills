---
name: commit
description: "Use when the user asks to commit changes or make a commit, e.g. `commit these changes`, `commit this`, or `stage and commit the changes`. Executes the commit and directs the agent to the conventional-commit-message skill for the message format and content."
---

# Commit

## Goal

Handle any request to commit staged or working changes, deferring the commit message itself to the `conventional-commit-message` skill so the result follows Conventional Commits 1.0.0.

## When to Use

Use this skill for requests like:

- `commit these changes`
- `commit this`
- `make a commit`
- `stage and commit the changes`

Do not use this skill for commit message wording; that is the `conventional-commit-message` skill's job. Do not use it for pull request bodies; that is the `pr-body` skill's job.

## Direct to conventional-commit-message

1. Load the `conventional-commit-message` skill (`skill_view(name='conventional-commit-message')`).
2. Follow its instructions to produce the commit subject, body, and any footers from the actual diff or staged changes.
3. Return the final message ready to paste into Git, or run the commit if the user asked for it directly.

## Notes

- This skill does not restate the Conventional Commits format; the `conventional-commit-message` skill is the single source of truth for message wording.
- If multiple unrelated changes are mixed together, suggest splitting into separate commits before committing.
