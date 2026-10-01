---
name: setup-repo
description: Add a "Task tracking: Yokka" section to this repo's AGENTS.md or CLAUDE.md, naming the Yokka project the repo belongs to.
argument-hint: "[project]"
disable-model-invocation: true
---

# Connect this repo to a Yokka project

Project, if named: $ARGUMENTS

Every agent already gets Yokka's instructions when it connects. What the server can't know is which project
this repo belongs to, so this section says it.

1. Find the project: the one named above, or call `list_projects` and pick the one that matches this repo. If
   more than one could fit, ask the human which.
2. Put the section below in the instructions file read at the start of every session here (`AGENTS.md` or
   `CLAUDE.md`; create `AGENTS.md` if there is neither). Fill in the project's name and slug. If the file
   already has a Yokka section, replace it instead of adding a second one. Keep the section as written and
   don't change anything else in the file.
3. Never put a token or a private link in it: the file gets committed.

```markdown
## Task tracking: Yokka

Work in this repo is tracked on the Yokka board, project "<project name>" (`<project-slug>`), through the `yokka` MCP server. Pass `<project-slug>` to tools that take a project.

- Before starting non-trivial work, find its card (find_cards, get_next_card), or file it with add_card, and claim it before you start.
- For everything else, follow the Yokka server's instructions.
- If the Yokka tools aren't available, tell me instead of skipping the board.
```
