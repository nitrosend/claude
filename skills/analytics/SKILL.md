---
name: analytics
description: >
  Email analytics, performance insights, and benchmarking. View account-wide
  metrics, campaign performance, flow analytics, and get AI-powered
  recommendations based on email marketing best practices. Use when:
  "email analytics", "campaign performance", "how are my emails doing",
  "email insights", "check my stats", "open rate", or "deliverability report".
---

# Analytics

Provide email analytics and actionable insights using Nitrosend data and email marketing benchmarks.

## Account Overview

Start with `nitro_get_insights` with `scope: "account"` to get the big picture:
- Total emails sent, opens, clicks, unsubscribes, complaints
- Trends (direction: up/down/flat, first half vs second half comparison)
- Benchmarks comparison
- AI recommendations

## Campaign Performance

For specific campaigns, use `nitro_get_insights` with `scope: "campaign"` and `entity_id`.

Key metrics to highlight:
| Metric | Good | Warning | Critical |
|--------|------|---------|----------|
| Open rate | >20% | 10-20% | <10% |
| Click rate | >3% | 1-3% | <1% |
| Unsubscribe rate | <0.2% | 0.2-0.5% | >0.5% |
| Complaint rate | <0.05% | 0.05-0.1% | >0.1% |

## Flow Performance

For automation flows, use `nitro_get_insights` with `scope: "flow"` and `entity_id`.

Flow-specific benchmarks:
| Flow Type | Expected Open Rate | Expected Click Rate |
|-----------|--------------------|---------------------|
| Welcome | 50-80% | 15-25% |
| Cart abandonment | 40-50% | 10-15% |
| Post-purchase | 40-60% | 10-20% |
| Win-back | 15-25% | 2-5% |

## Deep Dive Analysis

When doing monthly or comprehensive analysis:

1. Pull 30-day or 90-day data: `nitro_get_insights` with `period: "30d"` or `"90d"`
2. Query recent campaigns: `nitro_query` with entity `"campaigns"`
3. Query active flows: `nitro_query` with entity `"flows"`, filters `{ status: "active" }`
4. Check list growth: `nitro_query` with entity `"contacts"` for total count
5. Check deliverability signals: complaint rate, bounce rate trends

## Recommendations Engine

Based on the data, proactively suggest improvements from the email marketing bible:

- **Low open rates**: Review subject lines (30-50 chars optimal), test send times, check deliverability
- **Low click rates**: Strengthen CTAs, reduce distractions, ensure mobile-friendly design
- **High unsubscribes**: Review frequency (2-4 emails/month is typical), improve segmentation
- **High complaints**: Clean list, add easy unsubscribe, verify opt-in process
- **Flat growth**: Suggest lead magnet strategies, popup optimization, referral programs
- **No flows active**: Recommend starting with welcome flow (highest ROI automation)

## Proactive Reporting

If the user has set up scheduled analytics (via `/nitrosend:setup`), these reports run automatically:
- **Daily**: New subscribers + sending activity
- **Weekly**: Open/click rates, list growth, deliverability
- **Monthly**: Full benchmark comparison with specific improvement actions
