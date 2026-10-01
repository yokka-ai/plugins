---
name: status
description: Summarize a Yokka board — what's in progress and who holds it, what waits on the human, and what's next. Read-only. Use when the user asks what's happening on their board or project.
argument-hint: "[project]"
---

# Board status

Project, if named: $ARGUMENTS

This only reads the board. Don't claim, move or change anything.

1. Work out the project: the one named above, the repo's "Task tracking: Yokka" section, or `list_projects`
   (with several and no hint, summarize each one briefly).
2. Read it with `get_board`, and `find_cards` with `needs_human` for the cards waiting on the human.
3. Answer in a few short lines, in this order:
   - **Needs you:** each card waiting on the human, with its question in one line.
   - **In progress:** each card being worked, who holds it, and its last progress line.
   - **Next up:** the card `get_next_card` would hand out, and how many more are waiting.
   - **Yours:** the cards you hold (`find_cards` with `held_by_me`), if any.
4. Name cards by their reference (like YK-12) so the human can find them. Skip a heading that has nothing
   under it.
