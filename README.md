# Nitrosend Claude Plugin

AI-native email marketing for Claude. Connect Nitrosend over remote MCP,
authenticate with OAuth, and compose emails, build automation flows, manage
contacts, and send campaigns from Claude Code or Cowork.

## Quick Start

### 1. Install the plugin

**From a marketplace** on a current Claude Code build:
```bash
/plugin install nitrosend@marketplace-name
```

**Local development**:
```bash
claude --plugin-dir /path/to/claude
```

If you do not see `/plugin`, update Claude Code to the latest build first.

### 2. Connect Nitrosend

The plugin bundles Nitrosend as a remote MCP server at
`https://api.nitrosend.com/mcp`. Open the MCP/connectors UI for your Claude
client, select the bundled `nitrosend` server, and complete the browser OAuth
flow for the Nitrosend account you want active.

Do not install `@nitrosend/cli` for Claude Desktop, Claude.ai, Cowork, or this
plugin. The CLI is for terminal and CI workflows; Claude uses the remote MCP
connector directly.

See [SETUP.md](./SETUP.md) for connect, reconnect, disconnect, and
account-verification guidance.

### 3. Verify the connection

Ask Claude to run:

```text
nitro_get_status
```

This confirms the active Nitrosend account and current onboarding state.

## What's Included

### Nitrosend MCP Tools (28)

The plugin bundles the full Nitrosend remote MCP surface — 28 registered tools.
Three early-access tools are capability-gated and appear only on accounts with
the feature enabled.

- **Status & docs**: `nitro_get_status` (account health, onboarding state),
  `nitro_search_docs` (authoritative product documentation search)
- **Account & brand**: `nitro_select_account` (switch accounts on OAuth
  connections), `nitro_select_brand`, `nitro_set_brand_kit` (brand identity from
  URL or manual), `nitro_configure_account` (sender defaults, reply-to, test
  recipients)
- **Template management**: `nitro_manage_template` — create, update, clone
  reusable email templates with section-based design
- **Campaign management**: `nitro_compose_campaign` — email or SMS campaign
  drafts with audience targeting
- **Automation flows**: `nitro_compose_flow` — trigger-based email/SMS sequences
  with waits, splits, and webhooks
- **Review & delivery**: `nitro_review_delivery` (validation and readiness),
  `nitro_send_test_message` (real test sends), `nitro_control_delivery`
  (approve, go live, pause, schedule)
- **Transactional email**: `nitro_send_message` — receipts, OTPs, confirmations;
  immediate, single-recipient
- **Contact management**: `nitro_import_contacts` (bulk import),
  `nitro_manage_audience` (contacts, lists, events, tags),
  `nitro_define_segment` (filtered segments with preview),
  `nitro_search_contacts`, `nitro_query` (campaigns, flows, templates, lists,
  and other entities)
- **Assets**: `nitro_ingest` — host images on Nitrosend storage for use in email
  designs
- **Analytics**: `nitro_get_insights` — account-wide and per-entity insights
  with trends and benchmarks
- **Agent inbox** (early access): `nitro_inbox` (read the agent queue and
  mailbox), `nitro_inbox_action` (reply, mark handled, escalate to a human)
- **Outreach discovery** (early access): `nitro_manage_outreach` — plan and
  operate person-first outreach discovery with bounded spend
- **Domains & providers**: `nitro_manage_domains` (add, verify, list sending
  domains), `nitro_configure_providers` (BYO Mailgun/SES)
- **Billing, support & memory**: `nitro_manage_billing`, `nitro_request_support`,
  `nitro_set_memory` (persistent AI context)

### Skills

| Skill | Command | Description |
|-------|---------|-------------|
| Setup | `/nitrosend:setup` | Guided onboarding with optional proactive analytics |
| Compose Email | `/nitrosend:compose-email` | Template creation wizard |
| Send Campaign | `/nitrosend:send-campaign` | End-to-end campaign workflow |
| Send Transactional | `/nitrosend:send-transactional` | App-triggered email/SMS for developers |
| Build Flow | `/nitrosend:build-flow` | Automation flow builder |
| Import Contacts | `/nitrosend:import-contacts` | Contact import with compliance guidance |
| Analytics | `/nitrosend:analytics` | Performance insights and benchmarking |
| Email Marketing Bible | `/nitrosend:email-marketing-bible` | 65K-word knowledge base (908 sources) |

### Email Marketer Agent

An expert email marketing strategist that combines deep domain knowledge with Nitrosend's tools.

- Proactive best-practice suggestions backed by industry benchmarks
- Knows all 28 Nitrosend MCP tools and when to use each
- Covers strategy, copywriting, deliverability, compliance, and 19 industry playbooks
- Persistent memory across sessions

Run as the main agent:
```bash
claude --agent nitrosend:email-marketer
```

### Proactive Analytics

During setup, opt in to scheduled reports:
- **Daily** (weekday mornings): New subscribers + sending activity
- **Weekly** (Mondays): Open/click rates, list growth, deliverability
- **Monthly** (1st): Deep dive comparing your metrics against industry benchmarks

## Requirements

- Current Claude Code build with plugin support and remote MCP support
- Claude Cowork, if you are using the shared plugin directory there
- A Nitrosend account you can complete OAuth with in the browser
- Public reachability to `https://api.nitrosend.com/mcp`

## Validation

### Current-build plugin smoke test

1. Update Claude Code until `/plugin` is available.
2. Validate the plugin bundle locally:

```bash
claude plugin validate /path/to/claude
```

3. Install the plugin and restart Claude if prompted.
4. Open `/mcp` and confirm the bundled `nitrosend` server is present.
5. Start the Nitrosend connection flow and complete browser OAuth.
6. Ask Claude to run `nitro_get_status`.
7. Confirm the returned account and brand details match the account you meant to connect.

### Older-build transport fallback

If you are stuck on an older Claude Code build that has `claude mcp` but not
`/plugin`, you can still sanity-check the remote transport:

```bash
claude mcp add-json nitrosend-dev '{"type":"http","url":"https://api.nitrosend.com/mcp"}' --scope local
claude mcp get nitrosend-dev
claude mcp remove nitrosend-dev --scope local
```

On the local `1.0.119` environment used during this cutover, `add-json`
reported success but `get` and `list` did not reliably surface the local-scope
entry back. Treat that path as a transport sanity check only, not a substitute
for the current-build plugin smoke test above.

### Remote review smoke test

1. Install or enable the plugin in the Anthropic surface you plan to submit or review from.
2. Connect Nitrosend through the client's MCP/connectors UI.
3. Run `nitro_get_status` immediately after the browser flow returns.
4. If OAuth connects instantly and account selection is ambiguous, disconnect Nitrosend, log out of Nitrosend in the browser session Claude opens, reconnect, then run `nitro_get_status` again.

## Links

- [Nitrosend](https://nitrosend.com)
- [Email Marketing Bible](https://emailmarketingskill.com)
- [SDK Package](https://www.npmjs.com/package/@nitrosend/sdk)
- [API Docs](https://api.nitrosend.com)
- [Setup Guide](./SETUP.md)

## License

MIT
