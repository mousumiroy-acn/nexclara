# Nexlara Technologies — Data Package Guide
## Appendix A · For Participants

---

## What Is in This Package

You have received eight data files. Together they represent a snapshot of Nexlara Technologies' commercial operations — opportunities, quotes, approvals, deal health flags, renewal risks, and product catalogue. A ninth and tenth file may be released later in the day if your team asks the right questions.

All data is synthetic. All company names, customer names, and employee identifiers are fictional.

This appendix supports [`../participant-brief.md`](../participant-brief.md).

---

## The Eight Files

| File | What it contains | Rows |
|---|---|---|
| [`opportunity_pipeline.csv`](opportunity_pipeline.csv) | 120 open sales opportunities across geographies, deal motions, customer segments, and pipeline stages | 120 |
| [`quote_header.csv`](quote_header.csv) | One row per quote version linked to an opportunity — status, approval outcomes, rejection reason, Machine Learning (ML) approval score, license model | 85 |
| [`bom_line_items.csv`](bom_line_items.csv) | Individual product line items (Stock-Keeping Units) attached to each quote — pricing, Lower Eligibility Price (LEP) compliance, auto-attach flags, Stand-Alone Selling Price (SSP) documentation | ~360 |
| [`dhi_flags.csv`](dhi_flags.csv) | Deal Health Index (DHI) compliance flags raised on quotes — code, severity, status, mitigation and approver comments | ~175 |
| [`approval_events.csv`](approval_events.csv) | Timestamped event log of every approval action — submissions, approvals, rejections, escalations, comments | ~300 |
| [`renewal_risk_signals.csv`](renewal_risk_signals.csv) | Customer-level renewal health data for 48 renewal-stage accounts — adoption score, Net Promoter Score (NPS), churn risk, Contract Renewal Executive (CRE) engagement status | 48 |
| [`sku_catalogue.csv`](sku_catalogue.csv) | Master product and SKU reference — pricing, LEP, lifecycle status, pre-approved catalogue eligibility, ASC 606 SSP data | 66 |
| [`kpi_benchmarks.csv`](kpi_benchmarks.csv) | Internal Nexlara metrics and industry benchmarks for key commercial performance indicators | 18 |

---

## How the Files Relate to Each Other

```
opportunity_pipeline.csv
    │
    ├──► quote_header.csv          (opp_id links quotes to opportunities)
    │         │
    │         ├──► bom_line_items.csv   (quote_id links BOM lines to quotes)
    │         │         │
    │         │         └──► sku_catalogue.csv  (sku_id links BOM lines to SKU master)
    │         │
    │         ├──► dhi_flags.csv        (quote_id links flags to quotes)
    │         │
    │         └──► approval_events.csv  (quote_id links events to quotes)
    │
    └──► renewal_risk_signals.csv  (account_name links renewal signals to opportunities)
```

[`kpi_benchmarks.csv`](kpi_benchmarks.csv) is a standalone reference table — it does not join to other files.

---

## Three Metrics You Must Derive

Three metrics are intentionally not given to you directly. You must calculate them from the data and document your methodology as part of your diagnosis.

**1. Number of quote revisions per opportunity**
- Source: [`quote_header.csv`](quote_header.csv) grouped by `opp_id`
- Hint: count distinct `quote_version` values per `opp_id`. What is the distribution? What is the average? What is the maximum?

**2. Opportunity win rate**
- Source: [`opportunity_pipeline.csv`](opportunity_pipeline.csv)
- Hint: calculate `Closed Won / (Closed Won + Closed Lost)` overall and by deal motion, geography, and segment. Do the rates differ meaningfully across these cuts?

**3. Average deal value**
- Source: [`bom_line_items.csv`](bom_line_items.csv) aggregated by `quote_id`, joined to [`opportunity_pipeline.csv`](opportunity_pipeline.csv)
- Hint: sum `total_acv_eur` per quote, then average by deal motion and product line. How does average deal value differ between Net New, Upsell/Cross-sell, and Renewal?

Your team's methodology and findings for these three metrics should appear in your current-state diagnosis.

---

## Key Columns to Know

### [`quote_header.csv`](quote_header.csv)
- `quote_version` — version 1 is the first submission; higher versions follow rejections
- `quote_status` — `Draft` · `Submitted` · `Approved` · `Rejected` · `Withdrawn`
- `license_model` — `Perpetual` · `Subscription` · `Subscription+Overage` · `Consumption`. Critical for understanding ASC 606 (Accounting Standards Codification 606) revenue recognition implications
- `ml_approval_score` — predicted probability of first-pass approval at submission time (0–1). Treat with appropriate scepticism
- `days_in_approval` — how long the quote has been with approvers
- `rejection_reason_code` — standardised code for why the quote was rejected; null if not rejected
- `approvals_count_this_version` — number of approver-level actions on this specific version; combine with [`approval_events.csv`](approval_events.csv) to understand the full approval lifecycle

