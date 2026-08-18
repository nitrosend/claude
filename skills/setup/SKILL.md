---
name: setup
description: >
  Guided Nitrosend onboarding. Walks through remote MCP connection, Brand Kit
  identity setup, managed-sender preparation or customer-domain verification,
  and sender defaults. Also
  offers to configure proactive analytics (daily/weekly/monthly reports).
  Use when: "set up nitrosend", "configure email", "onboard", "get started
  with nitrosend", or when nitro_get_status shows incomplete onboarding.
---

# Nitrosend Setup

Guide the user through Nitrosend onboarding. Check what's already done and skip completed steps.

## Connection Surface

If the user is in Claude Desktop, Claude.ai, Claude Cowork, or this Nitrosend
Claude plugin, connect Nitrosend through the remote MCP connector at
`https://api.nitrosend.com/mcp`. Do not recommend `@nitrosend/cli`,
`nitrosend login`, `@nitrosend/mcp`, or API-key setup for standard Claude
Desktop onboarding.

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

## Step 3: Brand Kit

If Brand Kit is not set, ask the user for their website URL and run `nitro_set_brand_kit` with `url` to auto-scrape brand colors, fonts, and company info. Let them override any scraped values.

If they don't have a website, ask for:
- Company name
- Brand color (hex)
- Logo URL (optional)

## Step 4: Sending Domain

Read `brand_subdomain` from `nitro_get_status`. If there is no ready selected
sender, offer these two explicit paths:

1. Recommended quick start: call `nitro_manage_domains` with
   `operation: "prepare_brand_subdomain"`. Brand creation reserved the shown
   `<brand>.nitrosend.com` address, but this call is what creates its DNS,
   DKIM, provider, and tenant resources. It sends and retains no email.
2. Poll `nitro_get_status` until `brand_subdomain.ready` is true. Then call
   `nitro_manage_domains` with `operation: "select_brand_subdomain"` and the
   user's chosen `local_part` (default `hello`). Never infer selection from
   readiness.
3. Customer-domain alternative: use `nitro_manage_domains` with
   `operation: "add"`, show the returned DNS records, wait for the user to add
   them, then use `operation: "verify"`.

Do not claim that account creation or a draft consumes sending infrastructure.
Do not describe a shared sender or an allowance for unverified sending.

## Step 5: Sender Defaults

Use `nitro_configure_account` to set:
- `from_name`: Their preferred sender name
- `from_email`: Must match the explicitly selected Nitrosend brand-subdomain sender or a verified customer domain
- `reply_to`: Where replies should go
- `test_email_recipients`: Up to 5 emails for test sends

## Step 6: Proactive Analytics (Optional)

Ask the user if they'd like proactive email analytics reports. If yes, help them set up scheduled tasks:

- **Daily** (weekday mornings): Quick summary of new subscribers and any sending activity from yesterday. Use `nitro_get_insights` with `period: "7d"` and `nitro_query` for recent contacts.
- **Weekly** (Monday): Open/click rates, list growth, deliverability summary. Use `nitro_get_insights` with `period: "7d"` for trends.
- **Monthly** (1st of month): Deep dive comparing their metrics against email marketing bible benchmarks. Use `nitro_get_insights` with `period: "30d"`, compare against industry benchmarks, and suggest specific improvements from the email marketing bible.

Use the scheduled tasks system (`create_scheduled_task`) to set these up. Suggested cron expressions:
- Daily: `"23 8 * * 1-5"` (8:23am weekdays)
- Weekly: `"47 8 * * 1"` (8:47am Mondays)
- Monthly: `"13 9 1 * *"` (9:13am on the 1st)

## Completion

Summarize what was configured and suggest next steps:
- "Try `/nitrosend:compose-email` to create your first email"
- "Use `/nitrosend:import-contacts` to add your audience"
- "Build an automation with `/nitrosend:build-flow`"
