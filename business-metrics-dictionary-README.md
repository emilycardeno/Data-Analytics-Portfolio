# Business Metrics Dictionary

A single source of truth for business metrics on the COUNT platform, built during a Data Analyst internship at [COUNT](https://www.getcount.com). Defines 60+ metrics across ten business domains, each mapped to its exact underlying data source, so that dashboards, reports, and the AI engine all calculate the same metric the same way, regardless of how a user asks for it.

> **Note on data:** Sample rows below are illustrative and reflect the structure and format of the real deliverable. Client-specific workspace details, exact internal schema mappings, and proprietary implementation specifics have been generalized or omitted.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Why This Exists](#why-this-exists)
3. [Dictionary Structure](#dictionary-structure)
4. [Sample: Revenue Metrics](#sample-revenue-metrics)
5. [Sample: Cash Flow Metrics](#sample-cash-flow-metrics)
6. [Country-Specific Handling](#country-specific-handling)
7. [Companion Project: Time Intelligence Rules](#companion-project-time-intelligence-rules)
8. [Key Challenges & Solutions](#key-challenges--solutions)
9. [Tools & Technologies](#tools--technologies)
10. [Related Projects](#related-projects)

---

## Project Overview

Different teams and users tend to ask for the same underlying number in different words — "revenue," "sales," "turnover," "income" — and without a single governing definition, an AI-powered dashboard will happily calculate each phrasing differently. This project is the fix: a structured dictionary covering **Revenue, Profitability, Cash Flow, Working Capital, Customer Metrics, and Supplier Metrics**, with every metric mapped to a verified data source in the platform's live data model.

Each metric entry defines:
- A **plain-business-language definition** (what it means, what it includes/excludes)
- The exact **formula** used to calculate it
- The **data source** and underlying table/object it's calculated from
- **Synonyms** a user might type instead of the official name, for natural-language matching
- **Drill-down dimensions** for interactive exploration
- **Refresh frequency**, so users know how current the number is

## Why This Exists

Without a governing dictionary:
- The same metric name can mean different things to different teams
- An AI engine may calculate the same question two different ways depending on phrasing
- Developers building new dashboards start from scratch instead of a shared definition
- Reports across the business can quietly disagree with each other

Establishing the dictionary up front meant every new dashboard or AI-generated report could reference an existing, agreed definition instead of re-deriving logic — and gave the AI engine a consistent mapping between user language and the correct calculation.

## Dictionary Structure

Every metric entry uses the same 12-field structure:

| Field | Purpose |
|---|---|
| Metric ID | Unique internal identifier (e.g. `REV001`) |
| Metric Name | User-friendly name shown in dashboards and AI responses |
| Country | Whether the metric is universal or country-specific |
| Business Definition | Plain-language meaning, including inclusions/exclusions |
| Formula | Exact calculation logic |
| Data Source | Business process/module supplying the data |
| Platform Tables | The actual data objects the metric is calculated from |
| Date Field | Which date drives period filtering |
| Aggregation | SUM / AVG / COUNT / Percentage / Ratio |
| Synonyms | Alternative terms users might type, for NL matching |
| Drill Down | Dimensions for deeper investigation |
| Refresh Frequency | How often the metric updates |

## Sample: Revenue Metrics

| Metric | Definition | Formula | Data Source |
|---|---|---|---|
| Revenue | Total invoiced sales recognised during the period. Excludes drafts, quotes, and credit notes. | `SUM(Invoice net amount)` for approved invoices in period | Sales Invoices |
| Revenue Growth % | Percentage change in revenue vs. the prior comparable period | `((Current − Prior) ÷ Prior) × 100` | Sales Invoices |
| Recurring Revenue (MRR/ARR) | Revenue from subscription/recurring billing arrangements | `SUM(net amount from recurring billing templates)` | Recurring Billing, Sales Invoices |
| Revenue per Customer (ARPC) | Average revenue per active billed customer in the period | `Total Revenue ÷ COUNT(DISTINCT active Customer)` | Sales Invoices, Customers |

## Sample: Cash Flow Metrics

| Metric | Definition | Formula | Data Source |
|---|---|---|---|
| Cash Runway | Months the business can operate at current burn before cash runs out | `Current Cash Balance ÷ Average Monthly Burn Rate` | Bank & Cash Accounts, Transactions |
| Burn Rate | Net monthly cash consumption | `Monthly Cash Outflows − Monthly Cash Inflows` | Bank Transactions |
| Cash Conversion Ratio | How well accounting profit converts to actual cash | `Operating Cash Flow ÷ Net Profit` | Cash Flow, Profit & Loss |
| Free Cash Flow (FCF) | Cash left after operations and capital expenditure | `Operating Cash Flow − CapEx` | Bank Transactions, Fixed Assets |

## Country-Specific Handling

Most metrics are identical regardless of country. Where the definition genuinely differs — chiefly tax — the dictionary carries separate rows per country rather than forcing a single definition to cover both:

| Topic | New Zealand | United States |
|---|---|---|
| Indirect tax | GST at 15%, collected for IRD | Sales & use tax, varies by state |
| Tax authority | Inland Revenue Department | IRS + state authorities |
| Financial year | 1 April – 31 March | Calendar year (unless configured otherwise) |
| Reporting currency | NZD | USD (typical) |

A recommendation flagged during this work: adding an **Access/Permissions** field per metric, so visibility can be scoped by role (e.g. a bookkeeper vs. a CFO) — ensuring the AI engine doesn't surface sensitive metrics to users who shouldn't see them.

## Companion Project: Time Intelligence Rules

A related dictionary defining how natural-language time phrases ("this month," "YTD," "since last quarter") resolve to exact date ranges — the semantic layer that sits underneath every metric above. See the [Time Intelligence Rules](../time-intelligence-rules/) project for full detail.

## Key Challenges & Solutions

**1. Conflicting definitions of the same metric across teams**
Different stakeholders sometimes used the same term with different underlying calculations. Rather than resolving unilaterally, differences were brought back to stakeholders and disambiguated with distinct names where the underlying concepts were genuinely different (e.g. separating customer churn from revenue churn).

**2. Country-specific logic without duplicating the whole dictionary**
Rather than maintaining fully separate NZ and US dictionaries, country-specific rows were added only where the definition actually diverges (tax, fiscal year), keeping the ~90% of universal metrics in a single shared definition.

**3. Making the dictionary usable by both humans and an AI engine**
Every metric needed to be readable by a non-technical stakeholder *and* structured enough for an AI engine to generate a correct query from it — solved by pairing a plain-language business definition with an explicit, unambiguous formula and source mapping on every row.

## Tools & Technologies

- Excel for the dictionary structure and stakeholder-facing documentation
- Live platform API verification to confirm every metric maps to a real, queryable data object
- AI tooling (Claude, Copilot, ChatGPT) used to draft documentation and validate formula logic

## Related Projects

- [Time Intelligence Rules](../time-intelligence-rules/)
- [AI Data Requirements for Natural Language Dashboards](../ai-data-requirements/)
- [COUNT Financial API Data Model](../count-financial-api-data-model/)
- [Cash Flow KPI Dashboard & Forecasting Model](../cash-flow-forecasting/)

---

**Contact:** [ecardeno@gmail.com](mailto:ecardeno@gmail.com) · [LinkedIn](https://www.linkedin.com/in/emily-cardeno/) · [GitHub](https://github.com/emilycardeno)
