---
name: connect
description: Use when the Auditable Books tools are missing, return 401/unauthorized, say the token is invalid, or the user asks how to set up, connect, log in to, or fix the Auditable Books plugin.
---

# Connect Auditable Books to Claude Code

The plugin talks to one remote server, `auditable-books`, using a personal access token the accountant creates inside Auditable Books. There is no separate login.

## If the tools are not listed or return "unauthorized"

1. Ask the user to open **Auditable Books → Settings → Claude Code** (on the phone app: the Settings tab; on the web: `/account`) and press **Create token**. The token starts with `abk_` and is shown once.
2. Tell them to run `/config` in Claude Code, find **Auditable Books → Auditable Books token**, and paste it. Alternatively, reinstall with:
   ```
   /plugin install auditable-books@finochat
   ```
   and paste the token when prompted.
3. Run `/mcp` and confirm `auditable-books` shows as connected. Then call `list_clients` to verify.

## If `list_clients` returns no clients

The user has not connected a QuickBooks Online company yet. They connect it in Auditable Books (Clients → Connect QuickBooks); the plugin only sees companies connected there.

## If a client says "reconnect_required"

The QuickBooks connection expired. Reconnect that client in Auditable Books; nothing in Claude Code can fix it.

## Never

- Never ask the user to paste their token into the chat. It goes into `/config` (or the install prompt), where Claude Code stores it securely.
- Never suggest an `MCP_API_KEY`, `x-mcp-api-key`, or the `/mcp/public` endpoint; those are internal.
