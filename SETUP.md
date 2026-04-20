# Nitrosend Setup

Use this guide whenever the Nitrosend plugin is installed but not yet connected,
or when the user needs to verify which Nitrosend account Claude is currently
using.

## What This Plugin Does

The plugin bundles Nitrosend as a remote MCP server at
`https://api.nitrosend.com/mcp`. Connection is OAuth-based. Do not steer the
user toward the deprecated local bridge or an environment-variable API-key flow
as the default setup path for this plugin.

## First-Time Connect

1. Confirm the plugin is enabled.
2. If Claude reports that Nitrosend is not connected, direct the user to open
   the MCP/connectors UI for their Claude client and select the bundled
   `nitrosend` server.
3. Tell the user to complete the browser sign-in flow and approve the requested
   access for the intended Nitrosend account.
4. After the browser flow returns, verify the connection by calling
   `nitro_get_status`.

## Reconnect Or Switch Accounts

If the browser signs in instantly and the user cannot tell which account is
active:

1. Disconnect Nitrosend from Claude first.
   In Claude Code, inspect the plugin-provided `nitrosend` server from `/mcp`.
   In Claude web or Cowork, use Customize -> Connectors.
2. Log out of Nitrosend in the browser session that Claude opens for OAuth.
3. Start the connection flow again.
4. Immediately verify the active account by calling `nitro_get_status` and
   reporting the returned account/workspace details back to the user.

## Verification

After connect or reconnect:

1. Run `nitro_get_status`.
2. Confirm the account is healthy enough to proceed.
3. If the user is checking account selection, summarize the identifying account
   fields from the tool result instead of saying only "connected".

## If Connection Fails

Check the following before escalating:

1. `https://api.nitrosend.com/mcp` is reachable.
2. The user completed the OAuth browser flow without cancelling.
3. The user is connecting to a public Nitrosend environment, not a local or
   private endpoint.
4. Claude is using a build that supports remote MCP connections.

If the server is reachable but auth still fails, capture the exact failure point
and whether the flow returned to Claude after the browser step.

## Older Claude Code Builds

If the local Claude Code build has `claude mcp` but does not expose `/plugin`,
update Claude Code before treating the plugin lane as fully validated.

As a fallback sanity check for the remote transport, this command path is
available on older builds:

```bash
claude mcp add-json nitrosend-dev '{"type":"http","url":"https://api.nitrosend.com/mcp"}' --scope local
claude mcp get nitrosend-dev
claude mcp remove nitrosend-dev --scope local
```

On the local `1.0.119` environment used during this cutover, `add-json`
reported success but `get` and `list` did not reliably surface the local-scope
entry back. Use a current Claude Code build for the real plugin-manager smoke
test.
