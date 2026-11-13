---
name: to-design
description: Settle the shape of a change after grilling and before the spec. Ground it in the code it touches, sketch radically different shapes in parallel, screen them, and have the advisor choose. Use when grilling hands off its settled decisions and shape questions, when the user asks for a design, or when to-spec finds no design record.
---

# To Design

It runs between grilling and the spec:

```
grilling → to-design → to-spec
```

The issue tracker should have been provided to you. If not, tell the user to run `/setup-matt-pocock-skills`.

Everything `to-design` keeps lives on the issue tracker or in the conversation. Subagents return their findings in their reports, so their briefs name no notes file.

Grilling settles **what** and **why** with the user, and designs nothing. `to-design` settles **how**: the modules, interfaces, data shapes and seams of a change, in the vocabulary of `codebase-design` (call the Skill tool with "codebase-design" if it isn't loaded). Its input is grilling's settled decisions and **shape questions**. Its output is a **design record**, the input `to-spec` takes. `to-design` writes no implementation and changes no code.

It settles every shape question grilling handed it, in one design. Your first shape is unlikely to be the best (Ousterhout, "design it twice"), so the record always weighs at least two.

## Steps

1. **Ground.** Dispatch an `analyst` subagent (role `exploration`) to read every module the change touches. For each module, it reports:
   - file:line and what the module does today;
   - its callers;
   - the dependency category, per `codebase-design`'s DEEPENING.md;
   - the constraints a new shape must honour: types it interoperates with, callers it can't break, invariants that cross its seam.

   A shape built on a module nobody read is refuted by the first test. Skip this step only for greenfield work with nothing to integrate. **Done when:** every module the shape will touch is listed with its file:line.

2. **Frame.** Write the problem space for the user. Give the constraints, the dependencies, and a rough sketch that makes the constraints concrete; the sketch is not a proposal. Post it, then go straight to step 3, so the user reads it while the designers work. **Done when:** the frame is posted.

3. **Sketch.** Dispatch the `to-design designers` panel in parallel, each designer in a fresh context window. Each designer produces one shape under its own constraint:
   - "Minimise the interface: one to three entry points. Maximise leverage per entry point."
   - "Optimise for the most common caller: make the default case trivial."
   - "Maximise flexibility: support many use cases and extension."
   - If a dependency crosses a seam: "Design around ports and adapters."

   Each brief carries the grounding, the `codebase-design` vocabulary, and the project's `GLOSSARY.md` terms. Each designer returns these, in this order:
   1. the caller's usage, written first: two or three real call sites;
   2. the interface derived from that usage: types, signatures, invariants, ordering, error modes;
   3. what the implementation hides behind the seam;
   4. the dependency strategy and its adapters;
   5. the trade-offs: where leverage is high and where it is thin.

   The shapes must be whole-shape alternatives, not variants of one shape. **Done when:** at least two structurally distinct shapes are in hand. If two converge, dispatch one again with a sharper constraint.

4. **Screen.** Check every shape against [RED-FLAGS.md](RED-FLAGS.md). Revise or drop a shape that trips a flag, and say which flag. **Done when:** every surviving shape is clean, or its flag is named as an accepted trade-off.

5. **Choose.** Dispatch the **advisor subagent** (the `fable-advisor` agent) with the grounding, the surviving shapes and the constraints. It compares them on depth, locality and seam placement. It picks a base, names what to graft from the others, and names the risk that decides the choice. **Done when:** one shape is chosen, with its reason.

6. **Agree.** Show the user the chosen shape, the advisor's verdict, and each rejected shape with the reason it lost. Word the question so "yes" accepts the chosen shape. Pushback on the shape is new grounding: take it back to step 1. Pushback on what or why reopens grilling: call the Skill tool with "grilling" for that branch, then return here. **Done when:** the user accepts a shape.

7. **Record.** Write the design record per [DESIGN-RECORD.md](DESIGN-RECORD.md) and publish it to the issue tracker. `to-spec` and `to-tickets` take the shape and seams from it.

   A decision that should outlive the spec also becomes an ADR (call the Skill tool with "domain-modeling"). **Done when:** the record is on the issue tracker.

## When constraints force the shape

Sometimes the grounding leaves only one viable shape. Run step 3 anyway: a designer that cannot find a second shape is the evidence. The record then says "the only viable shape, because …", and names the constraint that rules out the others.
