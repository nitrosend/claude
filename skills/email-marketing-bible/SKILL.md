---
name: email-marketing-bible
description: >
  Apply durable email-marketing principles without treating static benchmarks,
  laws, vendor features, or prices as current facts. Use for strategy, flows,
  deliverability, copy, segmentation, measurement, and platform evaluation.
---

# Email Marketing Reference

Use this skill as a reasoning framework, not a current-facts database.

## Freshness Rules

- Prefer the active account's Nitrosend data and the benchmarks returned by
  `nitro_get_insights`.
- Verify mailbox-provider requirements, laws, vendor features, plan limits,
  pricing, and market comparisons from fresh authoritative sources before
  presenting them as current.
- Label external benchmark ranges with their source, period, audience, and
  methodology. Never turn them into universal pass/fail thresholds.
- Treat open rates as directional because privacy and automated fetching can
  distort them. Prefer clicks, conversions, replies, complaints, delivery, and
  revenue when available.
- Never claim a tactic guarantees delivery, placement, revenue, or compliance.

## Program Foundations

A sound email program has six connected parts:

1. A consented, well-maintained audience
2. Authenticated and monitored sending identity
3. Clear lifecycle and broadcast strategy
4. Accessible, useful content
5. Safe automation and delivery controls
6. Measurement tied to the business goal

Choose the message type first:

- **Campaign:** one broadcast to a subscribed audience
- **Flow:** repeatable automation triggered by an event or audience condition
- **Transactional:** an immediate message required by one user's action

Do not use campaigns for receipts or password resets, and do not model every
one-off transactional event as a flow.

## Audience and Consent

- Import only contacts with an established lawful basis or consent appropriate
  to the channel and jurisdiction.
- Never use purchased or scraped marketing lists.
- Keep source and consent evidence. Do not overwrite an existing opt-out during
  a routine import.
- Use lists for explicit membership, tags for durable facts, and segments for
  dynamic rules.
- Segment by meaningful behavior or lifecycle state before adding superficial
  personalization.
- Reduce or stop promotional sends to persistently unengaged recipients, but
  define engagement using evidence available to the account rather than one
  universal age threshold.

## Lifecycle Design

Start with the lifecycle moments most directly supported by the business's
events and data. Common candidates include welcome, onboarding, cart or browse
recovery, post-purchase, replenishment, renewal, win-back, and sunset.

For each flow:

1. Define the trigger and eligibility precisely.
2. State the user value and business goal.
3. Keep each message focused on one next action.
4. Add waits and branches only when they change the experience meaningfully.
5. Protect against duplicate entry, conflicting campaigns, and excessive
   frequency.
6. Define an exit condition and the metric used to evaluate the flow.

Use account results to tune timing and cadence. Generic timing advice is a test
hypothesis, not a fact about a particular audience.

## Copy

- Make the subject and preheader a truthful, complementary promise.
- Put the essential value early and use short, scannable paragraphs.
- Use one clear primary action. Secondary links should not compete with it.
- Prefer concrete evidence, product details, and user outcomes over generic
  superlatives.
- Preserve exact offers, URLs, legal text, and operator-supplied constraints.
- Personalization must degrade safely when a value is absent and must never
  expose internal data.
- Review generated copy for brand voice, factual accuracy, accessibility, and
  the risk of manipulative or misleading claims.

Useful structures include problem-solution, before-after-bridge, a concise
announcement hierarchy, a founder letter, and a utility-first transactional
layout. Choose the structure that matches the job rather than forcing every
email into the same formula.

## Design and Accessibility

- Design for narrow screens first and keep hierarchy obvious.
- Use readable type, sufficient contrast, descriptive alt text, and meaningful
  link text.
- Avoid making the message understandable only through images.
- Make tap targets comfortable and keep the primary action visually distinct.
- Expect dark-mode and image-blocking variation; test the actual rendered
  output instead of relying on design-source appearance.
- Keep the legal identity and unsubscribe mechanism intact for marketing mail.

## Deliverability

Deliverability is a system outcome, not a content score. Diagnose it in order:

1. Confirm sender identity and authentication state.
2. Inspect delivery failures, bounce categories, complaints, and suppressions.
3. Compare volume and cadence with the sender's recent history.
4. Check audience source, consent, and engagement quality.
5. Inspect content and link behavior only after infrastructure and audience
   causes are considered.
6. Make one bounded change and monitor recovery using provider evidence.

Keep transactional and marketing purposes operationally distinct when the
business's scale and risk justify it. Follow the domain and warmup limits
returned by Nitrosend; do not substitute a static ramp schedule.

## Testing

- Start from a falsifiable hypothesis tied to one decision.
- Change one meaningful variable at a time unless the design explicitly uses a
  multivariate method.
- Preselect the success metric, guardrails, population, and observation window.
- Avoid declaring a winner from a tiny sample or from opens alone.
- Apply learnings to durable flows when the evidence generalizes; do not copy a
  result across audiences without checking context.

## Measurement

Match the primary metric to the message job:

- Broadcast: conversion, revenue, clicks, complaints, and unsubscribe impact
- Lifecycle flow: completion, conversion, time-to-value, and incremental lift
- Transactional: delivery success, latency, and task completion
- Newsletter or nurture: qualified clicks, replies, retained engagement, and
  downstream conversion

Report absolute counts alongside rates. State the period, denominator, and
known tracking limitations. Compare with the account's own prior period or a
similar cohort before reaching for broad industry data.

## Compliance

Operational basics include truthful sender identity, a valid physical address
where required, a working unsubscribe path for marketing messages, prompt
suppression of opt-outs, and retained consent records. Exact duties vary by
jurisdiction, message type, audience, and current law. Verify the applicable
rules from an authoritative current source and recommend legal review for
material uncertainty.

## Platform Evaluation

Do not use a static vendor table. Build a current comparison from the user's
requirements and verified provider evidence:

- audience size and sending volume
- campaigns, lifecycle automation, and transactional needs
- integrations and data model
- consent, security, residency, and governance requirements
- deliverability controls and support
- API, SDK, MCP, and operator workflow needs
- total cost at current and expected scale
- migration and lock-in risk

For Nitrosend behavior, use the live MCP schemas and `nitro_search_docs`.
Verify every competing vendor's current pricing and feature claim from its
official source.

## Cold Outreach Boundary

Do not treat a marketing ESP or the presence of an outreach-discovery tool as
permission to send cold email. Establish the lawful basis, channel policy,
sender identity, audience provenance, rate limits, and human authorization
before any outbound action. Discovery and enrichment are separate from send
authority.
