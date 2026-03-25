---
name: build-flow
description: >
  Build email/SMS automation flows with triggers, steps, waits, splits, and
  webhooks. Covers welcome series, cart abandonment, post-purchase, win-back,
  browse abandonment, and custom event-driven flows. Use when: "automation
  flow", "welcome series", "drip campaign", "cart abandonment flow",
  "win-back flow", "build flow", or "email sequence".
---

# Build Flow

Help the user design and create automation flows using NitroSend.

## Understand the Goal

Ask what kind of flow they need. Common patterns:

| Flow Type | Trigger Event | Typical Steps |
|-----------|--------------|---------------|
| **Welcome series** | `contact_add` | Email 1 (immediate) -> Wait 1d -> Email 2 -> Wait 2d -> Email 3 |
| **Cart abandonment** | `cart_abandoned` | Wait 1h -> Email 1 -> Wait 1d -> Email 2 (with discount) |
| **Post-purchase** | `checkout` or custom | Email 1 (thank you) -> Wait 3d -> Email 2 (review request) |
| **Win-back** | Custom event or segment | Email 1 -> Wait 3d -> Split (opened?) -> Yes: Email 2 / No: Email 3 |
| **Browse abandonment** | `browse_abandoned` | Wait 2h -> Email with viewed products |
| **Onboarding** | `contact_add` or custom | Welcome -> Wait 1d -> Getting started -> Wait 2d -> Feature highlight |
| **Re-engagement** | Custom trigger | Email 1 -> Wait 7d -> Split (engaged?) -> Yes: normal cadence / No: final offer |

## Design the Flow

### Available Step Types

| Step | Required Props | Notes |
|------|---------------|-------|
| `email` | `subject` + (`design` or `body`) | Design uses `{sections, theme}` |
| `sms` | `body` | SMS text content |
| `wait` | `duration` (seconds) | 3600=1hr, 86400=1day, 604800=1week |
| `split` | `filters`, `yes` steps, `no` steps | Conditional branching. NO nested splits. |
| `emit_event` | `event_name` | Fire event to trigger other flows |
| `webhook` | `url` | POST or PUT to external URL |
| `subscribe` | `channel` (phone/email/all) | Opt contact in |
| `unsubscribe` | `channel` (phone/email/all) | Opt contact out |

### Split Filters

Available filter names for splits:
- `contact_first_name`, `contact_last_name`, `contact_email`, `contact_phone_number`
- `contact_country`, `contact_source`, `contact_tag`
- `contact_subscribed_email`, `contact_subscribed_phone`
- `contact_created_at`, `contact_last_interacted_at`
- `contact_verified`, `contact_enriched`

Predicates: `eq`, `not_eq`, `cont`, `not_cont`, `present`, `blank`, `true`, `false`, `gt`, `lt`, `in`, `not_in`

### Best Practices (from Email Marketing Bible)
- **Welcome flows**: Send first email within minutes. 74% of subscribers expect it.
- **Cart abandonment**: First email at 1 hour. 50% conversion happens in first email.
- **Wait times**: Space emails 1-3 days apart. Don't overwhelm.
- **Splits**: Use engagement-based splits (opened, clicked) to tailor follow-ups.
- **Unsubscribe step**: Add at end of win-back flows for contacts who don't re-engage.

## Create the Flow

Use `nitro_compose_flow` with:
- `name`: Descriptive flow name
- `trigger`: `{ event: "trigger_event_name" }` — optionally add `segment_id` or `contact_list_id`
- `steps`: Array of step objects

Offer `dry_run: true` first to preview the flow graph.

## Review and Approve

1. Show the dry-run preview (node types and connections)
2. Get user approval on the flow design
3. Create for real (without dry_run)
4. Approve: `nitro_control_delivery` with `target_type: "flow"`, `operation: "approve"`
5. Go live: `nitro_control_delivery` with `operation: "live"`

## Modify Existing Flows

To rebuild an existing flow:
- Use `nitro_compose_flow` with `mode: "replace"`, `flow_id`, and `confirm: true`

To rename only:
- Use `nitro_compose_flow` with `mode: "patch"` and `flow_id`
