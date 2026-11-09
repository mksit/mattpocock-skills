---
"mattpocock-skills": minor
---

The plugin ships two role agents: `analyst`, which only reads (exploration, review, research), and `implementer`, which changes the repo (tickets, fixes, merges). `implement-spec`, `code-review` and `research` dispatch them, and fall back to general-purpose subagents on a harness without plugin agents. The `analyst` can't spawn agents, which closes the research re-delegation loop.
