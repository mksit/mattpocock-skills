---
name: implement-spec
description: "Implement the result of /to-spec and /to-tickets in code."
disable-model-invocation: true
---

You have been provided a spec. This spec should have tickets associated with it, describing how to implement the spec.

The issue tracker should have been provided to you. If not, tell the user to run `/setup-matt-pocock-skills`.

The **model routing** (`.agents/docs/models.md`, named in the `## Agent skills` block) should also have been provided. It names the model and effort for each subagent role, and the **tier** each implementer gets from its ticket. Pass the role's `model` and `effort` on every dispatch. If there is no model routing, run every subagent on your own model, and tell the user once that `/setup-matt-pocock-skills` can add one.

The goal is the entire spec implemented on a single **integration branch**, with every ticket resolved the way the issue tracker closes work.

The tickets are not a list of steps. They are a **task graph** with blocking relationships between them. This means there is always a **frontier** of tickets which are ready to be grabbed.

Start every subagent with a fresh context window. Dispatch each one as the plugin's role agent named below. The `analyst` agent reads, the `implementer` agent changes the repo, and the **advisor subagent** (the `fable-advisor` agent) judges. If your harness doesn't have them, use a general-purpose subagent with the same brief. The role agent already carries the standing rules, so the brief only needs the pointers and what's different about this ticket.

Communication to and from subagents should be sparse. Communicate primarily through **context pointers**: to the spec, tickets, research notes, and previous commits. Don't duplicate information already available via pointers.

**Implementer subagents** should be run in the background where possible for maximum concurrency.

## Autonomy

Work **autonomously**: take the whole graph through to the final gate without handing the next move back to the user.

**Why:** Since code changes are reversible and reviewable, a wrong decision usually costs less than blocking.

- When a design choice is open, settle it yourself, even if the user is watching. If you already have a recommendation, take it, and give the reason in one line. If you don't, have the advisor settle it. Record the decision on the ticket and carry on.
- When a subagent hands back open choices instead of finished work, settle them the same way, and resume it with the decisions.
- When you find work outside the spec, open a new ticket on the tracker describing what you found, and carry on.
- Add a found ticket to the graph only if one of the spec's acceptance criteria can't pass without it. Leave every other found ticket off the graph for the user. The graph never grows past the spec.
- For `tdd`, treat the spec's seams as pre-agreed. When the spec is missing a seam, have the advisor agree it.
- Stop for the user only when:
  - the repo's instructions reserve an action to the user (a merge into the default branch, a push, a publish), and they haven't authorized it in advance
  - the final gate says **Rethink**
  - a `hard` implementer has failed its ticket twice, and the advisor can't name the cause
- In the closing report, list every design decision (yours and the advisor's) and every ticket you opened, so the user can review them in one pass.

## Steps

1. Read the spec and tickets to understand the task graph. If a ticket leaves a design choice open, settle it before dispatch, as **Autonomy** says. Give each ticket a tier from the model routing, based on what it touches, not how long it is.

2. (optional) Use an **exploration subagent** (the `analyst` agent, in the `exploration` role) to conduct any exploration required by the tickets - relevant codebase files or external documentation. Ensure the exploration subagent can save files - it should save its markdown notes in a directory outside the repo, accessible by all future subagents. This lets **implementer subagents** focus on implementation rather than exploration.

3. Create the integration branch. If the issue tracker closes work through PRs, or the user asks for one, open a draft PR after the first merge in step 5 (a branch with no commits ahead of main can't open one), marked as closing the spec and tickets.

4. Use a fresh **implementer subagent** (the `implementer` agent) for each ticket, each in its own worktree on its own branch, running on the model for its ticket's tier. If an implementer fails its ticket twice, call the Skill tool with "principle-attack-the-premise", then re-dispatch it one tier higher. Never retry it at the same tier. If a `hard` one fails twice, wait until the advisor has named the cause before you re-dispatch it. Never run an implementer on Fable. Each implementer subagent:
   - confirms its worktree is based on the integration branch before starting, and resets onto it if not;
   - calls the Skill tool with `tdd` to build the ticket;
   - merges the integration branch tip into its own branch before reporting done

5. Once an **implementer subagent** completes, merge its work to the integration branch with a **merger subagent** (the `implementer` agent with a merge brief, in the `merger` role). If it can't finish the merge cleanly, re-dispatch it in the `merger with conflicts` role, with a brief that lets it resolve conflicts. Call the Skill tool with "principle-prove-it-works" before you update the frontier.

6. If this changes the **frontier** of available tickets, kick off more **implementer subagents** to work on the new tickets. Before you dispatch them, call the Skill tool with "principle-separate-before-serializing-shared-state" on the files the frontier tickets will write. This allows for maximum concurrency.

7. Once all tickets are complete, call the Skill tool with "principle-prove-it-works" on the integration branch head. Then call the Skill tool with `code-review` on the integration branch. Use its merge-base with the default branch as the fixed point, and this spec as the spec. Fix all issues raised by the code review in a single fresh **implementer subagent** (in the `review fix implementer` role).

8. Dispatch the advisor as the **final gate**. Give it the integration branch's diff against the spec, and the verification evidence. If it says **Fix first**, fix every item in one fresh **implementer subagent** (in the `review fix implementer` role), then run this step again. If it says **Rethink**, stop and take its reason to the user. This step is done when the advisor says **Ship**.

9. If a draft PR exists, mark it ready for review. Otherwise, resolve each ticket the way the issue tracker closes work. Report the integration branch, with the closing list from the Autonomy section.

10. Clean up all **implementer subagent** worktrees.
