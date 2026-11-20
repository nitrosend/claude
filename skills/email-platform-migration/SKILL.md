---
name: email-platform-migration
description: >
  Switch email platforms: connect ActiveCampaign, or import contacts, lists,
  tags and custom fields exported as CSV from MailerLite, Brevo
  (Sendinblue), Kit (ConvertKit), beehiiv, Substack, Constant Contact,
  Omnisend or any other platform, keeping unsubscribes and suppressions,
  then rebuild the emails you need. Use for any email migration.
---

# Email platform migration

For a full move (opt-outs first, a count check, engagement and history,
domain, flows, warmup and cutover), use the `migrate-account` skill
(`/nitrosend:migrate-account`).

Move contacts, lists, tags and custom fields from MailerLite, ActiveCampaign,
HubSpot, Brevo (Sendinblue), Kit (ConvertKit), beehiiv, Substack, Constant
Contact, Omnisend, Drip, Campaign Monitor, AWeber, GetResponse or any CSV
export, then rebuild the templates and automations the user still needs.

1. Mailchimp and ActiveCampaign have native connectors in Integrations in the
   Nitrosend app (see `mailchimp-migration`); the user connects them there.
   For other platforms, export contacts and suppressions as CSV and import
   them with `nitro_import_contacts` (upload, then `signed_id`).
2. An import subscribes new contacts unless a column says otherwise. Keep the
   old platform's status column (subscribed, unsubscribed, cleaned, bounced),
   and give an unsubscribe or suppression list that has no status column an
   `Unsubscribed` column set to `true` before importing it. Apply the old
   platform's unsubscribes and suppressions this way before any send or flow
   activation, then confirm subscription status.
3. Recreate the templates and automations the user still needs as drafts.
4. A migration never widens consent: an address being on the old list is not
   proof of permission.
