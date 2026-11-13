## What it does

`to-design` decides the **shape** of a change before anyone writes code: its modules, interfaces, data shapes and seams. It uses the vocabulary of [codebase-design](./codebase-design.md), and it hands back a one-page **design record**. It does not write an implementation and it does not edit code.

It works in seven steps:

1. **Ground.** An analyst subagent reads every module the change touches.
2. **Frame.** It shows you the constraints the shape has to meet.
3. **Sketch.** A panel of designers, running in parallel, each draws one radically different shape under a different constraint.
4. **Screen.** Each shape is checked against a list of design red flags.
5. **Choose.** `fable-advisor` picks a base shape and says what to borrow from the others.
6. **Agree.** You accept the shape, or push back.
7. **Record.** It writes the design record.

The idea comes from Ousterhout's "design it twice": your first shape is unlikely to be your best, so every record weighs at least two.

## When it runs

`to-design` runs after every grilling session. Grilling settles what you are building and why, and it designs nothing: each time it meets a question about shape (a module, an interface, a data shape, a seam), it writes that question down and moves on. When the grilling is done, it passes the settled decisions and the shape questions to `to-design`. [to-spec](./to-spec.md) then takes its shape and seams from the design record. If `to-spec` finds no record, it runs `to-design` first, and so does [to-tickets](./to-tickets.md) when its work is not a spec.

You can also call it yourself ("design this"), and [improve-codebase-architecture](./improve-codebase-architecture.md) hands it a deepening candidate when you want alternative interfaces.

## Where it fits

```txt
grilling → to-design → to-spec → to-tickets → implement
```

If you push back on the shape, `to-design` starts again from the grounding step. If you push back on what or why, it reopens grilling for that branch, then comes back. `to-design` never implements anything.

## It's working if

- It reads the code before it draws anything, and every constraint it lists comes with a file:line.
- The shapes are genuinely different from each other, not small variations on one shape.
- It writes each caller's usage before the interface.
- It shows you why each rejected shape lost.
- The design record you get back is one page, uses your project's terms, and contains no implementation steps.

## Common questions

**Does it work outside Claude Code?**
Only partly. Step 3 depends on dispatching parallel subagents, and some harnesses can't do that. On such a harness the designers run one after another, which is slower. [Issue #564](https://github.com/mattpocock/skills/issues/564) tracks the portability of parallel subagents.

**Where did codebase-design's `DESIGN-IT-TWICE.md` go?**
It became this skill. `codebase-design` now holds only vocabulary, and the process that used to live there has a checkpoint before anything is settled.
