---
"mattpocock-skills": minor
---

New model-invoked skill, `architect`: it settles the shape of a change before any code. It grounds the change in the code it touches, has the `architect designers` panel sketch radically different shapes in parallel, screens them against design red flags, and has `fable-advisor` choose. It returns a design record and writes no implementation.

The main flow is now `grilling → architect → to-spec`. `grilling` settles what and why and designs nothing: it writes down every shape question (a module, an interface, a data shape, a seam) and always ends by handing off to `architect`. `to-spec` takes its shape and seams from the design record, and runs `architect` first when there is none. The design-it-twice pattern moves out of `codebase-design` into `architect`, so `codebase-design` is vocabulary only. The model-routing role `design-it-twice designers` is renamed `architect designers`.
