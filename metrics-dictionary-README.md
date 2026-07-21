# Business Metrics Data Dictionary

A metrics dictionary defining 20–30 key business metrics, built during a Data Analyst internship at [COUNT](https://www.getcount.com) from a full dashboard audit and stakeholder interviews across 8–10 cross-functional teams. Used to establish a shared definition of "what a metric means" across the organisation, and to inform a scalable dashboard framework and prioritised analytics roadmap.

> **Note on data:** Metric names, definitions, and structure shown here are reconstructed/generalized examples reflecting the approach and format of the real project. No proprietary COUNT metrics, internal figures, or confidential business logic are included.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Process](#process)
3. [Sample Metric Dictionary Entries](#sample-metric-dictionary-entries)
4. [Dashboard Framework](#dashboard-framework)
5. [Analytics Roadmap](#analytics-roadmap)
6. [Key Challenges & Solutions](#key-challenges--solutions)
7. [Tools & Technologies](#tools--technologies)
8. [Related Projects](#related-projects)

---

## Project Overview

Before this project, different teams referred to the same metric name with different underlying calculations — or used different names for the same thing. That ambiguity made cross-team reporting unreliable and slowed down dashboard development, since every new dashboard request started with a definitional argument rather than a build.

This project fixed that at the source: a single, agreed-upon dictionary of business metrics, each with an explicit definition, calculation logic, data source, and owning team — built directly from what stakeholders said they needed, not assumed from the data alone.

## Process

1. **Dashboard audit** — reviewed existing dashboards and reports across the organisation to catalogue every metric currently in use, including duplicates and near-duplicates with inconsistent definitions
2. **Stakeholder interviews** — structured interviews across 8–10 cross-functional teams to understand what each team actually needed to track, how they currently defined it, and where gaps existed
3. **Metric consolidation** — merged overlapping/conflicting metric definitions into a single canonical version per metric, flagging disagreements for stakeholder sign-off rather than resolving them unilaterally
4. **Documentation** — wrote each metric into a standard dictionary format (below), reviewed with the relevant team owner before finalizing
5. **Roadmap output** — used gaps surfaced during interviews to prioritise a forward analytics roadmap

## Sample Metric Dictionary Entries

| Metric | Definition | Calculation | Source | Owner |
|---|---|---|---|---|
| Monthly Recurring Revenue (MRR) | Predictable revenue normalized to a monthly basis | `sum(active_subscription_value) / 12` for annual plans, plus monthly plans at face value | Billing system | Finance |
| Customer Acquisition Cost (CAC) | Average cost to acquire one new paying customer in a period | `total_sales_and_marketing_spend / new_customers_acquired` | CRM + Finance | Marketing |
| Net Revenue Retention (NRR) | % of revenue retained from existing customers, including upgrades/downgrades, excluding new customers | `(starting_MRR + expansion - contraction - churn) / starting_MRR` | Billing system | Customer Success |
| Days Sales Outstanding (DSO) | Average days to collect receivables | `(accounts_receivable / total_credit_sales) × days_in_period` | Accounting system | Finance |
| Support Ticket Resolution Time | Average time from ticket creation to resolution | `avg(resolved_at - created_at)` for closed tickets | Support platform | Customer Success |

Each real dictionary entry also included: a plain-language description for non-technical stakeholders, known edge cases (e.g. how refunds affect MRR), and a "last reviewed" date to keep definitions from silently going stale.

## Dashboard Framework

Metrics were organised into three layers to keep dashboards consistent and maintainable:

```
┌─────────────────────────────────────────┐
│              Reporting Layer             │  ← Dashboards, exec summaries
│   (pre-built views for each stakeholder  │
│         group, using metrics below)      │
└───────────────────┬───────────────────────┘
                     │
┌────────────────────▼──────────────────────┐
│               Metrics Layer                │  ← This dictionary
│  (canonical, agreed-upon metric definitions)│
└────────────────────┬───────────────────────┘
                     │
┌────────────────────▼──────────────────────┐
│             Transactional Layer             │  ← Raw source data
│   (transactions, invoices, tickets, etc.)   │
└──────────────────────────────────────────────┘
```

Keeping the metrics layer separate meant a new dashboard could be built by referencing existing metric definitions rather than recalculating logic from scratch — reducing the risk of the same metric being computed two different ways in two different dashboards.

## Analytics Roadmap

Interview findings were used to produce a prioritised roadmap of gaps, generally structured as:

1. **High priority** — metrics multiple teams needed but nobody currently tracked (e.g. NRR)
2. **Medium priority** — metrics tracked inconsistently across teams, needing consolidation
3. **Lower priority / future** — metrics only one team needed, or requiring data not yet captured

## Key Challenges & Solutions

**1. Conflicting definitions of the same metric name**
Two teams both had a metric called "churn," calculated differently. Rather than picking one unilaterally, brought both definitions back to stakeholders, renamed one to disambiguate (e.g. "Customer Churn" vs. "Revenue Churn"), and documented both as distinct, valid metrics.

**2. Getting straight answers in interviews**
Early interviews returned vague answers like "we just want better visibility." Refined the interview structure to ask for a specific decision the metric would inform ("what would you do differently if this number changed?"), which produced far more usable requirements.

**3. Keeping the dictionary from going stale**
Added a "last reviewed" field and assigned an owning team per metric, so definitions had a clear point of accountability rather than becoming an orphaned document.

## Tools & Technologies

- Excel for the dictionary structure and stakeholder-facing documentation
- Structured interview templates for stakeholder discovery
- AI tooling (Claude, Copilot, ChatGPT) used to draft documentation, structure interview questions, and suggest dashboard framework organisation

## Related Projects

- [COUNT Financial API Data Model](../count-financial-api-data-model/)
- [Cash Flow KPI Dashboard & Forecasting Model](../cash-flow-forecasting/)
- [AI-Powered Natural Language Financial Dashboard](../ai-financial-dashboard/)

---

**Contact:** [ecardeno@gmail.com](mailto:ecardeno@gmail.com) · [LinkedIn](https://www.linkedin.com/in/emily-cardeno/) · [GitHub](https://github.com/emilycardeno)
