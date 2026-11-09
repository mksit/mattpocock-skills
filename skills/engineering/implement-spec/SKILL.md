---
name: implement-spec
description: "Implement the result of /to-spec and /to-tickets in code."
disable-model-invocation: true
---

You have been provided a spec. This spec should have tickets associated with it, describing how to implement the spec.

The issue tracker should have been provided to you. If not, tell the user to run `/setup-matt-pocock-skills`.

The **model routing** (`.agents/docs/models.md`, named in the `## Agent skills` block) should also have been provided. It names the model and effort for each subagent role, and the **tier** an implementer takes from its ticket. Pass the role's `model` and `effort` on every dispatch. If the repo has no model routing, run every subagent on your own model, and tell the user once that `/setup-matt-pocock-skills` adds one.

The goal is the entire spec implemented on a single **integration branch**, with every ticket resolved the way the issue tracker closes work.

The tickets are not a list of steps. They are a **task graph** with blocking relationships between them. This means there is always a **frontier** of tickets which are ready to be grabbed.

Start every subagent with a fresh context window. Dispatch each one as the plugin's role agent named below (`analyst` reads, `implementer` changes the repo); on a harness without them, use a general-purpose subagent with the same brief. A role agent carries the standing rules, so a brief holds only the pointers and what differs for this ticket.

Communication to and from subagents should be sparse. Communicate primarily through **context pointers**: to the spec, tickets, research notes, and previous commits. Don't duplicate information already available via pointers.

**Implementer subagents** should be run in the background where possible for maximum concurrency.

## Steps

1. Read the spec and tickets to understand the task graph. Give each ticket its tier from the model routing, by what the ticket touches, not by its length.

2. (optional) Use an **exploration subagent** (the `analyst` agent, `exploration` role) to conduct any exploration required by the tickets - relevant codebase files or external documentation. Ensure the exploration subagent can save files - it should save its markdown notes in a directory outside the repo, accessible by all future subagents. This lets **implementer subagents** focus on implementation rather than exploration.

3. Create the integration branch. If the issue tracker closes work through PRs, or the user asks for one, open a draft PR after the first merge in step 5 (a branch with no commits ahead of main can't open one), marked as closing the spec and tickets.

4. Use a fresh **implementer subagent** (the `implementer` agent) for each ticket, each in its own worktree on its own branch, on the model of the ticket's tier. An implementer that fails its ticket twice is re-dispatched one tier higher, never retried at the same tier. Each implementer subagent:
   - confirms its worktree is based on the integration branch before starting, and resets onto it if not;
   - calls the Skill tool with `tdd` to build the ticket;
   - merges the integration branch tip into its own branch before reporting done

5. Once an **implementer subagent** completes, merge its work to the integration branch with a **merger subagent** (the `implementer` agent with a merge brief, `merger` role). If it cannot finish the merge cleanly, re-dispatch it as `merger with conflicts`, with a brief that allows it to resolve conflicts.

6. If this changes the **frontier** of available tickets, kick off more **implementer subagents** to work on the new tickets. This allows for maximum concurrency.

7. Once all tickets are complete, call the Skill tool with `code-review` on the integration branch. Fix all issues raised by the code review in a single fresh **implementer subagent** (the `review fix implementer` role).

8. If a draft PR exists, mark it ready for review. Otherwise, resolve each ticket the way the issue tracker closes work, and report the integration branch.

9. Clean up all **implementer subagent** worktrees.
