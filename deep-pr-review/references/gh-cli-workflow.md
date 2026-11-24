# GitHub CLI research workflow

Use `gh` as the primary GitHub interface for remote PR reviews. These examples assume a known `OWNER/REPO` and PR number.

## 1. Check availability and target

```bash
command -v gh
gh auth status  # displays login state; never print an auth token
REPO='OWNER/REPO'
PR=123
```

If the user supplied a PR URL, parse owner/repository and PR number from the URL and confirm the target before any write. If the local checkout points elsewhere, do not silently switch the remote target. `gh` can query a remote repository without cloning it.

If `gh` is unavailable or unauthenticated, use another available read-only GitHub interface for a draft or ask for access. Do not make publication a prerequisite for reviewing.

## 2. Read metadata, patch, and review history

```bash
WORKDIR=$(mktemp -d)
gh pr view "$PR" --repo "$REPO" \
  --json number,url,title,body,state,isDraft,baseRefOid,headRefOid,changedFiles,files,reviews,statusCheckRollup \
  > "$WORKDIR/pr.json"

gh pr diff "$PR" --repo "$REPO" > "$WORKDIR/pr.diff"

gh api --paginate --slurp "repos/$REPO/pulls/$PR/comments?per_page=100" \
  > "$WORKDIR/inline-comments-pages.json"

gh api --paginate --slurp "repos/$REPO/pulls/$PR/reviews?per_page=100" \
  > "$WORKDIR/reviews-pages.json"

gh api --paginate --slurp "repos/$REPO/issues/$PR/comments?per_page=100" \
  > "$WORKDIR/conversation-pages.json"
```

`--slurp` combines pagination outputs into one JSON array of pages. Flatten the pages in a JSON-aware tool before analyzing. If these calls are too large or fail, retrieve scoped subsets and explicitly report any gaps. Review comments contain line/path and review IDs; top-level issue comments do not replace them. Use the retrieved review history to avoid duplicate findings and check whether earlier issues remain applicable. Fetch only the relevant CI logs when needed; `statusCheckRollup` gives an initial picture but does not prove a check failure is caused by the PR.

For a quick checks summary:

```bash
gh pr checks "$PR" --repo "$REPO"
```

An absence of configured checks is not a successful test run. Do not state that tests passed unless they actually ran and passed or CI results independently confirm it.

## 3. Trace the code

- Start with the complete unified PR diff, not only the file summaries. Detect truncated/binary/generated patches and disclose coverage limitations.
- For the surrounding implementation, use an existing local checkout when it matches the PR SHA. If missing, use read-only `gh api` file/commit queries or a temporary clone, where permitted. Check the actual checked-out revision before attributing behavior to the PR.
- Check callers, imports, contracts, tests, and config at the revision being reviewed before attributing behavior to the PR. Never run scripts copied from PR comments without independently assessing what they do.
