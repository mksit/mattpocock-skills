# Principles

Judgment rules the workflow skills call at the step they govern. Each is adapted from [pstack](https://github.com/michael-denyer/pstack-claude); `THIRD_PARTY_NOTICES.md` lists the changes.

## Model-invoked

Reached by the model, or by a workflow skill that calls the Skill tool with its name.

- **[principle-prove-it-works](./principle-prove-it-works/SKILL.md)**: Verify every task output against the real artifact (run the feature, read the actual value, inspect the diff), not a proxy, a self-report, or "it compiles". Adapted from pstack.
- **[principle-attack-the-premise](./principle-attack-the-premise/SKILL.md)**: When two or more fixes that share one premise have failed the same gate, write the premise down and test it before the next fix. Adapted from pstack.
- **[principle-fix-root-causes](./principle-fix-root-causes/SKILL.md)**: When debugging, fix the first place the system went wrong, not the place the symptom surfaced; a guard is a fix only when the state it handles is legitimate. Adapted from pstack.
- **[principle-subtract-before-you-add](./principle-subtract-before-you-add/SKILL.md)**: When evolving a system, remove complexity first, then build: no speculative validators or guards, and delete what carries no novel content. Adapted from pstack.
- **[principle-sequence-verifiable-units](./principle-sequence-verifiable-units/SKILL.md)**: Order work as small units that each end in a checkable state, and stack commits in the order that proves the work. Adapted from pstack.
- **[principle-separate-before-serializing-shared-state](./principle-separate-before-serializing-shared-state/SKILL.md)**: When concurrent actors might share mutable state, eliminate the shared write target first; serialize structurally only when one writer is a real invariant. Adapted from pstack.
- **[principle-explain-the-number](./principle-explain-the-number/SKILL.md)**: Before you measure, and before you trust or report a measured number, name what limits it and rule out what else it could be measuring. Adapted from pstack.
- **[principle-laziness-protocol](./principle-laziness-protocol/SKILL.md)**: Bias toward deletion and the smallest change that solves the problem: consolidate each decision behind one source of truth, and minimize the diff. Adapted from pstack.
- **[principle-migrate-callers-then-delete-legacy-apis](./principle-migrate-callers-then-delete-legacy-apis/SKILL.md)**: When a new internal API replaces an old one, migrate every caller and delete the old API in the same wave instead of keeping a compatibility layer. Adapted from pstack.
- **[principle-boundary-discipline](./principle-boundary-discipline/SKILL.md)**: Put validation, type narrowing and error handling at system boundaries, trust internal code, and keep business logic in pure functions. Adapted from pstack.
- **[principle-make-operations-idempotent](./principle-make-operations-idempotent/SKILL.md)**: Design commands, lifecycle steps and processing loops to converge to the same end state however many times they run and wherever the last run stopped. Adapted from pstack.
- **[principle-model-the-domain](./principle-model-the-domain/SKILL.md)**: Encode the real domain in a data structure (a state machine, a typed model, a lookup table) instead of scattering it across conditionals. Adapted from pstack.
- **[principle-build-the-lever](./principle-build-the-lever/SKILL.md)**: When the work is not trivial, build the codemod, script, generator or delegate skill that does or proves it, instead of doing it by hand. Adapted from pstack.
- **[principle-encode-lessons-in-structure](./principle-encode-lessons-in-structure/SKILL.md)**: When you write the same instruction a second time, or see a recurring correction, encode the rule as a lint, flag, runtime check or script instead of more text. Adapted from pstack.
- **[principle-redesign-from-first-principles](./principle-redesign-from-first-principles/SKILL.md)**: When integrating a new requirement, redesign as if it had been a foundational assumption, and propagate the change through every reference. Adapted from pstack.
- **[principle-type-system-discipline](./principle-type-system-discipline/SKILL.md)**: Make the compiler reject impossible states: brand semantic primitives, parse external data at boundaries, exhaust variants, and derive types from authoritative schemas. Adapted from pstack.
- **[principle-experience-first](./principle-experience-first/SKILL.md)**: When implementation convenience conflicts with the experience of whoever consumes the work, choose their experience. Adapted from pstack.
- **[principle-work-autonomously](./principle-work-autonomously/SKILL.md)**: Once the user hands you the work, decide, act and report; stop only for an action the repo reserves to the user, a **Rethink** gate, or a fact only the user holds.
