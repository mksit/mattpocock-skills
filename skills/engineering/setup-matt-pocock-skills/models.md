# Model routing

Which model, and at what reasoning effort, each subagent role runs on in this repo. `implement-spec` reads this file for its exploration, implementer, merger and fix subagents. The other roles apply whenever a session dispatches that kind of subagent. The dispatcher passes `model` and `effort` on each dispatch. The plugin's two role agents (`analyst`, `implementer`) leave their model to this file; a role here picks the model, the agent picks the rules.

A value is a model alias (`haiku`, `sonnet`, `opus`, `fable`), optionally with an effort (`sonnet @medium`; levels: `low`, `medium`, `high`, `xhigh`, `max`). `inherit` runs the role on the dispatching session's model. A comma-separated list is a panel: one subagent an entry. Delete a line to make that role `inherit`. A harness that cannot choose a subagent's model ignores this file.

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
- design-it-twice designers: opus, fable, sonnet
- judgment advisor: fable @medium

## Implementers, by tier

An implementer (or any subagent that changes code) takes the tier of its ticket:

- trivial: sonnet @low
- standard: sonnet @medium
- hard: opus @high
- judgment: fable @medium

What puts a ticket in each tier:

- **trivial**: the change is fully stated and mechanical. Docs, config, renames, file moves, deleting code with no callers, running a check and reporting it.
- **standard**: a feature or refactor with a clear spec and an existing pattern to follow, including multi-file mechanical edits.
- **hard**: a wrong change can pass its tests silently, or the cause is unknown. A bug fix with no known root cause, performance work, concurrency, the build system, data or history migrations.
- **judgment**: the ticket leaves a design choice open. Settle the choice before dispatch where you can; use this tier only where you can't.

## Routing rules

- Tier a ticket by what it touches, not by its length. A short ticket in a hard area is hard.
- When two tiers fit, take the higher one.
- An implementer that fails its acceptance twice is re-dispatched one tier higher, never retried at the same tier.
- A merger that cannot finish a merge cleanly is re-dispatched as `merger with conflicts`.
- A running subagent keeps its model. A new tier applies to the next dispatch.
