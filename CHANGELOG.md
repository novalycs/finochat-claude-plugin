# Changelog

All notable changes to **Auditable Books for Claude Code** (the `finochat` plugin marketplace) are documented in this file.

## [0.1.0] — 2026-09-30 17:31 UTC

### Added — first plugin: `auditable-books`

**What changed**: a Claude Code plugin marketplace with one plugin. `.mcp.json` declares the remote MCP server at `${api_url}/plugin/mcp` (agent 2.80.0) authenticated by a `userConfig` token pasted once at install; two skills (`answer-client-question`, `connect`); one eval case.

**Why**: accountants already use Claude Code. VISION §7.11 approved a connector (P8) and sequenced the Connectors Directory listing after item 17; a plugin marketplace reaches Claude Code users today with no OAuth server and no directory review, on the same MCP server the directory listing will use later.
