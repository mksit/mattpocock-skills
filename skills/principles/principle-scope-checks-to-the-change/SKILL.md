---
name: principle-scope-checks-to-the-change
description: "Apply before choosing which checks to run on a change, before writing a check list into a subagent's brief, and before adding a test. Run only the checks the change can move, and test only what it adds or fixes."
---

# Scope Checks to the Change

A check earns its cost only when the change can move its result. A check the change cannot move proves nothing, and it spends time, machines and a reviewer's attention. Running it is not caution; it is waste.

**Why:** Defensive over-checking feels safe and costs a lot: a deletion of dead code waits an hour for end-to-end comparisons that could not differ, and a brief that says "run everything" makes every agent after it run everything too.

## Name what the change reaches

Read the diff and name the paths it reaches before choosing a single check:

| The diff reaches | Checks |
| --- | --- |
| Documents, comments, agent instructions | format and lint |
| Code nothing reads (a deletion), or new code nothing calls yet | the test suites, lint, the build |
| One component's path | the above, and that component's end-to-end checks |
| A path every component shares | the above, and every end-to-end check |

When you cannot tell what a path reaches, trace it; do not widen the checks to cover the doubt.

## Do not repeat a result

A check already run on an earlier commit keeps its result when the diff from that commit cannot move it: a merge of an unrelated commit, a documentation follow-up, a reordering a test already pins. Rerun only what the new diff reaches; a conflict resolved in two test files reruns those two files.

## Test what the change adds

A new test covers a behaviour the change adds or a defect it fixes. No test asserts again what another test, a type or an end-to-end comparison already holds.

## Briefs carry the scope

A brief to a subagent names its checks by this table, not "every check". The subagent's report names each check it skipped and why.
