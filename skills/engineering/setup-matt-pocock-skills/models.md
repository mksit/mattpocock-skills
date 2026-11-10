# Model routing

This file says which model, at which reasoning effort, each subagent role runs on in this repo.

`implement-spec` reads it for its exploration, implementer, merger and fix subagents. The other roles apply whenever any session dispatches that kind of subagent. Pass the role's `model` and `effort` on every dispatch.

The plugin's two role agents (`analyst`, `implementer`) leave their model to this file. This file picks the model, and the agent brings the rules.

Each value is a model alias (`haiku`, `sonnet`, `opus`, `fable`), with an optional effort (`sonnet @medium`). The effort levels are `low`, `medium`, `high`, `xhigh` and `max`.

- `inherit` runs the role on the dispatching session's model.
- A comma-separated list is a panel: one subagent per entry.
- Delete a line to make that role `inherit`.

If the harness can't choose a subagent's model, ignore this file.

## Fixed roles

- exploration: sonnet @medium
- merger: sonnet @low
- merger with conflicts: opus @medium
- review fix implementer: opus @high
- code-review standards reviewer: sonnet @medium
- code-review spec reviewer: opus @high
- research: sonnet @medium
- research, novel or contested: opus @high
- wayfinder research: sonnet @medium
- architecture walker: opus @high
- architect designers: opus, fable, sonnet
- fable-advisor: fable @medium

## Implementers, by tier

An implementer, or any other subagent that changes code, runs on its ticket's tier:

- trivial: sonnet @low
- standard: sonnet @medium
- hard: opus @high

Never run an implementer on Fable. Fable is for judging, not for writing code.

Pick the tier from what the ticket is:

- **trivial**: the change is fully stated and mechanical. Docs, config, renames, file moves, deleting code with no callers, running a check and reporting it.
- **standard**: a feature or refactor with a clear spec and an existing pattern to follow, including multi-file mechanical edits.
- **hard**: a wrong change can pass its tests silently, or the cause is unknown. A bug fix with no known root cause, performance work, concurrency, the build system, data or history migrations.

An open design choice doesn't make a ticket a tier of its own. It's settled before dispatch, as `principle-work-autonomously` says. Then the ticket takes the tier of what it touches.

## Routing rules

- Tier a ticket by what it touches, not by how long it is. A short ticket in a hard area is hard.
- When two tiers fit, take the higher one.
- If an implementer fails its acceptance twice, re-dispatch it one tier higher. Never retry it at the same tier.
- If a `hard` implementer fails twice, don't re-dispatch it until the advisor has named the cause.
- If a merger can't finish a merge cleanly, re-dispatch it as `merger with conflicts`.
- A running subagent keeps its model. A new tier applies from the next dispatch.
