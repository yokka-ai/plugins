---
name: plan
description: Plan a feature, bug or goal into small Yokka cards, in order, without implementing anything.
argument-hint: "<what to plan>"
disable-model-invocation: true
---

# Plan work into cards

What to plan: $ARGUMENTS

If nothing was named, ask the human what to plan before you file anything.

1. Work out the project (the repo's "Task tracking: Yokka" section, or `list_projects`), then read the board
   with `get_board`: its swimlanes, lanes, labels, open epics and the cards already there. Don't file a card
   that already exists; `find_cards` checks.
2. Read enough of the code to plan for real, but don't change anything.
3. Split the work into 3 to 8 small cards that can each be finished and shipped alone. For each one, call
   `add_card` in the swimlane that fits, with a short title and a brief covering **What**, **Why** and
   **Done when**. Add labels the board already uses.
4. When cards have to happen in order, give each later card `blocked_by` with the cards it waits on, so agents
   pick them up in order and never start one too early.
5. If the plan is big enough to track as a whole, group the cards under an epic (`add_epic`, then `epic` on
   each card).
6. Finish with the list of cards you filed, by reference, and the order they go in. Do not implement
   anything.
