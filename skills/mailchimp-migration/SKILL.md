---
name: mailchimp-migration
description: >
  Migrate from Mailchimp to Nitrosend: connect Mailchimp to bring audiences,
  members and merge fields with subscribed, unsubscribed and cleaned status
  preserved, then rebuild the campaigns and customer journeys you still need
  as Nitrosend emails and flows. Use when moving an email list from
  Mailchimp.
---

# Mailchimp migration

Move a user from Mailchimp to Nitrosend without losing consent.

1. The Mailchimp connector in Nitrosend Integrations imports audiences as
   lists, members as contacts, names and common merge fields, and source
   metadata. Only `subscribed` members are opted in; unsubscribed, cleaned,
   pending and archived members arrive without email opt-in.
2. The user connects Mailchimp in the Nitrosend app (OAuth); don't ask for
   Mailchimp credentials in chat.
3. Campaign history, templates and reports don't migrate. Rebuild the emails
   and customer journeys the user still needs with `nitro_compose_campaign`
   and `nitro_compose_flow`, saved as drafts.
4. Nothing sends and no flow activates as part of the migration. Confirm
   subscription counts with `nitro_query` before any send.