### [`bom_line_items.csv`](bom_line_items.csv)
- `is_lep_compliant` — False means the net price is below the minimum allowed floor
- `is_auto_attached` — True means the system added this SKU automatically; the Account Executive (AE) did not select it
- `ssp_documented` — whether a Stand-Alone Selling Price is on file (required for ASC 606 on Consumption and Subscription+Overage lines)
- `license_model` — line-level license model; a single quote can have lines with different models
- `product_lifecycle_status` — `Active` · `Sunset Notice` · `Deprecated`

### [`dhi_flags.csv`](dhi_flags.csv)
- `severity` — `Blocker` (prevents approval) · `Warning` (flagged for review) · `Info`
- `status` — `Open` · `Mitigated` · `Accepted` · `Waived`
- `approver_comment` — what the approver said about this flag; may be vague or actionable

### [`approval_events.csv`](approval_events.csv)
- `event_type` — `Submitted` · `Approved` · `Rejected` · `Escalated` · `Comment Added` · `Resubmitted` · `Withdrawn`
- `approver_ooo_flag` — True means the approver was Out-Of-Office (OOO) when this event was recorded
- `comment_text` — free text from approvers; study the quality and actionability of these comments

### [`renewal_risk_signals.csv`](renewal_risk_signals.csv)
- `renewal_risk_score` — composite score from 0 (low risk) to 1 (high risk of churn)
- `cre_engagement_started` — False on a high-risk account is a problem worth investigating
- `product_adoption_score` — derived from usage telemetry; low adoption plus high price increase is a specific combination worth finding

### [`sku_catalogue.csv`](sku_catalogue.csv)
- `pre_approved_config_eligible` — whether this SKU is eligible for a pre-approved configuration catalogue (relevant to auto-approval design)
- `pre_approved_discount_band_pct` — the pre-approved discount for catalogue deals; null if not eligible
- `ssp_published_eur` — the published SSP for ASC 606 allocation; null means SSP has not been established for this SKU
- `lep_eur` — the absolute price floor; no deal may go below this without senior escalation

---

## Things the Data Will Not Tell You Directly

The data has deliberate gaps and imperfections. Some things you will need to probe through clarification sessions or derive through inference:

- **Why specific deals were rejected** beyond the `rejection_reason_code` — approver comments in [`dhi_flags.csv`](dhi_flags.csv) and [`approval_events.csv`](approval_events.csv) give partial clues
- **Historical win rate trends** — the pipeline file is a point-in-time snapshot; it does not show whether win rates are improving or declining
- **The relationship between approval delay and deal loss** — the data shows delays and it shows lost deals, but the causal link must be argued, not read off directly
- **Customer-level context** behind renewal risk signals — the data shows signals, not explanations

---

## Data Quality Notes

The data is intentionally imperfect. Some specific things to be aware of:

- Not all quotes have submission dates — Draft status quotes were never submitted
- `ml_approval_score` is blank for Draft quotes
- `rejection_reason_code` is blank for non-rejected quotes
- Some foreign key relationships involve account names (not IDs) — [`renewal_risk_signals.csv`](renewal_risk_signals.csv) links to [`opportunity_pipeline.csv`](opportunity_pipeline.csv) via `account_name`, which means some names may not match exactly if the renewal account is not in the pipeline snapshot
- A small number of quotes have `days_in_approval` values that appear inconsistent with submission and approval dates — this reflects real-world data quality issues in the source system

---

## Suggested Starting Queries

If you are using Python with pandas:

```python
import pandas as pd

opps   = pd.read_csv('opportunity_pipeline.csv')
quotes = pd.read_csv('quote_header.csv')
bom    = pd.read_csv('bom_line_items.csv')
flags  = pd.read_csv('dhi_flags.csv')
events = pd.read_csv('approval_events.csv')
skus   = pd.read_csv('sku_catalogue.csv')
renew  = pd.read_csv('renewal_risk_signals.csv')
bench  = pd.read_csv('kpi_benchmarks.csv')

# Approval rate by deal motion
quotes.groupby('deal_motion')['quote_status'].value_counts(normalize=True).unstack()

# Average days in approval by rejection reason
quotes[quotes['quote_status']=='Rejected'].groupby('rejection_reason_code')['days_in_approval'].mean()

# Most common DHI flags
flags['flag_code'].value_counts()

# Quotes with open blocker flags at submission
submitted_ids = quotes[quotes['quote_status']=='Submitted']['quote_id']
flags[(flags['quote_id'].isin(submitted_ids)) &
      (flags['severity']=='Blocker') &
      (flags['status']=='Open')]

# Renewal accounts at high risk with no CRE engagement
renew[(renew['renewal_risk_score'] > 0.70) &
      (renew['cre_engagement_started'] == False)]

# BOM lines below LEP
bom[bom['is_lep_compliant'] == False][['quote_id','sku_id','net_price_per_unit_eur','lep_per_unit_eur']]
```

---

*Data package version 1.0 · All data is synthetic · Do not use real-world data in your prototype*
