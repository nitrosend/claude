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

Move a user from Klaviyo to Nitrosend.

1. Ask the user to export profiles (with list membership and consent) and
   their suppressed or unsubscribed profiles from Klaviyo as CSV.
2. Import with `nitro_import_contacts` with `upload`, PUT the file, then call
   `nitro_import_contacts` again with the returned `signed_id` and a column mapping.
   Up to 100 records can go through `nitro_import_contacts` with `records`.
3. Import suppressions as unsubscribed before any sending, and confirm
   subscription status with `nitro_query`.
4. Imports don't start flows unless the user turns on Start flows.
5. Rebuild Klaviyo flows (welcome series, abandoned cart, browse abandonment,
   post-purchase, win-back) with `nitro_compose_flow` as inactive drafts, and
   connect Shopify in Integrations for store events.
