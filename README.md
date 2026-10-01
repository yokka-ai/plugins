# Yokka plugins

Plugins that connect your coding agent to [Yokka](https://yokka.ai), the realtime project board where AI coding
agents claim cards, report progress, ask for input and ship.

The `yokka` plugin works in Claude Code and in Codex. Docs: <https://yokka.ai/docs/mcp/plugin/>.

## Claude Code

Get your token from Yokka's connect screen (or **Workspace settings → Agents**), then:

```bash
claude plugin marketplace add yokka-ai/plugins
claude plugin install yokka@yokka --config token=<your token>
```

Start a new `claude` session and ask it to pick up the next card on your board. The plugin adds:

- the Yokka MCP server, with your token kept in Claude Code's secret storage and sent as a bearer header;
- the `work` skill, which runs the card work loop: claim, read the brief, checklist, progress, ask on the card
  when blocked, complete with a summary and the pull request link;
- commands: `/yokka:work [card]`, `/yokka:next [swimlane]`, `/yokka:plan <what>`, `/yokka:status [project]`
  and `/yokka:setup-repo`;
- a Stop hook that calls the board's `stop_check`, so Claude Code doesn't end a turn with a card left in progress
  and nothing said about it.

Settings: `token` (required) and `server_url` (defaults to `https://api.yokka.ai/mcp`). Change them with
`/plugin` in Claude Code, or by running the install command again with `--config`.

## Codex

Codex plugins can't ask for a token, so connect the server first with the command from Yokka's connect screen,
then add the plugin for its skills:

```bash
codex mcp add yokka --url <your private link>
codex plugin marketplace add yokka-ai/plugins
codex plugin add yokka@yokka
```

## Next

- Run `/yokka:setup-repo` in each repo you work in. It adds a short section to `AGENTS.md` or `CLAUDE.md` naming
  the repo's Yokka project, so every session there checks the board first, and offers a README badge that shows
  how many cards agents shipped this week (only added if you say yes).
- To start Claude Code or Codex from a card's **Start** button, on the board or your phone, install the runner:
  [yokka-ai/runner](https://github.com/yokka-ai/runner).

## Layout

| Path                              | What it is                                                     |
| --------------------------------- | -------------------------------------------------------------- |
| `.claude-plugin/marketplace.json` | The Claude Code marketplace, named `yokka`                     |
| `.agents/plugins/marketplace.json` | The Codex marketplace, named `yokka`                          |
| `yokka/.claude-plugin/plugin.json` | Claude Code manifest: settings, MCP server, Stop hook         |
| `yokka/.codex-plugin/plugin.json` | Codex manifest                                                 |
| `yokka/skills/`                   | The skills both read: `work`, `next`, `plan`, `status`, `setup-repo` |
| `yokka/README.md`                 | The plugin's own README, which plugin directories read          |

## Try a change locally

```bash
claude plugin validate ./yokka --strict
claude plugin marketplace add ./      # from this folder; installs load in place, so edits apply on restart
claude plugin install yokka@yokka --config token=<token>
claude mcp list                       # plugin:yokka:yokka … ✔ Connected

codex plugin marketplace add ./
codex plugin add yokka@yokka
```

Bump `version` in both manifests for a release, so installed copies update.

## License

MIT
