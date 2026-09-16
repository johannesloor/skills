---
name: visualize
description: Draw anything a team needs to look at together onto a shared canvas.
disable-model-invocation: true
argument-hint: "<where to visualize> [what to visualize]"
---

You are building something a team will stand in front of and argue with, and a picture carries that conversation where a paragraph stalls it. Every board is built to be **squinted** at: at a zoom where the words are unreadable, the argument still follows from shape, position, size and colour alone. Overview and connection points are the job; fine detail stays in the source document.

## Settle where and what

Ask for whatever the invocation left out. **Where** is a link to a board or a design file. **What** is whatever the user wants on it — a plan, a shipped implementation, a roadmap, an architecture, a retrospective, a ticket, an idea that exists only in this conversation.

Source material is whatever holds it: a named file, something agreed in this session, an issue thread, a repository you read yourself. Tell the user in one line which source you are reading and what you are drawing from it, then start without waiting for a reply.

## Hold the tense

A plan is forward-looking and conditional, its unknowns open and every branch live. A result is settled, one branch taken and the rest closed. Decide which the user asked for before drawing, because the wrong one still looks correct.

Drawing a plan, treat the implementation as unknown even when it sits in front of you in full: an uncertainty the plan carried stays an uncertainty on the board, every branch drawn, whatever you know about which one fired.

Asked for both, draw them on one page, plan left and result right, each headed with its label. Where one clean event turned one into the other, connect them and name it.

## Reach the canvas

Work through whatever MCP server serves the linked tool. When none is attached, search the vendor's current MCP documentation, walk the user through configuring it, then continue.

## Read the target before drawing

Check only whether the destination holds anything, not what it holds. Reading the existing style before you know you need it spends context on material you may throw away.

Then ask the user how the board should look, and where it goes when the destination is occupied: a new page, or somewhere else. When there is existing work, offer inheriting its **house style** as one of the options. Read that style only once they choose it — open the destination properly and learn its palette, spacing habits and the shapes people reach for. Any other answer, draw fresh and owe the existing work nothing.

Now read [`references/medium-craft.md`](references/medium-craft.md) for the canvas in front of you.

## Lay out the board

Open with a **summary card**: three or four sentences, no more. Why the board exists, and one stroke of how it is arranged. Anything longer gets cut back, because a summary needing its own summary has failed.

Draw the fewest boxes the argument needs. Most material rides on a handful of shapes and the arrows between them, so pitch it high-level and add detail only where the argument stops following without it. The bar is someone tracing it in one pass.

Break the source into **units**, each headed, visually distinct, numbered so a room can say "look at 3". Take the unit from the shape the material already has:

- a **band** per section, for a document that arrives in sections
- a **timeline**, for anything carrying dates, phases or sequence
- a **swimlane**, for work split across people, teams or services
- a **journey**, for something a person moves through step by step
- a **map**, for a system's parts and the connections between them

Mix them freely where the material is mixed — a timeline of phases above a swimlane of who owns what, beside two bands of open questions. Number every top-level unit in one sequence whatever its type, so the reading order survives someone dragging a piece of it. Every part of the source lands in some unit: shorten by compressing inside a unit, never by leaving material off the board.

Compress by translating into shape. A paragraph describing three stages and a dependency between two of them is three boxes and an arrow, and what survives the translation becomes labels on it. Keep a prose card only for material that genuinely resists shape — a unit needing a paragraph to explain itself is a diagram you have not found yet.

Squinting works when meaning rides in every channel the canvas offers:

- **size**, for importance or magnitude
- **position**, for sequence or rank
- **proximity**, for what belongs with what
- **connectors**, for dependency and flow
- **a repeated icon**, for a kind that recurs across the board
- **colour**, along one axis

Colour is the fastest channel and the easiest to waste, so give it one meaning and hold it across the whole board. For a decision or a diagnosis that means one tone each for blocked, kept, works-but-costs, open question, and neutral. For material that is not triage-shaped, colour along whatever axis it actually varies on — theme, team, phase, confidence. Either way give the conclusion a distinct treatment so the eye lands there last, and where the house style is inherited, map onto its colours.

Add the diagrams the source could not draw — what prose spreads across several paragraphs, a picture states at once. Reach for the shape the argument takes:

- a **bind**, several routes side by side each ending in a dead end, showing the options are exhausted
- a **decision tree**, branching, with the condition written on each edge
- a **pipeline**, stages with directional flow
- a **before and after**, two states side by side
- a **fork**, the option taken beside the option rejected
- a **story map**, user activities across the top, detail hanging beneath each
- an **impact and effort matrix**, for a field of options that needs ranking
- a **dependency graph**, for what blocks what
- a **stakeholder map**, for who cares about this and how much sway they hold

Carry images wherever one exists, because a real screenshot or design frame settles in a glance what a labelled box only gestures at. Start with what the material already holds — screenshots, diagrams, recordings, mockups attached to the plan, the pull request, the issue, or shared earlier in this conversation — then hunt the ones it does not: the design file's frame, a capture of the running thing, the diagram on the web page explaining the concept the board leans on. Put each in the unit it belongs to, sized to read at the zoom that unit is read at.

Draw unknowns as unknowns. A question with three possible answers looks like one: an explicit unknown marker, a branch per outcome, the condition on each edge. Where a human decision interrupts the flow, draw a gate: a shape across the path saying work stops here until a person decides.

## Verify until it is clean

Build one unit per operation and check it before starting the next.

Checking means three passes. Query the bounds of every element and confirm nothing overlaps a heading or a neighbour and no text is clipped by its container. Then render the board as an image and look at it, because a structural check passes happily on something ugly. Then squint: where the argument does not survive, the board is carrying in text what it ought to carry in form.

Repeat until every element passes all three. When a defect resists fixing, name it precisely and hand it back — a named unresolved overlap is worth more than a board reported as finished.

## Hand over

Give the user the link. When the work is coupled to an issue, ticket or pull request, ask whether to post the link there too.
