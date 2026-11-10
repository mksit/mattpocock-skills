# Design red flags

<!-- Adapted from skills/architect/references/design-red-flags.md in pstack (MIT); see THIRD_PARTY_NOTICES.md. -->

Screen every shape against these flags before the advisor chooses. A flag is a reason to revise or drop the shape. A shape may keep a flag only when the design record names it as an accepted trade-off.

Assume the next contributor is an agent. It sees only the files it opened, copies the nearest example, and takes the shortest path that compiles. Prefer the shape in which a change that looks right from one file is right for the whole repo.

## Shallow module

The interface is nearly as large as what it hides. Look for these signs:

- Callers coordinate several calls to complete one operation.
- Public options expose internal stages or implementation choices.
- Learning the interface does not save the caller from learning the implementation.

Do not confuse a deep module with a deep call chain. A deep call chain spreads understanding across layers; a deep module concentrates capability behind one interface.

## Information leakage

Several modules depend on the same internal decision: a representation, a policy or a protocol detail shows up in more than one place, so changing it takes coordinated edits. Re-exporting transport or wire types on the interface is leakage. Parse external data into domain types behind the seam, and keep storage schemas, framework objects and protocol details private.

## Temporal decomposition

The modules follow execution order (load, validate, transform, save) instead of the knowledge they own, so one representation and its invariants repeat across several seams. Group code by the decisions it protects. Code that runs at different times can still belong in one module.

## Pass-through

A function forwards its arguments to another function of the same shape. It adds a layer and hides nothing. Delete it, or move the work to the module that can finish the operation. Keep a forwarding seam only when it adds policy, adaptation, or a distinct abstraction.

## Split ownership

More than one module writes the same state or keeps its own copy of it. An agent that edits one writer cannot see the others, so their rules drift apart. Give each piece of state one owner; the other modules read it, or ask the owner to change it.

## Two ways to do one task

The shape offers more than one way to do the same task. An agent copies whichever way it finds first, so every way keeps gaining callers. Keep one way.

## Reachable internals

A caller can import a module's internals. An agent takes the shortest path that compiles, so it imports them directly and they become part of the interface. Make internals unreachable from outside the module, so that such an import fails the build.

## Hand-synced lists

Two or more places list the same items, so adding an item means editing every list, and an agent that sees one list updates only that one. Keep one list and derive the others from it. If a list cannot be derived, make the build fail when the lists disagree.
