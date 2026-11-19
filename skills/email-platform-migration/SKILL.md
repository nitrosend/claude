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

Move contacts, lists, tags, custom fields, templates and automations from
MailerLite, ActiveCampaign, HubSpot, Brevo (Sendinblue), Kit (ConvertKit),
beehiiv, Substack, Constant Contact, Omnisend, Drip, Campaign Monitor, AWeber,
GetResponse or any CSV export.

1. Mailchimp and ActiveCampaign have native connectors in Integrations in the
   Nitrosend app (see `mailchimp-migration`); the user connects them there.
   For other platforms, export contacts and suppressions as CSV and import
   them with `nitro_import_contacts` (upload, then `signed_id`).
2. Apply the old platform's unsubscribes and suppressions before any send or
   flow activation, then confirm subscription status.
3. Recreate the templates and automations the user still needs as drafts.
4. A migration never widens consent: an address being on the old list is not
   proof of permission.
