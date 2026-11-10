# Design record

<!-- Adapted from skills/architect/references/rationale-template.md in pstack (MIT); see THIRD_PARTY_NOTICES.md. -->

The record that `architect` hands back. One page. Use the project's `GLOSSARY.md` terms and the `codebase-design` vocabulary. Replace each italic note with content.

## Questions

*The settled decisions from grilling that this design rests on, and each shape question it settles, one line each.*

## Constraints

*What the grounding found that the shape must honour: types it interoperates with, callers it cannot break, invariants that cross its seam. Give file:line for each.*

## Usage

*The caller's view, written before the interface: two or three real call sites. The interface below is derived from these. When the two disagree, change the interface to match the usage.*

## Shape

*The chosen shape. Give the data structures first, then the interface: types, signatures, invariants, ordering and error modes. Then give the seams and what each one hides. Say where validation lives and what the shape deliberately does not do. State its depth: what the interface hides, and why it is no larger than it must be.*

## Choice

*The advisor's verdict. Name the base shape, what was grafted from the others, and the risk that decided the choice.*

## Trade-offs accepted

*One bullet per trade-off, in the form "we accept X in exchange for Y". Include every red flag kept on purpose, and anything a later reader could mistake for an oversight.*

## Rejected shapes

*Each rejected shape in one line: its idea, and why it lost on depth, locality or seam placement. If the constraints forced the shape, write "the only viable shape, because …" and name the constraint.*

## Open questions

*What the user still has to weigh in on, each written as a question.*
