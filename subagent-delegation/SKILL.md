---
name: subagent-delegation
description: Delegate independent work to subagents and verify the results.
---

# Subagent delegation

Use subagents to split work that can be completed independently. Keep the parent responsible for scope, coordination, integration, and the final result. Every subagent result must be independently verified before it is accepted or reported as complete. This skill describes the workflow, not a particular agent runtime.

## When to use

Use when a task has two or more independent workstreams, or when a focused investigation can be done without the parent's full context.

Do not use for:

- A small edit that is faster to complete directly
- Work that requires ongoing user interaction
- Dependent steps that must share an evolving decision or result
- Several agents editing the same files without an isolation strategy

## Decide whether to delegate

Delegate only when the expected time saved exceeds the cost of writing context, starting workers, and reviewing their output. Prefer one well-scoped worker over several overlapping workers.

Before dispatching, identify:

- The final outcome and acceptance criteria
- Which workstreams are independent
- Which results another workstream depends on
- The files, services, or environment that workers may change
- How the parent will verify each result

## Prepare each task

Give every worker a self-contained brief. Do not make it infer requirements from the parent conversation.

```text
Goal: <one concrete outcome>

Context:
- <relevant repository, files, decisions, and constraints>

Scope:
- In scope: <explicit boundaries>
- Out of scope: <things not to change>

Acceptance criteria:
- <checkable condition>
- <checkable condition>

Expected report:
- Changed files or artifact identifiers
- Commands run and their relevant output
- Open questions, blockers, or unverified claims
```

Include exact paths, versions, interfaces, and user decisions when they matter. Keep unrelated history out of the brief. If a worker needs another worker's result, dispatch it only after that result is available.

## Dispatch

1. Group only genuinely independent tasks into one parallel dispatch.
2. Keep dependent tasks in an explicit sequence.
3. Use isolated worktrees, branches, or directories when more than one worker may edit code.
4. Limit each worker to one clear responsibility.
5. Set an output contract when the runtime supports structured results.
6. In Hermes, use `delegate_task` for bounded child work. Use a durable scheduler or background process instead when the work must survive the parent session.

Do not ask multiple workers to solve the same problem unless comparison is the goal. If comparison is the goal, define the decision criteria before dispatching.

## Collect and integrate

Treat worker reports as evidence to inspect, not as proof that the task is complete.

For each worker:

1. Check that the reported files or artifacts exist.
2. Read the relevant diff or output yourself.
3. **Run the relevant tests, checks, or queries yourself in the parent environment.** If a criterion cannot be independently verified, mark it unverified.
4. **Compare every result with the acceptance criteria.**
5. Record any missing evidence or unresolved blocker.

Integrate only verified results. Resolve overlapping edits before combining them. If a worker changed code, inspect the complete diff and run the affected test suite after integration.

## Failure handling

- If a worker fails, preserve the error and decide whether to retry, complete the work directly, or remove that workstream.
- If one parallel task fails, do not report the batch as successful.
- If a worker returns no artifact, treat the task as incomplete even when its narrative sounds confident.
- If results conflict, return to the stated acceptance criteria and inspect the underlying evidence.
- Stop and ask the user before expanding scope, making irreversible external changes, or resolving an ambiguous product decision on their behalf.

## Final report

Report the final outcome, not the dispatch activity. Include:

- What was completed
- Which files or external artifacts changed
- Verification commands and results
- Any partial completion, skipped work, or remaining blocker

Never claim success from a worker's report alone. The parent must have a concrete verification result for every acceptance criterion.
