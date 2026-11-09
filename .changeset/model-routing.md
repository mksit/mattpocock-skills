---
"mattpocock-skills": major
---

`setup-matt-pocock-skills` now writes `.agents/docs/models.md`, which routes each subagent role to a model and each implementer to a tier. `implement-spec` reads it, passes each role's model and effort on every dispatch, and re-dispatches an implementer that fails twice one tier higher.

The engineering skills' per-repo config moves from `docs/agents/` to `.agents/docs/`. `setup-matt-pocock-skills` offers to `git mv` an existing `docs/agents/` into place, and checks that `.agents/docs/` isn't gitignored.

The domain docs and the local tracker move under `.agents/docs/` too: `GLOSSARY.md`, `GLOSSARY-MAP.md` and `adr/` (per-context ones under `src/<context>/.agents/docs/`), and local-markdown issues under `.agents/docs/scratch/`. `setup-matt-pocock-skills` offers to `git mv` each legacy location into place.
