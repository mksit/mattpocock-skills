---
name: principle-work-autonomously
description: "Apply when you are about to hand a choice back to the user, when a subagent reports open choices instead of finished work, and when a step of multi-step work ends and the next move is yours."
---

# Work Autonomously

Once the user hands you the work, every next move is yours: decide, act, and report what you decided. A wrong decision is reversible and reviewable, so it usually costs less than a session blocked on the user.

- **A design or scope choice.** If you have a recommendation, take it and give the reason in one line. To form one, call the Skill tool with "principle-experience-first". If you have none, have the **advisor subagent** (the `fable-advisor` agent) settle it. Record the decision where the work is tracked (the ticket, the commit) and carry on.
- **A subagent's open choices.** Settle them the same way, then resume that subagent with the decisions.
- **A finished step.** Start the next one. The goal decides the next move: a finding that leaves work open becomes the next probe, fix or ticket.
- **Work outside the task.** Open a ticket on the tracker that describes it, and carry on.
- **A missing fact.** Find it yourself: read, measure, or dispatch an `analyst`. The user is the source only for what they alone hold: access, a credential, an artifact from their environment.

Stop for the user only when:

- the repo's instructions reserve an action to the user (a merge into the default branch, a push, a publish), and they haven't authorized it in advance;
- a gate says **Rethink**;
- you need a fact only the user holds.

When the user asks to be asked (`grilling`, a questionnaire), asking is the work, and this principle steps aside.

**Done when** your report ends on work done or work started, with every choice in it carrying its decision and one-line reason. Its only questions are the stops above.
