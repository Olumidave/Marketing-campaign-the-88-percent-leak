# Marketing Campaign Performance & ROI Analysis

A Power BI project analyzing multi-channel marketing campaign performance for a consumer retail company — evaluating channel efficiency, funnel leakage, customer acquisition cost, and repeat customer behavior across 12 campaigns and 12,000 customer interactions.

---

## Business Scenario

A consumer retail company is running multi-channel campaigns (Paid Search, Instagram, Facebook, Google Display, Email) but leadership is unsure whether marketing spend is generating efficient, valuable customers.

## Objective

Evaluate campaign/channel performance, identify funnel leakage, quantify acquisition efficiency, and assess repeat customer behavior.

## Business Problem

Leadership has spent ₦30.68M across 5 channels and 12 campaigns without knowing which channels create profitable, repeat-buying customers — and the portfolio is currently losing 88% of every naira spent (overall ROAS of 0.12), concentrated almost entirely in Acquisition channels, while a small Retention/Email program quietly returns a profit.

---

## Dataset

| Table | Rows | Description |
|---|---|---|
| Campaigns | 12 | One row per campaign — name, channel, type, dates, budget |
| Interactions | 12,000 | One row per ad interaction/touchpoint — funnel events, spend, revenue |
| Customers | 5,526 | One row per unique customer — segment, preferred channel |
| Campaign_Summary | 12 | Pre-aggregated KPIs per campaign, used to validate the DAX model |

**Time period covered:** 5 Jan 2026 – 20 Sep 2026

---

## Tools Used

- **Microsoft SQL Server** — data staging and cleaning
- **Power Query** — transformation and load
- **Power BI / DAX** — data modeling, measures, and dashboard
- **Excel** — source data review

---

## Process

### 1. Data Understanding
Reviewed all four source tables, confirmed column meanings, checked for nulls and duplicates, and validated date ranges and categorical values before any modeling began.

### 2. Data Modeling
Built a star schema:
- **Fact_Interactions** (fact table, grain = 1 row per ad interaction)
- **Dim_Campaigns**, **Dim_Customers**, **Dim_Calendar** (dimension tables)
- Single-direction, many-to-one relationships from the fact table to each dimension
- `Dim_Calendar` built as a dynamic calculated table (`CALENDAR()` bounded by the fact table's min/max dates) and marked as the official Date Table for time intelligence

### 3. DAX Measure Development
Measures were built in dependency order — Core measures first, then ratios built on top of them:
1. **Core measures** — Total Spend, Total Revenue, Total Impressions, Total Clicks, Total Visits, Total Add to Cart, Total Purchases, Total Budget, Total Customers, Total Repeat Purchasers
2. **Funnel rate measures** — CTR, Click-to-Visit Rate, Visit-to-Cart Rate, Cart-to-Purchase Rate, Overall Conversion Rate
3. **Efficiency measures** — CAC, ROAS, ROI, AOV, Net Profit or Loss, Budget Utilization %
4. **Retention measures** — Repeat Purchase Rate, New vs Existing Customer Conversion Rate
5. **Discount measures** — Conversion Rate and AOV split by Discounted vs No Discount
6. **Executive/overview measures** — dynamic verdict text, top/worst campaign by ROAS, best channel, % budget in Acquisition
7. **Time intelligence** — MTD, QTD, YTD, and single-measure MoM/YoY % Change (icon + formatted % + label), plus paired color measures for conditional formatting

### 4. Validation
Every measure was cross-checked against the pre-aggregated `Campaign_Summary` sheet at campaign grain (ROAS, CAC, conversion rate, etc.) to confirm the DAX model reproduced the correct numbers before building any visuals.

### 5. Dashboard Design
Designed 5 dashboard pages, each built around one specific business question rather than a generic KPI dump, with slicers scoped to what's relevant on that page.

---

## Dashboard Pages

### Page 1: Executive Overview
*Answers: Is marketing working overall?*

`[Insert screenshot here]`

KPI cards (Total Spend, Total Revenue, ROAS, ROI, Net Profit/Loss), a dynamic verdict banner, Revenue vs Spend trend, and headline callouts (top campaign, worst zero-revenue campaign, best channel, % budget in Acquisition).

### Page 2: Channel & Campaign Performance
*Answers: Which campaigns/channels are worth funding?*

`[Insert screenshot here]`

Full campaign table ranked by ROAS with CAC, ROI, and a status icon, plus CAC and ROAS comparisons by campaign.

### Page 3: Funnel Analysis
*Answers: Where is funnel leakage happening?*

`[Insert screenshot here]`

Impressions → Clicks → Visits → Cart → Purchase funnel visual, with CTR and conversion breakdowns by channel and device.

### Page 4: Customer & Repeat Behavior
*Answers: Does conversion translate into loyal customers?*

`[Insert screenshot here]`

Repeat Purchase Rate by campaign, and New vs Existing customer conversion comparison.

### Page 5: Discounts & Segment Deep Dive
*Answers: Are discounts helping or hurting, and who converts best?*

`[Insert screenshot here]`

Discounted vs non-discounted conversion rate and AOV, plus conversion rate by region and age group.

---

## Key Findings

- **Overall ROAS is 0.12** — for every ₦1 spent, only ₦0.12 came back in revenue (-88% ROI)
- **Acquisition campaigns received 76% of total budget** but delivered the weakest ROAS (0.03); 4 campaigns generated **zero revenue** despite ₦7.4M combined spend
- **Email/Retention campaigns are the only profitable category** (ROAS 1.61) despite receiving just 5.8% of budget, with CAC 60–70x lower than paid acquisition channels
- **The funnel leak is concentrated at the top** — CTR of 6.5% is the weak point; once a user clicks, the rest of the funnel converts at a healthy rate (81% click-to-visit, 38% cart-to-purchase)
- **Acquisition and Retargeting campaigns produce almost no repeat customers**, while Email/Retention drives 14–22% repeat purchase rates — acquisition spend is buying one-time transactions, not customers
- **Discounts are not eroding value** — discounted interactions convert better (0.71% vs 0.46%) and carry a higher average order value (₦357 vs ₦279) than non-discounted ones

## Recommendation

Reallocate budget away from underperforming paid Acquisition channels (Facebook, Instagram, Google Display) toward Email/Retention, and address the top-of-funnel targeting/creative problem driving low CTR before increasing Acquisition spend further.

---

## Author

**Olumide David Faleye**
Data Analyst / BI Analyst — Aba, Abia State, Nigeria
GitHub: [github.com/Olumidave](https://github.com/Olumidave)
