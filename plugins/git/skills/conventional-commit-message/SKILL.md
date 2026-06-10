---
name: conventional-commit-message
description: Use when writing, rewriting, or tightening a Git commit message into Conventional Commits 1.0.0 format. Produces a commit subject in `<type>[optional scope][!]: <description>` form and adds a body or footers only when the change needs that extra context.
---

# Conventional Commit Message

## Goal

Write commit messages that follow the Conventional Commits 1.0.0 specification while still reading like normal human commit messages.

Default to a commit message that can be pasted directly into Git.

## Core Format

Use this shape:

```text
<type>[optional scope][!]: <description>

[optional body]

[optional footer(s)]
```

Rules to preserve:

- Always include a type, a colon, and a space before the description.
- Add a scope only when it adds useful context.
- Use `!` immediately before the colon when the change is breaking.
- Put the body one blank line after the subject.
- Put footers one blank line after the body, or one blank line after the subject when there is no body.
- Keep `BREAKING CHANGE:` uppercase when used as a footer.

## Type Selection

Choose the smallest accurate type for the actual change:

- `feat`: new user-facing or API-facing functionality
- `fix`: bug fix or correctness repair
- `docs`: documentation-only change
- `refactor`: code restructuring with no intended behavior change
- `test`: test-only change
- `perf`: measurable performance improvement
- `build`: build tooling or dependency packaging change
- `ci`: CI or automation workflow change
- `chore`: maintenance work that does not fit better elsewhere
- `style`: formatting or whitespace-only change
- `revert`: reverting a previous change

`feat` and `fix` are the only types with SemVer meaning in the spec. The others are common extensions and are fine to use when they fit better.

## Scope Guidance

- Keep scope short and noun-based, such as `api`, `cli`, `auth`, or `docs`.
- Do not force a scope when the change is cross-cutting or the scope would be vague.
- Prefer one scope over a long, comma-like subject.

## Description Guidance

- Keep the subject short, specific, and implementation-aware.
- Prefer lowercase after the colon unless a proper noun or acronym needs capitals.
- Describe what changed, not why the author is happy about it.
- Avoid a trailing period.
- Avoid filler such as `update stuff`, `misc fixes`, or `WIP`.

## Body And Footer Guidance

Add a body only when the subject alone is not enough. Use it for:

- motivation that matters to future readers
- concise implementation notes
- important operational or migration context

Add footers only when they carry real metadata, such as:

- `BREAKING CHANGE: ...`
- `Refs: #123`
- `Reviewed-by: Name`

If the change is breaking:

- include `!` in the subject, or
- include a `BREAKING CHANGE:` footer, or
- include both when the subject should flag the break and the footer should explain migration impact

## Working Style

- Infer the message from the actual diff, staged changes, or user summary when available.
- If multiple unrelated changes are mixed together, prefer suggesting a split into separate commits before forcing one misleading message.
- If the user asks for a single commit message, return the final message with minimal extra commentary unless they ask for explanation or alternatives.
- When useful, provide two or three strong subject alternatives rather than many weak ones.

## Examples

```text
feat(auth): add SSO callback validation
```

```text
fix(cli): handle empty config path
```

```text
docs: clarify local plugin installation steps
```

```text
feat(api)!: remove legacy token endpoint

BREAKING CHANGE: clients must migrate to the OAuth device flow endpoint.
```
