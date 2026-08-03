---
name: compose-email
description: >
  Create or edit email templates with Nitrosend's section-based designer.
  Guides through layout, content, and brand styling. Supports preview and
  test sends. Use when: "compose email", "design template", "create email",
  "email template", "edit template", or "update email design".
---

# Compose Email

Help the user create or edit an email template using Nitrosend's section-based email designer.

## Understand the Goal

Ask what the email is for:
- Newsletter / announcement
- Welcome email
- Promotional / sale
- Product update
- Event invitation
- Transactional (receipt, confirmation, etc.)
- Re-engagement

## Design the Email

Build the email using sections. Available section types:

| Section | Key Props |
|---------|-----------|
| `header` | `logo_url`, `logo_alt`, `logo_width`, `background_color` |
| `text` | `content` (HTML string) |
| `image` | `src`, `alt`, `href`, `width` |
| `button` | `text`, `href`, `background_color`, `text_color`, `align`, `border_radius` |
| `columns` | `columns: [{width, sections: [...]}]` — nested sections |
| `product` | `name`, `price`, `image_url`, `href`, `description` |
| `social` | `links: [{platform, url}]`, `align` |
| `divider` | `color`, `width`, `padding` |
| `spacer` | `height` |
| `footer` | `company_name`, `address`, `unsubscribe_text` |

### Best Practices (from Email Marketing Bible)
- **Subject line**: 30-50 characters, 6-10 words. Front-load the value.
- **Preheader**: Complement the subject, don't repeat it. 40-130 characters.
- **Mobile-first**: 60%+ opens are on mobile. Single-column layouts work best.
- **One primary CTA**: Make it obvious. Use a button, not a text link.
- **Dark mode**: Use transparent PNGs for logos, avoid pure white backgrounds.
- **Footer**: Always include company name, physical address, and unsubscribe link.

## Composition Contract

Newly authored copy or full sections always enter the composition contract
before persistence:

1. Call `nitro_manage_template` with `composition_mode: "intent"` — the
   response includes the current brand, memory, source, binding, and design
   context plus a `next_call` scaffold
2. Fill `next_call` with your authored sections and send it back, preserving
   its contract and idempotency fields; optionally check it first with
   `composition_mode: "validate"` (no persistence)
3. Persist with `composition_mode: "draft"`

Alternatively, request server-side authoring via the returned metered
`composition_mode: "generate"` call, which composes, validates, and persists one
draft. Supplying creative fields without a contract returns the intent contract
instead of silently creating anything. Targeted `section_updates` (exact
`text_patch` copy fixes, prop/style tweaks) and clones without creative
overrides do not need recomposition.

## Create or Update

**New template**: Use `nitro_manage_template` with `sections` array and `subject` (via the composition contract above).

**Update existing**: Use `nitro_manage_template` with `template_id` and the fields to change — prefer `section_updates` for targeted edits.

**Clone existing**: Use `nitro_manage_template` with `based_on` (source template ID).

Always set a descriptive `name` for the template.

## Images

Host images on Nitrosend storage with `nitro_ingest` (V1 is image-only):

- Small local or chat-attached images: pass `image_data`
- Larger local files: request a direct upload with `upload: {kind, filename,
  content_type, byte_size, checksum}`, PUT the bytes to the returned
  `direct_upload.url`, then call `nitro_ingest` again with the `signed_id`
- Public remote image URLs: use directly in sections when permanence is not
  needed, or pass as `image_url` for a Nitrosend-hosted copy

The returned hosted URL is what goes into the `image` section's `src`.

## Preview and Test

After creating/updating:
1. Use `nitro_review_delivery` with `target_type: "template"` and `target_id` to review content, validate, and check delivery readiness
2. Ask if they want a test email sent
3. If yes, use `nitro_send_test_message` with `target_type: "template"` and `target_id` — sends to saved test recipients or specify explicit recipients with `to`

## Merge Tags

Available personalization variables:
- `{{ first_name }}`, `{{ last_name }}`, `{{ email }}`
- `{{ unsubscribe_url }}` — required in marketing emails
- Custom event data fields when used in flows
