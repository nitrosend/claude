---
name: import-contacts
description: >
  Import contacts into Nitrosend from inline records or CSV. Handles email
  opt-in, SMS TCPA compliance, list assignment, and tagging. Use when:
  "import contacts", "add subscribers", "upload contacts", "add to list",
  "bulk add contacts", or "import CSV".
---

# Import Contacts

Help the user import contacts into Nitrosend.

## Understand the Source

Ask where contacts are coming from:
- **Inline records**: User provides names/emails directly (up to 100 at a time)
- **CSV file**: Reference a pre-uploaded CSV import
- **One by one**: Use `nitro_manage_audience` with `operation: "create_contact"` for individual contacts

## Inline Import (< 100 contacts)

Use `nitro_import_contacts` with `records` array:

```json
{
  "records": [
    { "email": "alice@example.com", "first_name": "Alice", "last_name": "Smith" },
    { "email": "bob@example.com", "first_name": "Bob", "source": "webinar-2026" }
  ]
}
```

### Important Rules
- **Email contacts**: Auto-subscribed by default (`opt_in: true` implied)
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
- `tags: ["webinar-attendee", "2026-q1"]`
- `tag_action: "add"` (default), `"remove"`, or `"set"`

## Compliance Reminders

- Only import contacts who have given consent to receive emails
- Never import purchased or scraped lists
- Include a source identifier for audit trails
- SMS contacts require explicit written consent (TCPA)
- GDPR: Ensure lawful basis for EU contacts
