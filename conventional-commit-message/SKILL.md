---
name: conventional-commit-message
description: "Use when the user wants to write, rewrite, or tighten a Git commit message into Conventional Commits 1.0.0 format, including requests like `write a commit message`, `make this a conventional commit`, or `help me word this commit`. Produces a commit subject in `<type>[optional scope][!]: <description>` form and adds a body or footers only when the change needs that extra context."
---

# Conventional Commit Message

Write commit messages that follow the [Conventional Commits 1.0.0 specification](https://www.conventionalcommits.org/en/v1.0.0/). Default to a message ready to paste into Git.

## Format

```text
<type>[optional scope][!]: <description>

[optional body]

[optional footer(s)]
```

- Always include a type, colon, and space before the description.
- Add a scope only when it adds useful context.
- Use `!` before the colon for breaking changes.
- Body goes one blank line after the subject; footers one blank line after the body (or after the subject when there is no body).
- Keep `BREAKING CHANGE:` uppercase as a footer.

## Types

| Type | Use for |
| --- | --- |
| `feat` | New user-facing or API-facing functionality |
| `fix` | Bug fix or correctness repair |
| `docs` | Documentation-only change |
| `refactor` | Restructuring with no behavior change |
| `test` | Test-only change |
| `perf` | Measurable performance improvement |
| `build` | Build tooling or dependency packaging |
| `ci` | CI or automation workflow |
| `chore` | Maintenance that fits nowhere better |
| `style` | Formatting or whitespace-only |
| `revert` | Reverting a previous change |

`feat` and `fix` are the only SemVer-meaningful types; the rest are common extensions.

## Guidance

- **Scope**: short, noun-based (`api`, `cli`, `auth`). Skip when vague or cross-cutting.
- **Subject**: short and specific; lowercase after the colon unless a proper noun or acronym needs capitals; describe what changed, not why; no trailing period; avoid filler like `update stuff` or `WIP`.
- **Body**: only when the subject alone is not enough — motivation, implementation notes, migration context.
- **Footers**: only for real metadata — `BREAKING CHANGE: ...`, `Refs: #123`, `Reviewed-by: Name`.
- **Breaking**: include `!` in the subject, or a `BREAKING CHANGE:` footer, or both when the subject flags the break and the footer explains migration impact.

## Working Style

- Infer the message from the actual diff or staged changes when available.
- If unrelated changes are mixed, suggest splitting into separate commits first.
- Return the final message with minimal commentary unless the user asks for explanation or alternatives.
- Offer two or three strong subject alternatives when useful.

## Examples

```text
feat(auth): add SSO callback validation
fix(cli): handle empty config path
docs: clarify local plugin installation steps
```

```text
feat(api)!: remove legacy token endpoint

BREAKING CHANGE: clients must migrate to the OAuth device flow endpoint.
```
