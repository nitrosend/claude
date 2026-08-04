---
name: send-transactional
description: >
  Send transactional emails or SMS for app-triggered communications: receipts,
  password resets, OTPs, order confirmations, shipping notifications, system
  alerts. Single recipient, immediate delivery, no campaign or approval needed.
  Use when: "transactional email", "receipt email", "password reset email",
  "send notification email", "order confirmation", "OTP email", or "system email".
---

# Send Transactional

Help the user send or set up transactional emails/SMS — immediate, single-recipient messages triggered by application events.

## Understand the Use Case

Common transactional email types:
- **Password reset**: OTP or magic link
- **Order confirmation**: Receipt with order details
- **Shipping notification**: Tracking info
- **Welcome/verification**: Account activation
- **System alert**: Service status, security notice
- **Invoice/receipt**: Payment confirmation

## Pre-Send Deliverability Check

Before calling `nitro_send_message` or `nitro_send_test_message` — including a "just send a quick test" request — run `nitro_get_status` and check the account's deliverability signals:

- **Sending domain** — the shared sandbox domain, or a verified custom domain? Sandbox sends are likely to land in spam or Promotions.
- **Sender name** — is `from_name` configured, or blank/generic?
- **Physical address** — is one set on the account? Required for compliance, and weighted by some providers for reputation.
- **Domain verification** — if a custom domain is added, is it actually verified (SPF/DKIM/DMARC confirmed), or only added and still pending?

Transactional sends have no approval gate — unlike campaigns, there is no later checkpoint that catches a bad configuration. This check is the only safeguard, so treat it as required, not optional, even for a quick test.

If any signal is missing or unverified, **stop before sending and ask the user how to proceed.** Do not send first and mention the risk afterward — the send must not happen until the user has responded. For example:

> "Before I send this — you're still on the sandbox domain and haven't set a sender name or physical address. This will very likely land in spam or Promotions, not the inbox. Want me to fix these first (`/nitrosend:setup`), or send anyway to see current behavior?"

Only call `nitro_send_message` / `nitro_send_test_message` after the user has explicitly confirmed how to proceed — either once blockers are resolved, or once they've explicitly said to send anyway.

"Send anyway" applies to the message just discussed, not to the rest of the conversation. If the user asks you to resend or retry that same message, don't re-ask. But a new send request — a different recipient, different content, or a new email entirely — is a new decision: run the check again. The account is likely still in the same broken state, and one acknowledged risk for a test-to-self should not silently waive the check for the next email, especially if it's going to a real recipient.

## Design Approach

### Quick Send (Plain Text)
For a one-off send from Claude over MCP, use `nitro_send_message`:
```
channel: "email"
to: "user@example.com"
subject: "Your order #1234 is confirmed"
body: "Thank you for your order..."
```

### Branded Template
For on-brand transactional emails:
1. Create a template with `nitro_manage_template` via the composition contract
   (`composition_mode: "intent"` → fill the returned `next_call` → persist with
   `composition_mode: "draft"`; see the compose-email skill)
2. For a one-off MCP send, call `nitro_send_message` with `template_id`
3. For application-triggered production sends, wire `POST /v1/my/messages` or `ns.messages.send` using `template_id`

Live sends require an `idempotency_key` — reuse the same stable key on retry to
prevent duplicate delivery.

### Merge Variables
Use the `data` field for personalization:
```
data: { order_id: "1234", customer: { name: "Alice" }, amount: "$49.99" }
```

Reference in templates/body as `{{ data.order_id }}`, `{{ data.amount }}`, or
nested paths like `{{ data.customer.name }}`.

## Key Differences from Marketing Email

| | Marketing | Transactional |
|---|---|---|
| Audience | Lists/segments | Single recipient |
| Requires subscription | Yes | No |
| CAN-SPAM footer | Required | Skipped |
| Unsubscribe link | Required | Not needed |
| Delivery | Campaign workflow | Immediate |
| Tracking | Opens/clicks tracked | Opens/clicks tracked (engagement tracking stays enabled) |

Do not model transactional sends as MCP flows. Use `nitro_send_message` for one-off MCP sends, normal flow email steps for lifecycle automation, and API/SDK transactional sends for application events.

## For Developers

Help developers integrate transactional emails into their apps:

### API Integration
```
POST https://api.nitrosend.com/v1/my/messages
Authorization: Bearer nskey_live_...
Idempotency-Key: verification-user-123-attempt-1
Content-Type: application/json

{
  "channel": "email",
  "to": "user@example.com",
  "subject": "Your verification code",
  "body": "Your code is: 123456",
  "data": { "code": "123456" }
}
```

### SDK Integration
```javascript
import { Nitrosend } from '@nitrosend/sdk'
const ns = new Nitrosend('nskey_live_...')
await ns.messages.send({
  channel: 'email',
  to: 'user@example.com',
  subject: 'Your verification code',
  template_id: 42,
  data: { code: '123456' }
}, 'verification-user-123-attempt-1')
```

## Dry Run

Always offer to do a dry run first:
`nitro_send_message` or `POST /v1/my/messages` with `dry_run: true` validates without sending.

## Idempotency

Every live send must have a stable idempotency key. Reuse it only for an exact
retry of the same recipient and payload; use a new key for a different logical
message. MCP uses `idempotency_key`, the REST API uses the `Idempotency-Key`
header, and the Node SDK accepts the key as the second argument to
`messages.send`.
