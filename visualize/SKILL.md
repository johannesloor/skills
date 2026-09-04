---
name: visualize
description: Draw anything a team needs to look at together onto a shared canvas.
disable-model-invocation: true
argument-hint: "<where to visualize> [what to visualize]"
---

You are building something a team will stand in front of and argue with, and a picture carries that conversation where a paragraph stalls it. Every board is built to be **squinted** at: at a zoom where the words are unreadable, the argument still follows from shape, position, size and colour alone, and words only label what the shapes already carry. Overview and connection points are the job; the fine detail stays in the source document.

## Settle where and what

Ask for whatever the invocation left out.

**Where** is a link to a board or a design file. **What** is whatever the user wants on it — a plan, a shipped implementation, a roadmap, an architecture, a retrospective, a ticket, an idea that exists only in this conversation. Take what they name.

Source material is whatever holds it: a named file, something agreed in this session, an issue thread, or a repository you read yourself. Name the source back to the user in one line along with what you take it to be, then start work without waiting for a reply.

## Hold the tense

This section applies when what you are drawing is a plan or a result, and asking for both means the pair side by side on one page. A plan is forward-looking and conditional, its unknowns open and every branch live. A result is settled and past, one branch taken and the rest closed. Drawing the wrong one is the most expensive mistake available here, because the output still looks correct.

When drawing a plan, treat the implementation as unknown even when it sits in front of you in full. An uncertainty the plan carried stays an uncertainty on the board, every branch drawn, whatever you happen to know about which one fired.

When the plan document has been rewritten in place, reconstruct it from the surrounding evidence — task lists, decision records, commit history, issue threads. Use that evidence to recover what was **proposed**, and let what it reveals about outcomes pass by. Write `reconstructed plan from implementation` on the board.

## Reach the canvas

Work through whatever MCP server serves the linked tool. When none is attached, search the vendor's current MCP documentation, walk the user through configuring it, then continue.

## Read the target before drawing

Open the destination and learn what is already there: pages, house palette, spacing habits, the shapes people already reach for. Match them. Now that you know which kind of canvas you are drawing on, read [`references/medium-craft.md`](references/medium-craft.md).

An empty destination gets drawn on directly. A destination holding existing work gets a question first — a new page, or somewhere else.

Behaviour you have not seen before is cheap to probe and expensive to guess. Create one element, measure what it does — whether it grows with its content or clips it, where its origin sits, how text wraps — then delete it. Do this before laying out anything that depends on the answer.

Element types size themselves differently, often in opposite directions within the same tool: one grows with its content while another clips it and appends an ellipsis. Match the element type to the length of the content — short labels in constrained shapes, long prose in containers that grow. An element that grows collides with whatever sits beneath it, so vertical positions computed from intended heights are wrong the moment content expands past them. Use real layout containers where the tool has them, and otherwise measure rendered heights and reposition afterwards.

## Lay out the board

Break the source into **units**, each with a heading, each visually distinct, numbered so a room can say "look at 3". Take the unit from the shape the material already has:

- a **band** per section, for a document that arrives in sections
- a **timeline**, for anything carrying dates, phases or sequence
- a **swimlane**, for work split across people, teams or services
- a **journey**, for something a person moves through step by step
- a **map**, for a system's parts and the connections between them

Mix them freely on one board where the material is mixed — a timeline of phases above a swimlane of who owns what, beside two bands of open questions. Number every top-level unit in one sequence whatever its type, so the board keeps a reading order that survives someone dragging a piece of it.

Whatever the unit, the guarantee holds: every part of the source lands in one, compression happens inside a unit, and no unit is dropped.

Compress by translating into shape. A paragraph describing three stages and a dependency between two of them is three boxes and an arrow; what survives the translation becomes labels on it. Draw every unit this way wherever the material permits, and keep a prose card for the material that genuinely resists shape. When a unit needs a paragraph to explain itself, it is a diagram you have not found yet — find it, and let the paragraph become its labels.

Squinting works when meaning rides in every channel the canvas offers:

- **size**, for importance or magnitude
- **position**, for sequence or rank
- **proximity**, for what belongs with what
- **connectors**, for dependency and flow
- **a repeated icon**, for a kind that recurs across the board
- **colour**, along one axis

Colour is the fastest channel and the easiest to waste, so give it one meaning and hold it across the whole board. For a decision or a diagnosis that means one tone for blocked or rejected, one for kept or succeeded, one for works-but-costs, one for open questions, and one neutral. For material that is not triage-shaped, colour along whatever axis it actually varies on — theme, team, phase, confidence. Either way, give the conclusion a distinct treatment so the eye lands there last, and where the destination already has a house convention, map onto its colours instead.

Add the diagrams the source could not draw — what prose spreads across several paragraphs, a picture states at once. Draw every one the material earns. These are additions to the source rather than divisions of it, so reach for the shape the argument takes:

- a **bind**, where several routes each dead-end and the point is that they exhaust the options
- a **decision tree**, branching, with the condition written on each edge
- a **pipeline**, stages with directional flow
- a **before and after**, two states side by side
- a **fork**, the option taken beside the option rejected
- a **story map**, user activities across the top, detail hanging beneath each
- an **impact and effort matrix**, for a field of options that needs ranking
- a **dependency graph**, for what blocks what
- a **stakeholder map**, for who cares about this and how much sway they hold

Place any images the material already carries — screenshots, diagrams, recordings, mockups attached to the plan, the pull request, the issue, or shared earlier in this conversation. Put each one in the unit it belongs to, sized to read at the zoom level the unit is read at. Use what is in front of you and leave the user's attention alone.

Draw unknowns as unknowns. A question with three possible answers looks like a question with three possible answers: an explicit unknown marker, one branch per outcome, and the condition written on each edge. Where a human decision interrupts the flow, draw a gate — it says work stops here and waits for a person better than a sentence does.

When the ask is a plan and its result together, put the plan on the left and the result on the right, in reading order on one page, named with ordinals so the sequence survives being zoomed out. A connector between them naming the event that turned one into the other earns its place when there is a clean event to name.

## Verify until it is clean

Build one unit per operation and check it before starting the next.

Checking means three passes. Query the bounds of every element and confirm that nothing overlaps a heading, nothing overlaps a neighbour, and no text has been clipped by its container. Then render the board as an image and look at it, because a structural check passes happily on something ugly. Then squint. Where the argument does not survive, the board is carrying in text what it ought to be carrying in form.

Repeat until every element passes all three. When a defect resists fixing, name it precisely and hand it back — a named unresolved overlap is worth more than a board reported as finished.

## Hand over

Give the user the link.

When the work is coupled to an issue, ticket or pull request, post the link there too, so whoever finds the ticket next finds the board with it.
