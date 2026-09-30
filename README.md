# Auditable Books for Claude Code

A Claude Code plugin for accountants who use [Auditable Books](https://auditablebooks.com). Once installed, Claude can list your connected QuickBooks Online clients and answer a client's accounting question **from that client's books, with the evidence attached** — the ledger tools that ran and the Intuit trace ids. Read-only. Uses your Auditable Books credit exactly like a question asked in the app.

This repo is a Claude Code **plugin marketplace** (`finochat`) with one plugin (`auditable-books`).

## Install (accountants)

1. In Auditable Books, open **Settings → Claude Code** and press **Create token**. Copy the token; it is shown once.
2. In Claude Code:
   ```
   /plugin marketplace add novalycs/finochat-claude-plugin
   /plugin install auditable-books@finochat
   ```
3. Paste the token when Claude Code asks for **Auditable Books token**.
4. Ask something: *"What did Riverbend Bakery bill in August, with the evidence?"*

To rotate a token: create a new one in Auditable Books, run `/config` in Claude Code, and paste it under **Auditable Books**; then revoke the old one.

## What the plugin contains

| Path | What it is |
|---|---|
| `plugins/auditable-books/.mcp.json` | One remote MCP server: `POST https://api.auditablebooks.com/plugin/mcp`, Bearer = your token |
| `plugins/auditable-books/skills/answer-client-question/` | How Claude routes a client question and presents answer + evidence |
| `plugins/auditable-books/skills/connect/` | Setup and "unauthorized" troubleshooting |
| `plugins/auditable-books/evals/` | `claude plugin eval` cases |

### Tools exposed by the server

| Tool | Read-only | Returns |
|---|---|---|
| `list_clients` | yes | your connected QBO companies (`clientId`, `name`, `status`, `accessMode`) — never a session id |
| `answer_client_question` | yes | `answer` (text, table, chart or rich), `evidence.toolCalls`, `evidence.intuitTraceIds`, `client`, `requestId` |
| `get_credit_status` | yes | `usedFraction`, `exhausted`, `periodResetsAt` — never a token count |

No tool creates a task, posts to QuickBooks, or sends anything to a client. Those stay in Auditable Books where a human approves them.

## Develop

```
claude plugin validate ./plugins/auditable-books
claude --plugin-dir ./plugins/auditable-books
claude plugin eval ./plugins/auditable-books
```

Point at QA by setting **API host** to `https://api-qa.auditablebooks.com` in `/config`.

The server side lives in `finchat-agent` (`src/mcp/plugin-mcp.ts`, `src/mcp/plugin-auth.ts`, `src/routes/plugin-tokens.ts`). Token UI lives in `finchat-agent-ui` (`PluginTokensSection`).

## Versioning

`plugins/auditable-books/.claude-plugin/plugin.json` `version` is the plugin's version; bump it and the matching entry in `.claude-plugin/marketplace.json` together. See `CHANGELOG.md`.
