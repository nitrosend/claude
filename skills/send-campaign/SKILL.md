---
name: send-campaign
description: >
  End-to-end campaign workflow: create, design, target audience, preview,
  test, approve, and send or schedule an email or SMS campaign. Use when:
  "send campaign", "email blast", "newsletter", "send to my list",
  "broadcast email", or "schedule campaign".
---

# Send Campaign

Guide the user through the full campaign lifecycle.

## Step 1: Create the Campaign

Ask:
- Campaign name
- Channel: email (default) or SMS
- Subject line (email only)

For email, choose design approach:
- **Inline sections**: Build design from scratch (use compose-email patterns)
- **Clone template**: Use `template_id` from an existing template
- **Plain text**: Just `body` + `subject`

Use `nitro_compose_campaign` with the chosen approach.

**Composition contract**: newly authored email copy or design always enters the
contract before persistence. Call `nitro_compose_campaign` with
`composition_mode: "intent"` — the response includes the current brand, memory,
source, binding, and design context plus a `next_call` scaffold. Fill
`next_call` and send it back, preserving its contract and idempotency fields
(optionally `composition_mode: "validate"` first), then persist with
`composition_mode: "draft"` — or use the returned metered
`optional_server_authoring_call` (`composition_mode: "generate"`) for
server-side authoring.
Supplying creative fields without a contract returns the intent contract instead
of creating the campaign. Cloning a template without creative overrides, plain
operational edits, and `dry_run` previews skip recomposition. Preserve explicit
user constraints verbatim: exact copy, URLs, offers, sender fields, audience,
schedule, and legal text.

## Step 2: Target Audience

Ask who should receive this:
- **Contact lists**: Use `audience: { audience_type: "lists", contact_list_ids: [LIST_ID] }` — query lists with `nitro_query` entity "lists"
- **Segment**: Use `audience: { audience_type: "segment", segment_id: SEGMENT_ID }` — query segments with `nitro_query` entity "segments"
- **All subscribers**: Use `audience: { audience_type: "all_contacts" }` only when the user explicitly wants an all-subscribed-contacts send

If they need a new segment, use `nitro_define_segment` to create one with filters.

## Step 3: Preview and Test

1. Review: `nitro_review_delivery` with `target_type: "campaign"` and `target_id` for validation, readiness, and delivery context
2. Test send: `nitro_send_test_message` with `target_type: "campaign"`, `target_id`, and an `idempotency_key` (reuse the same key on retry to avoid duplicates)
3. Share preview results and ask for approval

## Step 4: Approve

Run `nitro_control_delivery` with `target_type: "campaign"`, `operation: "approve"`.

This runs preflight checks:
- Can send (within limits)
- Subscription active
- Domain verified (or sandbox)
- Physical address set
- Sender configured

Report any failed checks and help fix them.

## Step 5: Send or Schedule

Ask: send now or schedule for later?

- **Send now**: `nitro_control_delivery` with `operation: "live"` and `confirm_send_to_all: true` only when the audience is `all_contacts`
- **Schedule**: `nitro_control_delivery` with `operation: "schedule"` and `scheduled_at` (ISO 8601)

### Optimal Send Times (from Email Marketing Bible)
- **B2B**: Tuesday-Thursday, 9-11am recipient's timezone
- **B2C**: Tuesday, Thursday, Saturday. 10am or 7-9pm.
- **Newsletters**: Consistency matters more than specific timing
- Avoid Mondays (inbox overload) and Fridays (weekend mindset)

## After Sending

Suggest checking results:
- Use `nitro_get_insights` with `scope: "campaign"` and the campaign ID
- Key metrics: open rate, click rate, unsubscribe rate
- Compare against benchmarks from the email marketing bible
