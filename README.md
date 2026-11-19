# Nitrosend: AI email marketing for Claude

Nitrosend is a full AI email suite for Claude: email marketing, newsletters,
transactional email, a shared inbox and marketing automation, run from one
conversation. Build an email brand kit from your website, then write, design
and send newsletters and email campaigns, build automated email flows (welcome
series, abandoned cart, win-back, post-purchase, drip and nurture), send
receipts and password resets from your app, and read and reply to email.

This plugin connects Claude Code, Cowork and Claude to the Nitrosend remote MCP
server with OAuth, and adds guided skills for each job:

- **Newsletters and email campaigns:** on-brand email templates, subject
  lines, test emails, then approval, scheduling or sending to a list or segment.
- **Marketing automation:** welcome email series, abandoned cart and browse
  abandonment reminders, win-back, post-purchase, drip and nurture flows.
- **Transactional email:** receipts, alerts, magic links and password resets
  by REST API, SDK or Events API, on Nitrosend or your own SendGrid, Amazon
  SES, Postmark, Resend or Mailgun account.
- **Signup forms and contacts:** signup forms for any website, contacts,
  email lists, segments, tags and email verification.
- **Migrations and integrations:** Mailchimp and ActiveCampaign connectors,
  CSV import from Klaviyo, MailerLite, Brevo, Kit (ConvertKit), beehiiv or
  Substack, and native Shopify, Stripe, HubSpot and Attio integrations.
- **Shared inbox, deliverability and analytics:** replies with reply-all, Cc
  and Bcc, SPF, DKIM and DMARC checks, and email analytics with what to fix
  first.

Nothing is sent to your audience until you approve it.

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

### Skills only (any agent)

To add just the skills to Claude Code, Codex, Cursor, Gemini CLI and other
agents that read Agent Skills, use the open `skills` CLI:

```bash
npx skills add nitrosend/claude
```

The skills call the Nitrosend MCP server, so connect
`https://api.nitrosend.com/mcp` in that agent as well. See the
[integration guides](https://docs.nitrosend.com/integrations/overview).

### Google Antigravity

The same repository installs as an Antigravity plugin, with the skills, the
`email-marketer` agent and the remote MCP server:

```bash
agy plugin install https://github.com/nitrosend/claude
```

Then authenticate `nitrosend` from Antigravity's Customizations settings. See
the [Antigravity guide](https://docs.nitrosend.com/integrations/antigravity).

### Email Marketing Bible

The email-marketing reference skill lives in its own repository so installs
are not split across copies:

```bash
npx skills add CosmoBlk/email-marketing-bible
```

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
| Setup | `/nitrosend:nitrosend-setup` | Connect and complete account readiness |
| Compose Email | `/nitrosend:compose-email` | Create or edit a reusable template |
| Send Campaign | `/nitrosend:send-campaign` | Draft, review, test, approve, and deliver a campaign |
| Send Transactional | `/nitrosend:send-transactional` | Send or integrate one-recipient application messages |
| Build Flow | `/nitrosend:build-flow` | Build and publish event-driven automation |
| Import Contacts | `/nitrosend:import-contacts` | Import consented contacts |
| Analytics | `/nitrosend:nitrosend-analytics` | Inspect account, campaign, flow, or message performance |
| Email Brand Kit | `/nitrosend:email-brand-kit` | Build the Brand Kit from a website |
| Welcome Email Series | `/nitrosend:welcome-email-series` | Welcome emails for new subscribers or customers |
| Abandoned Cart Emails | `/nitrosend:abandoned-cart-emails` | Cart, checkout and browse abandonment reminders |
| Signup Form Builder | `/nitrosend:signup-form-builder` | Signup form snippet for any website |
| Mailchimp Migration | `/nitrosend:mailchimp-migration` | Move audiences from Mailchimp with consent kept |
| Klaviyo Migration | `/nitrosend:klaviyo-migration` | Import a Klaviyo export and rebuild its flows |
| Email Platform Migration | `/nitrosend:email-platform-migration` | ActiveCampaign connector or CSV from other platforms |
| Contacts and Segments | `/nitrosend:contacts-and-segments` | Lists, tags, segments and email verification |
| Deliverability Check | `/nitrosend:email-deliverability-check` | Sending domain, SPF, DKIM, DMARC and bounces |
| Shared Email Inbox | `/nitrosend:shared-email-inbox` | Read and reply to inbox conversations |
| Shopify Email Marketing | `/nitrosend:shopify-email-marketing` | Shopify customers and store activity in email |
| Email Integrations | `/nitrosend:email-integrations` | Native integrations and the Events API |
| Agency Client Email | `/nitrosend:agency-client-email` | Work across client accounts and brands |

### Email Marketer Agent

The bundled `email-marketer` agent routes work through the tools actually
available in the live MCP session. In Claude Code, manage it from `/agents` or
invoke it by its scoped agent name when the client exposes plugin agents.

In MCP-Apps-capable hosts (Claude.ai and Claude Desktop),
`nitro_compose_campaign` and `nitro_manage_template` results also render an
inline preview of the composed email alongside the text response.

## Desktop Extension (Claude Desktop)

The Claude Desktop extension (`.mcpb`) now lives in
[nitrosend/mcp](https://github.com/nitrosend/mcp/tree/main/mcpb). Download the
current build,
[nitrosend.mcpb 1.0.1](https://github.com/nitrosend/claude/releases/download/v2.1.5/nitrosend.mcpb),
from the v2.1.5 release.

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

Antigravity reads `.antigravity-plugin/plugin.json` and the root
`mcp_config.json`. Check them with:

```bash
agy plugin validate /absolute/path/to/claude
```

The only expected warning is that Antigravity ignores Claude's `.mcp.json`.

## Privacy and Support

- Privacy policy: [nitrosend.com/privacy](https://nitrosend.com/privacy)
- Support: [contact@nitrosend.com](mailto:contact@nitrosend.com) or
  [app.nitrosend.com/support](https://app.nitrosend.com/support)
- Plugin issues: [github.com/nitrosend/claude/issues](https://github.com/nitrosend/claude/issues)

## Links

- [Nitrosend](https://nitrosend.com)
- [Nitrosend API](https://docs.nitrosend.com/api)
- [Nitrosend SDK](https://www.npmjs.com/package/@nitrosend/sdk)
- [Setup guide](./SETUP.md)

## License

MIT
