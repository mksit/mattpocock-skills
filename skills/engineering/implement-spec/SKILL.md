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

Start every subagent with a fresh context window. Dispatch each one as the plugin's role agent named below (`analyst` reads, `implementer` changes the repo, `fable-advisor` judges); on a harness without them, use a general-purpose subagent with the same brief. A role agent carries the standing rules, so a brief holds only the pointers and what differs for this ticket.

Communication to and from subagents should be sparse. Communicate primarily through **context pointers**: to the spec, tickets, research notes, and previous commits. Don't duplicate information already available via pointers.

**Implementer subagents** should be run in the background where possible for maximum concurrency.

## Autonomy

The run is **autonomous**: it works the whole graph to the final gate without handing the next move to the user.

- A design choice the spec leaves open is the `fable-advisor` agent's to settle. Record its verdict on the ticket and carry on.
- Work found outside the spec becomes a new ticket on the tracker, carrying what was found, and the run carries on. A new ticket that blocks the spec joins the graph; any other waits for the user.
- For `tdd`, the spec's seams are pre-agreed; a seam the spec lacks is agreed by the `fable-advisor` agent.
- The run stops for the user only where:
  - the repo's instructions reserve an action to the user (a merge into the default branch, a push, a publish), unless they authorize it in advance;
  - the final gate says **Rethink**;
  - a `hard` implementer has failed its ticket twice and the `fable-advisor` agent cannot name the cause.
- The closing report lists every verdict the `fable-advisor` agent gave and every ticket the run opened, so the user reviews them in one pass.

## Steps

1. Read the spec and tickets to understand the task graph. A ticket that leaves a design choice open goes to the `fable-advisor` agent first; record its verdict on the ticket. Give each ticket its tier from the model routing, by what the ticket touches, not by its length.

2. (optional) Use an **exploration subagent** (the `analyst` agent, `exploration` role) to conduct any exploration required by the tickets - relevant codebase files or external documentation. Ensure the exploration subagent can save files - it should save its markdown notes in a directory outside the repo, accessible by all future subagents. This lets **implementer subagents** focus on implementation rather than exploration.

3. Create the integration branch. If the issue tracker closes work through PRs, or the user asks for one, open a draft PR after the first merge in step 5 (a branch with no commits ahead of main can't open one), marked as closing the spec and tickets.

4. Use a fresh **implementer subagent** (the `implementer` agent) for each ticket, each in its own worktree on its own branch, on the model of the ticket's tier. An implementer that fails its ticket twice is re-dispatched one tier higher, never retried at the same tier; a `hard` one is re-dispatched only after the `fable-advisor` agent has named the cause. No implementer runs on Fable. Each implementer subagent:
   - confirms its worktree is based on the integration branch before starting, and resets onto it if not;
   - calls the Skill tool with `tdd` to build the ticket;
   - merges the integration branch tip into its own branch before reporting done

5. Once an **implementer subagent** completes, merge its work to the integration branch with a **merger subagent** (the `implementer` agent with a merge brief, `merger` role). If it cannot finish the merge cleanly, re-dispatch it as `merger with conflicts`, with a brief that allows it to resolve conflicts.

6. If this changes the **frontier** of available tickets, kick off more **implementer subagents** to work on the new tickets. This allows for maximum concurrency.

7. Once all tickets are complete, call the Skill tool with `code-review` on the integration branch, with its merge-base with the default branch as the fixed point and this spec as its spec. Fix all issues raised by the code review in a single fresh **implementer subagent** (the `review fix implementer` role).

8. Dispatch the `fable-advisor` agent as the **final gate**: the integration branch's diff against the spec, with the verification evidence. On **Fix first**, fix every item with one fresh `review fix implementer`, then run this step again. On **Rethink**, stop and bring its reason to the user. Done when its verdict is **Ship**.

9. If a draft PR exists, mark it ready for review. Otherwise, resolve each ticket the way the issue tracker closes work. Report the integration branch with the list the Autonomy section names.

10. Clean up all **implementer subagent** worktrees.
