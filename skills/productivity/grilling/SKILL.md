---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **decision tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Format a round like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Word each question so "yes" accepts your recommended answer. When a wrong answer would be costly to undo (a migration, a data or history change), get the recommended answer from the **advisor subagent** (the `fable-advisor` agent).

Grilling settles **what** and **why**, never **shape**. Never ask or answer a question about a module, an interface, a data shape or a seam: write it down as a **shape question** for `to-design`, and keep grilling the rest of the tree.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent with a fresh context window to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the decision tree visited, nothing left silently assumed. Before you call it empty, dispatch the advisor with every settled decision and the code they rest on. Put each gap or contradiction it names back on the frontier. Do not act on it until the user confirms you have reached a shared understanding.

Then call the Skill tool with "to-design". Pass it the settled decisions and every shape question. Grilling always hands off to `to-design`; it never designs and never goes straight to a spec or to code.
