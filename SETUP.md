# Nitrosend Setup

This plugin uses the production remote MCP server at
`https://api.nitrosend.com/mcp`. Browser OAuth is the default connection path
for Claude Code, Claude Desktop, Claude.ai, and Cowork.

Do not direct those users to a terminal CLI or an API key. Those are separate
terminal, CI, and headless-runner surfaces.

## Connect

1. Enable or locally load the plugin.
2. Open the client's MCP or connectors UI and select `nitrosend`.
3. Complete browser sign-in and approve access for the intended Nitrosend
   login.
4. Call `nitro_get_status`.
5. Confirm the returned account and brand before changing state.

## Switch Account or Brand

An OAuth session can switch among accounts accessible to the signed-in user:

1. Read `available_accounts.items` from `nitro_get_status`.
2. Call `nitro_select_account` with the chosen `account_id`.
3. Call `nitro_get_status` again; the switch applies on that next call.
4. If needed, choose a non-default brand with `nitro_select_brand`.

An API-key MCP connection is pinned to its account and cannot switch accounts.

Reconnect only when the required account belongs to a different Nitrosend
login. Disconnect `nitrosend` in the client's MCP/connectors UI, sign out of
the browser session used for OAuth, reconnect, and verify with
`nitro_get_status`.

## Troubleshoot

Before escalating, establish the exact failure point:

1. Confirm `https://api.nitrosend.com/mcp` is reachable.
2. Confirm the browser OAuth flow completed and returned to Claude.
3. Confirm the client supports current remote MCP connections.
4. Reopen the MCP/connectors UI and inspect the `nitrosend` connection state.
5. If connected, call `nitro_get_status` and report its structured error rather
   than guessing about credentials or account state.

Use `nitro_search_docs` for Nitrosend product behavior not established by the
live tool descriptions or the current status response.
