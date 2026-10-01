---
name: work
description: Work one Yokka card end to end with the Yokka MCP tools. Claim it, read the brief, write a checklist, report progress, ask the human on the card when blocked, and complete it with a summary and the pull request link. Use when the user asks you to pick up, work on or finish a card (like "YK-12"), to take the next card from their board, or to do work that should be tracked on Yokka.
argument-hint: "[card, like YK-12; leave out to take the next card]"
---

# Work a Yokka card

Card named for this run, if any: $ARGUMENTS

You work this card through the Yokka MCP tools (`claim_card`, `get_card`, `set_checklist`...). The board is
how the human follows your work live, so what you say there matters as much as the code. If the Yokka tools
aren't available, say so and stop: don't do the work off the board.

## Before you start

- **Which project.** If the repo's `AGENTS.md` or `CLAUDE.md` has a "Task tracking: Yokka" section, it names
  the project: pass it as `project` to every tool that takes one. Otherwise call `list_projects`, and ask the
  human if more than one could fit.
- **Which card.** If a card was named (above, or in the human's request), use it. If not, call `claim_card` without a card: it finds and
  claims the next card waiting in one step (pass `swimlane` or `epic` if the human named one). If nothing is
  waiting, say so and stop.
- **Project notes.** `get_card` and `claim_card` bring the notes that fit this card, any handoff note the
  last agent left, and what the cards it was blocked by delivered. Read them before you plan. `find_notes`
  searches the rest. When you learn something the next agent will need (a decision, a gotcha, where things
  live), save it with `add_note`, or fix an outdated one with `update_note`.

## The loop

1. **Claim** the card with `claim_card` when you start on it, not a batch ahead. If you were given an `as` name
   (by `introduce`, or by the agent that started you), pass it as `as`.
2. **Read** the full brief, checklist, attachments and linked pull requests with `get_card`. Items already on
   the checklist were written by the human: they are the acceptance criteria.
3. **Plan** with `set_checklist` before you change anything: 3 to 10 short steps, in order. Keep the human's
   items and work through them. Only a one-line fix may skip this.
4. **Work**, and keep the board honest:
   - `check_items` as each step is really done.
   - `report_progress` at real milestones, one short line each. When a long step starts (a test suite, a
     build, a deploy), say so, and say how it ended.
   - Any call keeps your claim alive; it lapses after the project's stale time without one (30 minutes by
     default), so report during long steps.
   - Handing part of the card to a subagent? Give it a short name, have it claim with that name as `as`, and
     have it call `check_items` and `report_progress` on the card itself.
5. **Ask** with `request_input` when you're blocked on a decision only the human can make, so the question
   shows on the card and not only in your chat. Offer 2 to 6 `options` when the answer is a choice. Then wait:
   call `get_card_activity` with the cursor it returned and `wait_seconds: 50`, and call it again if it times
   out. If the human answers you in this chat instead, record it with `comment_card` and
   `answers_question: true` so the card stops waiting on them.
6. **Show it works** when you can: `attach_file` a screenshot, a short recording or a test report.
7. **Complete** with `complete_card`: a one or two sentence summary of what changed, and `links` to the pull
   request (or the commits) and any preview. If you know what the work cost, send your totals with
   `report_usage` first.

If you can't or shouldn't finish, `release_card` with a one-line reason, and a `handoff` note telling the next
agent what's done, what's left and what to watch out for. File follow-up work you find with
`add_card` rather than doing it now; if cards must happen in order, give the later ones `blocked_by`.

## Before you end your turn

Leave every card you hold completed, released, waiting on the human with `request_input`, or with a
`report_progress` line saying what it waits on ("e2e suite running"). `stop_check` lists any you would leave
hanging: if your app doesn't run it for you at the end of each turn, call it yourself before you stop. After a
restart, `find_cards` with `held_by_me` lists the cards you still hold.

Never report work you haven't done.
