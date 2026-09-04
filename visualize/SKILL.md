---
name: visualize
description: Put a plan, an implementation, or both onto a shared canvas for a team to discuss. Use when visualising work onto a linked board or design file.
disable-model-invocation: true
argument-hint: "<where to visualize> [plan | implementation | both]"
---

You are building something a team will stand in front of and argue with. Overview and connection points carry that conversation; exact identifiers, reproduction steps and API minutiae stay in the source document. Once you know which kind of canvas you are drawing on, read [`references/medium-craft.md`](references/medium-craft.md).

## Settle where and what

Ask for whatever the invocation left out.

**Where** is a link to a board or a design file. **What** is one of three artifacts:

- **plan** — what was proposed, before the work
- **implementation** — what was built, after it
- **both** — the pair, on one page

Source material is whatever holds that artifact: a named file, the plan agreed in this session, or a repository you read yourself. Name the source back to the user in one line along with the artifact and its tense, then start work without waiting for a reply.

## Hold the tense

A plan is forward-looking and conditional, its unknowns open and every branch live. A result is settled and past, one branch taken and the rest closed. Drawing the wrong one is the most expensive mistake available here, because the output still looks correct.

When drawing a plan, treat the implementation as unknown even when it sits in front of you in full. An uncertainty the plan carried stays an uncertainty on the board, every branch drawn, whatever you happen to know about which one fired.

When the plan document has been rewritten in place, reconstruct it from the surrounding evidence — task lists, decision records, commit history, issue threads. Use that evidence to recover what was **proposed**, and let what it reveals about outcomes pass by. Write `reconstructed plan from implementation` on the board.

## Reach the canvas

Work through whatever MCP server serves the linked tool. When none is attached, search the vendor's current MCP documentation, walk the user through configuring it, then continue.

## Read the target before drawing

Open the destination and learn what is already there: pages, house palette, spacing habits, the shapes people already reach for. Match them.

An empty destination gets drawn on directly. A destination holding existing work gets a question first — a new page, or somewhere else.

Behaviour you have not seen before is cheap to probe and expensive to guess. Create one element, measure what it does — whether it grows with its content or clips it, where its origin sits, how text wraps — then delete it. Do this before laying out anything that depends on the answer.

Element types size themselves differently, often in opposite directions within the same tool: one grows with its content while another clips it and appends an ellipsis. Match the element type to the length of the content — short labels in constrained shapes, long prose in containers that grow. An element that grows collides with whatever sits beneath it, so vertical positions computed from intended heights are wrong the moment content expands past them. Use real layout containers where the tool has them, and otherwise measure rendered heights and reposition afterwards.

## Lay out the board

Give the source one **band** per section, each with a heading, each visually distinct, numbered so a room can say "look at 3". Compress inside a band as hard as the space demands: a ten-line paragraph becomes a three-line card. Every band survives; the compression happens within it.

Draw at least one **derived diagram** — something the prose states across several paragraphs that a picture states at once. Reach for the shape the material already has:

- a **bind**, where several routes each dead-end and the point is that they exhaust the options
- a **decision tree**, branching, with the condition written on each edge
- a **pipeline**, stages with directional flow
- a **before and after**, two states side by side
- a **fork**, the option taken beside the option rejected

Place any images the material already carries — screenshots, diagrams, recordings, mockups attached to the plan, the pull request, the issue, or shared earlier in this conversation. Put each one in the band it belongs to, sized to read at the zoom level the band is read at. Use what is in front of you and leave the user's attention alone.

Draw unknowns as unknowns. A question with three possible answers looks like a question with three possible answers: an explicit unknown marker, one branch per outcome, and the condition written on each edge. Where a human decision interrupts the flow, draw a gate — it says work stops here and waits for a person better than a sentence does.

Carry a palette where colour means something and keeps meaning it across the whole board: one tone for blocked or rejected, one for kept or succeeded, one for works-but-costs, one for open questions, one neutral, and a distinct treatment for the conclusion so the eye lands there last. Where the destination already has a house convention, map these onto its colours instead.

For **both**, put the plan on the left and the result on the right, in reading order on one page, named with ordinals so the sequence survives being zoomed out. A connector between them naming the event that turned one into the other earns its place when there is a clean event to name.

## Verify until it is clean

Build one band per operation and check it before starting the next.

Checking means both halves. Query the bounds of every element and confirm that nothing overlaps a heading, nothing overlaps a neighbour, and no text has been clipped by its container. Then render the board as an image and look at it, because a structural check passes happily on something ugly.

Repeat until every element passes both halves. When a defect resists fixing, name it precisely and hand it back — a named unresolved overlap is worth more than a board reported as finished.

## Hand over

Give the user the link.

When the work is coupled to an issue, ticket or pull request, post the link there too, so whoever finds the ticket next finds the board with it.
