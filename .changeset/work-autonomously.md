---
"mattpocock-skills": minor
---

New model-invoked principle, `principle-work-autonomously`: once the user hands over the work, the agent decides, acts and reports, and stops only for an action the repo reserves to the user, a **Rethink** gate, or a fact only the user holds. It settles a subagent's open choices and resumes it, and turns a finding that leaves work open into the next probe, fix or ticket. The rule used to live only in `implement-spec`'s Autonomy section, so the agent asked the user outside that skill. `implement-spec` now calls the principle and keeps only its spec-specific rules; `diagnosing-bugs` calls it for its loop; `tdd` and the model routing point at it.
