---
name: klaviyo-migration
description: >
  Migrate from Klaviyo to Nitrosend: import profiles and lists exported from
  Klaviyo as CSV, keep unsubscribes and suppressions, then rebuild flows
  such as welcome series, abandoned cart and post-purchase as Nitrosend
  drafts and connect Shopify for store events. Use when moving email
  marketing from Klaviyo.
---

# Klaviyo migration

For a full move (opt-outs first, a count check, engagement and history,
domain, flows, warmup and cutover), use the `migrate-account` skill
(`/nitrosend:migrate-account`).

Move a user from Klaviyo to Nitrosend.

1. Ask the user to export profiles (with list membership and consent) and
   their suppressed or unsubscribed profiles from Klaviyo as CSV.
2. Import with `nitro_import_contacts` with `upload`, PUT the file, then call
   `nitro_import_contacts` again with the returned `signed_id` and a column mapping.
   Up to 100 records can go through `nitro_import_contacts` with `records`.
3. An import subscribes new contacts unless a column says otherwise. Keep
   Klaviyo's Email Marketing Consent column in the file (map it to
   `email_status` in the column mapping if it isn't picked up), and give a
   suppression export that has no status column an `Unsubscribed` column set
   to `true` before importing it. Never import suppressed or unsubscribed
   profiles as plain addresses. Do this before any sending, and confirm
   subscription status with `nitro_query`.
4. Imports don't start flows unless the user turns on Start flows.
5. Rebuild Klaviyo flows (welcome series, abandoned cart, browse abandonment,
   post-purchase, win-back) with `nitro_compose_flow` as inactive drafts, and
   connect Shopify in Integrations for store events.
