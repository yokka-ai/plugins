---
name: next
description: Take the next card waiting on the Yokka board and work it to the end.
argument-hint: "[swimlane or epic, like Bugs]"
disable-model-invocation: true
---

# Take the next card

Swimlane or epic to take it from, if any: $ARGUMENTS

1. Work out the project as the `work` skill says (the repo's "Task tracking: Yokka" section, or
   `list_projects`).
2. Call `claim_card` without a card. It finds the next card waiting and claims it in one step, so no other
   agent takes it in between. Pass the swimlane or epic named above as `swimlane` or `epic`; if you can't tell
   which it is, `get_board` lists both.
3. If nothing is waiting, say so in one line and stop, unless the human asked you to keep going (step 5).
4. Otherwise work the card exactly as the `work` skill describes: read the brief, set the checklist, report
   progress, ask on the card when blocked, and complete it with a summary and the pull request link.
5. When it's done, say in one line what the next card waiting is (`get_next_card`). Take it only if the human
   asked you to keep working until they stop you: then call `claim_card` without a card and with
   `wait_seconds: 50`. It claims the next card as soon as one is ready; if it times out, call it again.
