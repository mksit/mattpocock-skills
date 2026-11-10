# Principles

Judgment rules the workflow skills call at the step they govern. Each is copied verbatim from [pstack](https://github.com/michael-denyer/pstack-claude) (MIT, Copyright (c) 2026 Lauren Tan).

## Model-invoked

Reached by the model, or by a workflow skill that calls the Skill tool with its name.

- **[principle-prove-it-works](./principle-prove-it-works/SKILL.md)**: Verify every task output against the real artifact (run the feature, read the actual value, inspect the diff), not a proxy, a self-report, or "it compiles". Adapted from pstack.
- **[principle-attack-the-premise](./principle-attack-the-premise/SKILL.md)**: When two or more fixes that share one premise have failed the same gate, write the premise down and test it before the next fix. Adapted from pstack.
- **[principle-fix-root-causes](./principle-fix-root-causes/SKILL.md)**: When debugging, fix the first place the system went wrong, not the place the symptom surfaced; a guard is a fix only when the state it handles is legitimate. Adapted from pstack.
- **[principle-subtract-before-you-add](./principle-subtract-before-you-add/SKILL.md)**: When evolving a system, remove complexity first, then build: no speculative validators or guards, and delete what carries no novel content. Adapted from pstack.
- **[principle-sequence-verifiable-units](./principle-sequence-verifiable-units/SKILL.md)**: Order work as small units that each end in a checkable state, and stack commits in the order that proves the work. Adapted from pstack.
- **[principle-separate-before-serializing-shared-state](./principle-separate-before-serializing-shared-state/SKILL.md)**: When concurrent actors might share mutable state, eliminate the shared write target first; serialize structurally only when one writer is a real invariant. Adapted from pstack.
