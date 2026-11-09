---
"mattpocock-skills": major
---

`setup-matt-pocock-skills` writes `.agents/docs/models.md`, a model routing for subagent roles and implementer tiers. `implement-spec` reads it and passes each role's model and effort on every dispatch, re-dispatching a twice-failed implementer one tier higher.

The engineering skills' per-repo config moves from `docs/agents/` to `.agents/docs/`. `setup-matt-pocock-skills` offers to `git mv` an existing `docs/agents/` into place, and checks that `.agents/docs/` is not gitignored.

The domain docs and the local tracker move under `.agents/docs/` too: `GLOSSARY.md`, `GLOSSARY-MAP.md` and `adr/` (per-context ones under `src/<context>/.agents/docs/`), and local-markdown issues under `.agents/docs/scratch/`. `setup-matt-pocock-skills` offers to `git mv` each legacy location into place.
