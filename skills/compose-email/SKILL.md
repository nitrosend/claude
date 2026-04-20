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

## Create or Update

**New template**: Use `nitro_manage_template` with `sections` array and `subject`.

**Update existing**: Use `nitro_manage_template` with `template_id` and the fields to change.

**Clone existing**: Use `nitro_manage_template` with `based_on` (source template ID).

Always set a descriptive `name` for the template.

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
