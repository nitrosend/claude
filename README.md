# Nitrosend Claude Plugin

Use Nitrosend from Claude through the production remote MCP server. The plugin
adds guided workflows for account setup, templates, campaigns, flows,
transactional messages, contacts, and analytics.

## Install

For local use or source review, clone this repository and load it directly:

```bash
claude --plugin-dir /absolute/path/to/claude
```

This repository is itself a plugin marketplace, so the standard install is:

```text
/plugin marketplace add nitrosend/claude
/plugin install nitrosend@nitrosend
```

If the plugin also appears in Anthropic's official directory catalog, installing
it from there is equivalent.

## Connect

The bundled `.mcp.json` connects to:

```text
https://api.nitrosend.com/mcp
```

Open the MCP or connectors UI in the Claude client, select `nitrosend`, and
complete browser OAuth for the intended Nitrosend login. Then ask Claude to
call `nitro_get_status` and confirm the returned account and brand.

Do not install the Nitrosend CLI or request an API key for normal Claude
plugin, Claude Desktop, Claude.ai, or Cowork setup. The CLI is for terminal,
CI, and headless runner workflows outside this plugin.

See [SETUP.md](./SETUP.md) for account switching and troubleshooting.

## What Is Included

The production MCP catalog is authoritative. Claude must use the tools exposed
by the connected session and must never offer a tool that is absent. Some
early-access capabilities, including inbox and outreach operations, are
account-gated and therefore may not appear.

Current workflow groups include:

- **Context and docs:** `nitro_get_status`, `nitro_select_account`,
  `nitro_select_brand`, `nitro_search_docs`
- **Brand and sending setup:** `nitro_set_brand_kit`,
  `nitro_configure_account`, `nitro_manage_domains`,
  `nitro_configure_providers`
- **Audience:** `nitro_import_contacts`, `nitro_manage_audience`,
  `nitro_define_segment`, `nitro_search_contacts`, `nitro_query`
- **Authoring:** `nitro_manage_template`, `nitro_compose_campaign`,
  `nitro_compose_flow`, `nitro_ingest`
- **Review and delivery:** `nitro_review_delivery`,
  `nitro_send_test_message`, `nitro_control_delivery`, `nitro_send_message`
- **Operations:** `nitro_get_insights`, `nitro_manage_billing`,
  `nitro_request_support`, `nitro_set_memory`

Supported BYO email providers are Mailgun, Amazon SES, Postmark, Resend, and
SendGrid. The connected tool schema remains the authority if that set changes.

### Skills

| Skill | Command | Purpose |
|---|---|---|
| Setup | `/nitrosend:setup` | Connect and complete account readiness |
| Compose Email | `/nitrosend:compose-email` | Create or edit a reusable template |
| Send Campaign | `/nitrosend:send-campaign` | Draft, review, test, approve, and deliver a campaign |
| Send Transactional | `/nitrosend:send-transactional` | Send or integrate one-recipient application messages |
| Build Flow | `/nitrosend:build-flow` | Build and publish event-driven automation |
| Import Contacts | `/nitrosend:import-contacts` | Import consented contacts |
| Analytics | `/nitrosend:analytics` | Inspect account, campaign, flow, or message performance |
| Email Marketing Bible | `/nitrosend:email-marketing-bible` | Apply durable email-marketing principles |

### Email Marketer Agent

The bundled `email-marketer` agent routes work through the tools actually
available in the live MCP session. In Claude Code, manage it from `/agents` or
invoke it by its scoped agent name when the client exposes plugin agents.

In MCP-Apps-capable hosts (Claude.ai and Claude Desktop),
`nitro_compose_campaign` and `nitro_manage_template` results also render an
inline preview of the composed email alongside the text response.

## Desktop Extension (Claude Desktop)

The [`mcpb/`](./mcpb/) directory packages Nitrosend as a Claude Desktop
extension (`.mcpb`): a one-click install that proxies to the same production
MCP server with browser OAuth by default and an optional account-pinning API
key setting. Build it with `cd mcpb && npm ci --omit=dev && npx
@anthropic-ai/mcpb pack . dist/nitrosend.mcpb`; see
[mcpb/README.md](./mcpb/README.md) for install, troubleshooting, and the
extension privacy policy.

## Requirements

- A current Claude client with plugin and remote MCP support
- A Nitrosend account accessible through browser OAuth
- Network access to `https://api.nitrosend.com/mcp`

## Validate

Validate a local checkout with the current Claude Code plugin validator:

```bash
claude plugin validate --strict /absolute/path/to/claude
```

Then load it with `--plugin-dir`, open `/mcp`, connect `nitrosend`, call
`nitro_get_status`, and confirm the active account and brand before performing
any mutation.

## Links

- [Nitrosend](https://nitrosend.com)
- [Nitrosend API](https://docs.nitrosend.com/api)
- [Nitrosend SDK](https://www.npmjs.com/package/@nitrosend/sdk)
- [Setup guide](./SETUP.md)

## License

MIT
