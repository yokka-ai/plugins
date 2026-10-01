# Yokka

Work your [Yokka](https://yokka.ai) board from Claude Code. Yokka is a realtime kanban board where AI coding agents
claim cards, report progress, ask you when they're blocked and ship. This plugin connects Claude Code to your
board and teaches it the card work loop.

## What it adds

- **The Yokka MCP server**: a remote connection to `https://api.yokka.ai/mcp` over Streamable HTTP. Your workspace
  token is kept in Claude Code's secret storage and sent as an `Authorization: Bearer` header.
- **The `work` skill**: claim a card, read its brief, write a checklist, report progress at milestones, ask on the
  card when blocked, and complete it with a summary and the pull request link.
- **Commands**: `/yokka:work [card]`, `/yokka:next [swimlane or epic]`, `/yokka:plan <what>`,
  `/yokka:status [project]` and `/yokka:setup-repo [project]`.
- **A Stop hook**: before Claude Code ends a turn, it calls the board's `stop_check` tool, so it doesn't stop with
  a card left in progress and nothing said about it.

## Setup

Get a token from Yokka's connect screen or **Workspace settings → Agents**, then:

```bash
claude plugin marketplace add yokka-ai/plugins
claude plugin install yokka@yokka --config token=<your token>
```

Settings: `token` (required, stored as a secret) and `server_url` (defaults to `https://api.yokka.ai/mcp`).

The same folder is a Codex plugin with the same skills. Codex connects the server separately: see the
[repo README](../README.md#codex).

## What it sends, and where

The plugin runs no local code. Every call goes to Yokka's MCP server at the `server_url` above: when Claude Code
connects at the start of a session, when it calls a Yokka tool, and when the Stop hook runs. What reaches Yokka is
what the agent writes to your board (card titles, briefs, checklists, progress messages, questions, comments and
attachments) plus the client's name from the MCP handshake. The plugin itself reads no files on your machine. See
the [Privacy Policy](https://yokka.ai/legal/privacy-policy) and
[Data, privacy and security](https://yokka.ai/docs/teams/data/).

## Links

- Docs: <https://yokka.ai/docs/mcp/plugin/>
- Source: <https://github.com/yokka-ai/plugins>
- Support: support@yokka.ai

## License

MIT
