---
name: email-integrations
description: >
  Connect the tools that trigger emails and update contacts: Shopify,
  Stripe, HubSpot, Attio, Mailchimp and ActiveCampaign connect natively in
  the Nitrosend app, and any other app, including Zapier and Make, can send
  contacts and events through the REST API or Events API. Use for email
  integrations and automations between tools.
---

# Email integrations

Connect the tools that should trigger emails or update contacts.

- Shopify, Stripe, HubSpot, Attio, Mailchimp and ActiveCampaign are native
  integrations. The user connects them from Integrations in the Nitrosend
  app; never ask for credentials in chat.
- Any other tool, including Zapier, Make or the user's own app, can send
  contacts and events through the Nitrosend REST API or Events API. Look up
  payloads with `nitro_search_docs`. Don't describe these as native
  integrations.
- Event names are lowercase with underscores; keep each event's fields stable
  once a flow uses them.
- Test with `nitro_manage_audience` (`operation: "record_event"`) on a test contact only after the user
  confirms, because events can start live flows.
