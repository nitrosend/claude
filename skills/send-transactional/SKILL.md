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

## Design Approach

### Quick Send (Plain Text)
For simple messages, use `nitro_send_message` directly:
```
channel: "email"
to: "user@example.com"
subject: "Your order #1234 is confirmed"
body: "Thank you for your order..."
```

### Branded Template
For on-brand transactional emails:
1. Create a template with `nitro_manage_template` using sections
2. Then send with `nitro_send_message` using `template_id`

### Merge Variables
Use the `data` field for personalization:
```
data: { order_id: "1234", name: "Alice", amount: "$49.99" }
```

Reference in templates/body as `{{ order_id }}`, `{{ name }}`, etc.

## Key Differences from Marketing Email

| | Marketing | Transactional |
|---|---|---|
| Audience | Lists/segments | Single recipient |
| Requires subscription | Yes | No |
| CAN-SPAM footer | Required | Skipped |
| Unsubscribe link | Required | Not needed |
| Delivery | Campaign workflow | Immediate |
| Tracking | Opens/clicks tracked | Optional |

When using transactional in a flow, set `transactional: true` on the email step.

## For Developers

Help developers integrate transactional emails into their apps:

### API Integration
```
POST https://api.nitrosend.com/v1/my/messages
Authorization: Bearer nskey_live_...
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
})
```

## Dry Run

Always offer to do a dry run first:
`nitro_send_message` with `dry_run: true` validates without sending.

## Idempotency

For production use, recommend `idempotency_key` to prevent duplicate sends on retries.
