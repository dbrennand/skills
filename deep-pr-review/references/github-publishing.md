# Publishing a grouped GitHub review with `gh`

## Why `gh api` instead of `gh pr review`

`gh pr review --comment --body ...` submits a review body, but it does not take an array of line-specific inline comments. This skill needs **one** review that combines a summary findings table with all inline feedback, so use the REST create-review endpoint through `gh api`:

`POST /repos/{owner}/{repo}/pulls/{pull_number}/reviews`

## Preflight

1. Reconfirm the write target, `gh auth status`, and write access. Never print or reveal the auth token.
2. Recheck reviews and inline comments (`gh api --paginate`) for feedback or findings added during analysis. Never publish unchanged findings again on a re-review.
3. Refresh the PR head SHA (`gh pr view "$PR" --repo "$REPO" --json headRefOid`) and full patch (`gh pr diff "$PR" --repo "$REPO"`) immediately before preparing anchors. Do not post if the head changes before submission.
4. For each finding, verify `path` and actual blob `line` on the current diff. Use `RIGHT` for added/new-side context lines, `LEFT` for removed/old-side lines. Do **not** confuse actual line numbers with diff positions.
5. For multi-line comments, add `start_line` and `start_side` if supported. If fewer than one valid inline finding exists, a body-only review is acceptable.

## Prepare the request

Use a JSON serializer (for example Python's `json.dump` or `jq -n`) to create a UTF-8 JSON file **outside the repository**. Don't interpolate arbitrary PR content into unquoted shell commands or hand-build JSON through string concatenation. Example structure, with **illustrative** values only:

```json
{
  "commit_id": "<current head SHA>",
  "body": "<prepared review summary>",
  "event": "COMMENT",
  "comments": [
    {
      "path": "src/example.py",
      "line": 42,
      "side": "RIGHT",
      "body": "<prepared inline finding comment>"
    }
  ]
}
```

- Default event is `COMMENT`. Only use `REQUEST_CHANGES` for substantiated merge blockers when the user has requested a review with that outcome; only `APPROVE` when requested and justified. Do not make a formal approval merely because no findings were detected.
- Pass the prepared summary and comments without changing their findings or IDs. `comments[]` contains one comment for each finding with a valid inline anchor; any summary-only findings remain referenced in the summary.
- Inspect the JSON and ensure it targets the right repo and PR and includes no credentials or other inadvertent sensitive data.

## Submit once

```bash
REPO='OWNER/REPO'
PR=123
PAYLOAD='/path/to/review.json'

gh api --method POST "repos/$REPO/pulls/$PR/reviews" \
  --input "$PAYLOAD" \
  --jq '{id,html_url,state,commit_id}'
```

Verify a successful response and the review's URL/ID; when needed, inspect the review and comments with `gh api`. Report what was posted. If the command times out or reports an unclear outcome, **check for an existing review first** before retrying; avoid duplicate notifications.

## Failure and permissions

If `gh` is missing, unauthenticated, read-only, or permissions are insufficient, leave the review in draft form. Another GitHub integration may be used only if it supports a single grouped review. Never claim success without the API response.

References: https://cli.github.com/manual/gh_pr_view ; https://cli.github.com/manual/gh_api ; https://docs.github.com/en/rest/pulls/reviews
