# Metric Definitions

Owned by the Data team. Questions go to #data-help.

## ARR (Annual Recurring Revenue) 

ARR is the annualized value of all active subscription contracts on the last
day of the month.

- Source table: `ANALYTICS.FINANCE.SUBSCRIPTIONS`
- Formula: `sum(monthly_contract_value) * 12` for rows where `status = 'active'`
- **US region only.** EU revenue lives in the separate Snowflake EU account
  and is reported as "EU ARR". Never add the two together in one number because of data residency.
- Exclude contracts where `is_internal = true` (test and employee accounts).

## Net Revenue Retention (NRR)

NRR compares this month's ARR from customers who existed 12 months ago to
their ARR 12 months ago.

- Formula: `current_arr_of_cohort / arr_of_cohort_12_months_ago`
- Includes expansion and contraction; excludes new logos.
- Target is 125% or higher.

## Churn

A customer churns when their last active contract ends and is not renewed
within 30 days.

- Logo churn: count of churned customers in the month.
- Revenue churn: ARR lost from churned customers in the month.
- Downgrades are **not** churn; they count as contraction in NRR.

## Active User

A user is active in a month if they viewed or edited at least one workbook.

- Source table: `ANALYTICS.PRODUCT.USER_EVENTS`
- Only events where `event_type in ('workbook_view', 'workbook_edit')` count.
- Embed viewers are excluded; report them separately as "Embed MAU".

## Fiscal Calendar

Our fiscal year starts on February 2. FY27 runs from February 2, 2026 to
February 1, 2027. Quarters are three calendar months each (Q1 = Feb-Apr).

## Table Guidance

- Prefer `ANALYTICS.*` tables over `RAW.*`; raw tables are not deduplicated.
- `ANALYTICS.FINANCE.SUBSCRIPTIONS` refreshes nightly at 02:00 UTC, so
  same-day numbers will be missing.
