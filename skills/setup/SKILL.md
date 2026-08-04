---
name: setup
description: >
  Guided Nitrosend onboarding. Walks through remote MCP connection, Brand Kit
  identity setup, sending domain verification, and sender defaults. Also
  offers to configure proactive analytics (daily/weekly/monthly reports).
  Use when: "set up nitrosend", "configure email", "onboard", "get started
  with nitrosend", or when nitro_get_status shows incomplete onboarding.
---

# Nitrosend Setup

Guide the user through Nitrosend onboarding. Check what's already done and skip completed steps.

## Connection Surface

If the user is in Claude Desktop, Claude.ai, Claude Cowork, or this Nitrosend
Claude plugin, connect Nitrosend through the remote MCP connector at
`https://api.nitrosend.com/mcp`. Do not recommend a terminal CLI or API-key
setup for standard Claude Desktop onboarding.

Use the CLI only when the user explicitly wants a terminal, CI, or headless
agent-runner workflow outside Claude Desktop. If a command sandbox cannot reach
`api.nitrosend.com`, treat that as sandbox egress, not as a reason to switch
Claude Desktop users to the CLI.

## Step 1: Check Current State

Run `nitro_get_status` to see what's already configured. Report the current state briefly.

## Step 2: Connection

If the MCP server is connected, OAuth is already working. Confirm this.

If not connected, tell the user:
1. Open the MCP/connectors UI in their Claude client
2. Select the bundled `nitrosend` server
3. Complete the browser sign-in and approval flow
4. Verify the connection with `nitro_get_status`

### Multiple Accounts

If the user needs a different Nitrosend account on an OAuth connection:

1. Run `nitro_get_status` and read the ids from `available_accounts.items[*].id`
2. Run `nitro_select_account` with the target `account_id` — the switch takes
   effect on the next tool call and lands on that account's default brand
3. Re-run `nitro_get_status` to confirm, then `nitro_select_brand` if a
   non-default brand is needed

API-key connections are pinned to one account and cannot switch.

## Step 3: Brand Kit

If Brand Kit is not set, ask the user for their website URL and run
`nitro_set_brand_kit` with `url`, the default synchronous mode, and a stable
`idempotency_key`. Let them override any scraped values. Reuse that key only
for an exact retry.

If they don't have a website, ask for:
- Company name
- Brand color (hex)
- Logo URL (optional)

Send direct identity values in `fields`. Pass a public `logo_url` separately;
remote logo fetches also require a stable `idempotency_key`. For a local image,
use `nitro_ingest` first and pass its returned `media_url` or `image_url`.

## Step 4: Sending Domain

If no domain is verified, explain that the shared `nitr-o.com` sandbox supports
test sends and small campaigns while a custom domain is being prepared. If the
user wants a branded sender, guide them through:

1. `nitro_manage_domains` with `operation: "add"` and
   `params: {domain_name: "send.example.com"}`
2. Show the DNS records they need to add at their registrar
3. After they add the records, call `nitro_manage_domains` with
   `operation: "verify"` and the same `params.domain_name`

## Step 5: Sender Defaults

Use `nitro_configure_account` to set:
- `from_name`: Their preferred sender name
- `from_email`: Use a sender address allowed by the current sandbox or verified
  custom-domain state returned by `nitro_get_status`
- `reply_to`: Where replies should go
- `test_email_recipients`: Up to 5 emails for test sends

## Step 6: Proactive Analytics (Optional)

Offer proactive analytics only if the current host exposes a scheduled-task
capability. If it does not, say that recurring reports are unavailable in this
client and continue without inventing a task tool. When scheduling is available:

- **Daily** (weekday mornings): Quick summary of new subscribers and any sending activity from yesterday. Use `nitro_get_insights` with `period: "7d"` and `nitro_query` for recent contacts.
- **Weekly** (Monday): Open/click rates, list growth, deliverability summary. Use `nitro_get_insights` with `period: "7d"` for trends.
- **Monthly** (1st of month): Deep dive using `nitro_get_insights` with
  `period: "30d"`, comparing with the account's prior periods and any
  benchmarks returned by the live tool.

Use only the scheduling operation actually exposed by the host. Suggested cron expressions:
- Daily: `"23 8 * * 1-5"` (8:23am weekdays)
- Weekly: `"47 8 * * 1"` (8:47am Mondays)
- Monthly: `"13 9 1 * *"` (9:13am on the 1st)

## Product Questions

For any Nitrosend product question not answered by the current context (how a
feature works, setup or verification steps, API/SDK/CLI usage, integrations),
call `nitro_search_docs` and treat the returned excerpts as the source of truth.
If they do not establish an answer, search again or say what is not verified —
never invent product behavior.

## Completion

Summarize what was configured and suggest next steps:
- "Try `/nitrosend:compose-email` to create your first email"
- "Use `/nitrosend:import-contacts` to add your audience"
- "Build an automation with `/nitrosend:build-flow`"
