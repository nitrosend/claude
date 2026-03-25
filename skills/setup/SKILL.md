---
name: setup
description: >
  Guided NitroSend onboarding. Walks through API key configuration, brand
  identity setup, sending domain verification, and sender defaults. Also
  offers to configure proactive analytics (daily/weekly/monthly reports).
  Use when: "set up nitrosend", "configure email", "onboard", "get started
  with nitrosend", or when nitro_get_status shows incomplete onboarding.
---

# NitroSend Setup

Guide the user through NitroSend onboarding. Check what's already done and skip completed steps.

## Step 1: Check Current State

Run `nitro_get_status` to see what's already configured. Report the current state briefly.

## Step 2: API Key

If the MCP server is connected, the API key is already working. Confirm this.

If not connected, tell the user:
1. Sign up at https://nitrosend.com
2. Copy their API key from the dashboard
3. Set the environment variable: `export NITROSEND_API_KEY=nskey_live_...`
4. Restart Claude Code

## Step 3: Brand Identity

If brand is not set, ask the user for their website URL and run `nitro_set_brand` with `url` to auto-scrape brand colors, fonts, and company info. Let them override any scraped values.

If they don't have a website, ask for:
- Company name
- Brand color (hex)
- Logo URL (optional)

## Step 4: Sending Domain

If no domain is verified, guide them through:
1. `nitro_manage_domains` with `operation: "add"` and their domain (e.g. `send.example.com`)
2. Show the DNS records they need to add at their registrar
3. Wait for them to add records, then `nitro_manage_domains` with `operation: "verify"`
4. Mention they can use the sandbox domain (`try.nitrosend.com`, 50 emails/month) while setting up

## Step 5: Sender Defaults

Use `nitro_configure_account` to set:
- `from_name`: Their preferred sender name
- `from_email`: Must match a verified domain
- `reply_to`: Where replies should go
- `test_email_recipients`: Up to 5 emails for test sends

## Step 6: Proactive Analytics (Optional)

Ask the user if they'd like proactive email analytics reports. If yes, help them set up scheduled tasks:

- **Daily** (weekday mornings): Quick summary of new subscribers and any sending activity from yesterday. Use `nitro_get_insights` with `period: "7d"` and `nitro_query` for recent contacts.
- **Weekly** (Monday): Open/click rates, list growth, deliverability summary. Use `nitro_get_insights` with `period: "7d"` for trends.
- **Monthly** (1st of month): Deep dive comparing their metrics against email marketing bible benchmarks. Use `nitro_get_insights` with `period: "30d"`, compare against industry benchmarks, and suggest specific improvements from the email marketing bible.

Use the scheduled tasks system (CronCreate) to set these up. Suggested cron expressions:
- Daily: `"23 8 * * 1-5"` (8:23am weekdays)
- Weekly: `"47 8 * * 1"` (8:47am Mondays)
- Monthly: `"13 9 1 * *"` (9:13am on the 1st)

## Completion

Summarize what was configured and suggest next steps:
- "Try `/nitrosend:compose-email` to create your first email"
- "Use `/nitrosend:import-contacts` to add your audience"
- "Build an automation with `/nitrosend:build-flow`"
