---
name: pr-body-markdown
description: Use when drafting or updating a GitHub pull request body and the user wants a consistent Markdown format that is useful for reviewers. Applies Summary, What, Why, optional Review Notes, and Validation sections with concise, accurate content.
---

# PR Body Markdown

## Overview

Use this skill when creating or revising a pull request body so it is concise, readable, and useful for reviewers.

## Format

Write the PR body in Markdown with these top-level headings:

## Summary

- Limit this section to three sentences maximum.
- Explain the change at a high level and focus on the outcome.
- Mention the main user, operational, or maintenance impact when relevant.

## What

- Use a flat bullet list.
- Describe the concrete changes that were made.
- Group by logical change area when that is clearer than listing files one by one.
- For very small PRs, per-file bullets are acceptable when they add clarity.
- If referencing files, start each bullet with the file path in backticks.

## Why

- Explain the reason for the change in a few sentences or short bullets.
- State the maintenance, bug fix, security, feature, or operational motivation directly.
- Do not make the reviewer infer intent from the diff.

## Review Notes

- Include this section only when there is something worth flagging for reviewers.
- Use it for risk areas, rollout concerns, migration impact, or places that deserve closer manual review.
- Omit it entirely when there are no notable review callouts.

## Validation

- List only checks that were actually run.
- Explicitly mention any testing that was performed.
- If no validation was run, say so plainly.
- If validation was partial, state what was covered, including testing performed, and what was not.

## Style

- Prefer plain technical language over marketing phrasing.
- Do not restate obvious diff details line by line.
- Do not add filler content, checklist items, or generic background unless the user explicitly asks for them.
- Do not claim review notes or validation that do not exist.

## Example Shape

```md
## Summary
This PR updates the dependency pins used by the container build and repository workflows. It keeps the runtime image current and aligns the workflow references with newer approved versions.

## What
- `Dockerfile`: Bumps the builder and runtime `caddy` base images to the newer release.
- `.github/workflows/ci.yml`: Updates the pinned `actions/checkout` and `github/codeql-action` references used in CI.
- `.github/workflows/snyk.yml`: Updates the same pinned workflow references used in the scheduled Snyk scan.

## Why
These updates fold in open dependency maintenance changes and keep the container and workflow dependencies current.

## Review Notes
- The changes are limited to dependency pins and container base image versions; reviewer attention should focus on workflow compatibility and image update risk.

## Validation
- No local testing was performed.
```
