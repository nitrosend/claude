# Nitrosend Claude Plugin

AI-native email marketing for Claude Code. Compose emails, build automation flows, manage contacts, and send campaigns — all from your terminal.

## Quick Start

### 1. Set your API key

```bash
export NITROSEND_API_KEY=nskey_live_your_key_here
```

Get your API key at [nitrosend.com](https://nitrosend.com).

### 2. Install the plugin

**From a marketplace** (when available):
```bash
claude plugin install nitrosend@marketplace-name
```

**Local development**:
```bash
claude --plugin-dir /path/to/claude-plugin
```

### 3. Run setup

```
/nitrosend:setup
```

This walks you through brand configuration, domain verification, and sender defaults.

## What's Included

### MCP Tools (16+)

The plugin bridges `@nitrosend/mcp` to give Claude access to the full Nitrosend platform:

- **Email composition**: Create templates with section-based design, preview, and test sends
- **Campaign management**: Create, target, approve, send, and schedule campaigns
- **Automation flows**: Build trigger-based email/SMS sequences with waits, splits, and webhooks
- **Transactional email**: Send receipts, OTPs, confirmations — immediate, single-recipient
- **Contact management**: Import, segment, tag, list management
- **Analytics**: Account-wide and per-entity insights with trends and benchmarks
- **Account management**: Brand setup, domain verification, sender config, billing

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

An expert email marketing strategist that combines deep domain knowledge with Nitrosend's tools. Activated by default when the plugin is enabled.

- Proactive best-practice suggestions backed by industry benchmarks
- Knows all 16 MCP tools and when to use each
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

### SessionStart Hook

Automatically checks your account status when Claude starts. Surfaces incomplete onboarding steps silently — never blocks your workflow.

## Requirements

- Claude Code 1.0.33+
- Node.js 18+ (for `npx @nitrosend/mcp`)
- Nitrosend API key (`NITROSEND_API_KEY` environment variable)

## Links

- [Nitrosend](https://nitrosend.com)
- [Email Marketing Bible](https://emailmarketingskill.com)
- [MCP Package](https://www.npmjs.com/package/@nitrosend/mcp)
- [SDK Package](https://www.npmjs.com/package/@nitrosend/sdk)
- [API Docs](https://api.nitrosend.com)

## License

MIT
