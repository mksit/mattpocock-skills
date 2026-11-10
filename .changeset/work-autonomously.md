---
"mattpocock-skills": minor
---

New model-invoked principle, `principle-work-autonomously`: once the user hands over the work, the agent decides, acts and reports, and never lets an action reserved to the user block the work: it sets each aside, carries on, and asks for all of them at once only when nothing else can run (with a **Rethink** gate or a fact only the user holds). Work outside the task is taken on only with the user's leave; otherwise it is listed in the report, with no ticket and no work. It settles a subagent's open choices and resumes it, and turns a finding that leaves work open into the next probe, fix or ticket. The rule used to live only in `implement-spec`'s Autonomy section, so the agent asked the user outside that skill. `implement-spec` now calls the principle and keeps only its spec-specific rules; `diagnosing-bugs` calls it for its loop; `tdd` and the model routing point at it.
