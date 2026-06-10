---
name: pr-body-markdown
description: "Use when drafting or updating a GitHub pull request body or description and the user asks for something like `write the PR description`, `draft the pull request body`, `help me open a PR`, or `summarize this for reviewers`. Produces a concise summary, grouped implementation details, and a clear test plan with accurate scope and testing status."
---

# PR Body Markdown

## Goal

Write PR bodies that feel polished and human: easy to scan, detailed enough for reviewers, and explicit about testing.

## Trigger Examples

Use this skill for requests like:

- `write the PR description`
- `draft the pull request body`
- `summarize this for reviewers`
- `help me open a PR`
- `turn these changes into a PR description`

## Default Shape

Prefer this structure unless the repo clearly uses a different convention:

## Summary

- Keep this short.
- Use one short paragraph or two to four bullets.
- Explain the outcome and main scope of the change.
- Mention the user, operational, or maintenance impact when relevant.
- Include linked issues only when they help with context.

## Details

- Use this section for the concrete changes.
- Group bullets by workflow, component, feature area, or file only when that grouping improves scanability.
- Add `###` subheadings when there are distinct change areas worth separating.
- Start bullets with strong verbs such as `Add`, `Remove`, `Update`, `Align`, or `Refactor`.
- Mention file paths or workflow names in backticks when helpful, but do not force every bullet into a file-by-file format.
- Include rationale inline when it helps the reviewer understand why a detail matters.

## Test Plan

- Always include this section.
- Use task list items so reviewers can quickly scan what was run and what still needs confirmation.
- Use `- [x]` for checks that were actually completed.
- Use `- [ ]` for expected CI checks, manual verification, or follow-up testing that has not completed yet.
- Mention exact commands, workflows, or reviewer-visible signals when known.
- If no local testing was run, say so plainly in this section.

## Optional Sections

- Add `## Review Notes`, `## Risks`, or `## Follow-ups` only when they carry real information.
- Omit optional sections rather than filling them with boilerplate.
- Avoid a separate `Why` section unless the motivation is substantial enough that it does not fit naturally into `Summary` or `Details`.

## Style Rules

- Prefer plain technical language over marketing phrasing.
- Keep spacing clean: blank line after headings, then the paragraph or list.
- Avoid giant paragraphs and avoid one bullet per trivial edit.
- Do not restate obvious diff details line by line.
- Do not add filler, generic checklists, or generated-by signatures unless the user explicitly asks for them.
- Do not claim testing, review notes, or follow-up work that did not happen.

## Example Shape

```md
## Summary
This PR aligns the certification workflows with the execution environments they are meant to validate against. It also removes redundant sanity job configuration and makes the expected Python baseline explicit.

## Details

### `certification-reusable.yml`
- Remove the per-version sanity input so `ansible-test` can cover supported Python versions automatically.
- Hard-code the supported stable branches used by downstream execution environments.
- Add a pre-test step that writes `tests/config.yml` when the collection does not provide one.

### Calling workflows
- Update the reusable workflow callers to stop passing the removed input.
- Refresh comments and examples so the documented interface matches the workflow behavior.

## Test Plan
- [x] `zizmor` passes locally.
- [ ] CI passes for the PR workflow matrix.
- [ ] Sanity jobs run for each supported stable branch.
```
