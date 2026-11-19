---
name: import-contacts
description: >
  Import contacts into Nitrosend from inline records or CSV. Handles email
  opt-in, SMS TCPA compliance, list assignment, and tagging. Use when:
  "import contacts", "add subscribers", "upload contacts", "add to list",
  "bulk add contacts", or "import CSV". For a move from another email tool,
  use migrate-account instead.
---

# Import Contacts

Help the user import contacts into Nitrosend.

## Migrating From Another Email Tool?

If the contacts are coming out of another email tool (Klaviyo, Mailchimp,
Brevo, ActiveCampaign, beehiiv, MailerLite, Kit, HubSpot, Shopify, Substack,
Omnisend, GoHighLevel or any other), stop here and use the `migrate-account`
skill (`/nitrosend:migrate-account`). A migration imports unsubscribes,
bounces and complaints before any contact and checks the counts against the
old tool. A plain contact import does neither, and a list exported from
another tool can include people who opted out.

Use this skill for everything else: a signup sheet, an event list, a handful
of contacts, or a CSV the user collected themselves.

## Understand the Source

Ask where contacts are coming from:
- **Inline records**: User provides names/emails directly (up to 100 at a time)
- **CSV file**: Upload a local CSV through the direct-upload path (below)
- **One by one**: Use `nitro_manage_audience` with `operation: "create_contact"` for individual contacts

## CSV Import (direct upload)

For a local CSV, `nitro_import_contacts` runs a three-step upload:

1. Call it with `upload: {filename, content_type, byte_size, checksum}` to
   reserve an authorized upload link
2. PUT the file bytes to the returned `direct_upload.url`, sending exactly the
   returned `direct_upload.headers`
3. Call `nitro_import_contacts` again with the returned `signed_id` (plus a
   `columns` mapping) to finalize; an existing `import_id` can also be processed

Large CSVs use the same async pipeline as the app/API/CLI: up to 250k rows
self-serve, sends held for review above 20k rows, larger files return
`contact_sales`.

## Inline Import (< 100 contacts)

Use `nitro_import_contacts` with `records` array:

```json
{
  "records": [
    { "email": "alice@example.com", "first_name": "Alice", "last_name": "Smith" },
    { "email": "bob@example.com", "first_name": "Bob", "source": "product-webinar" }
  ]
}
```

### Important Rules
- **Email contacts**: New contacts are auto-subscribed by default; existing
  contacts keep their current subscription state unless `opt_in` is explicit
- **SMS contacts**: Must explicitly set `opt_in: true` (TCPA compliance)
- **Both channels**: Include both `email` and `phone` fields
- **Source tracking**: Set `source` to track where contacts came from

Always offer `dry_run: true` first to preview.

## After Import

### Add to a List
Use `nitro_manage_audience` with `operation: "manage_list"`:
- Create a list: `action: "create"`, `name: "List Name"`
- Add contacts: `action: "add_contacts"`, `list_id`, `emails: [...]` or `contact_ids: [...]`

### Tag Contacts
Use `nitro_manage_audience` with `operation: "bulk_tag"`:
- `contact_ids: [...]`
- `tags: ["webinar-attendee", "product-interest"]`
- `tag_action: "add"` (default), `"remove"`, or `"set"`

## Compliance Reminders

- Only import contacts who have given consent to receive emails
- Never import purchased or scraped lists
- Include a source identifier for audit trails
- SMS contacts require explicit written consent (TCPA)
- GDPR: Ensure lawful basis for EU contacts
