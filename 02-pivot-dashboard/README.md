# Project 2 — Pivot Tables & Interactive Dashboard

**Skill demonstrated:** Create pivot tables, charts, and dashboards to present key metrics.

---

## Business question
Leadership needs a one-page, at-a-glance view of retail sales performance
by category, location, and payment method — with the ability to understand
revenue trends over time without touching any formulas.

---

## Dataset
This project uses the same Dirty Retail Store Sales dataset from Project 1.
The cleaned output (7,579 rows) is the source for this dashboard, demonstrating
an end-to-end workflow — clean first, then analyse.

> 4,996 rows were excluded during cleaning in Project 1 due to missing values.
> Only complete, validated records are used here.

**Source:** [Kaggle](https://www.kaggle.com/datasets/ahmedmohamed2003/retail-store-sales-dirty-for-data-cleaning)
- 7,579 clean rows (filtered from 12,575 raw)
- License: CC BY-SA 4.0

---

## Fields used in this dashboard

| Field | Type | Role in dashboard |
|-------|------|-------------------|
| Category | Text | Revenue by Category bar chart; slicer filter |
| Location | Text | Online vs In-store pie chart; slicer filter |
| Payment Method | Text | Revenue by Payment Method bar chart; slicer filter |
| Transaction Date | Date | Monthly trend line chart; timeline filter |
| Total Spent | Decimal | Primary measure across all charts and KPI cards |
| Transaction ID | Text | Transaction count in KPI card |

---

## Dashboard

![Dashboard](/screenshots/Retail_Performance_Dashboard.png)

---

## What the dashboard shows

| Element | Type | Insight |
|---------|------|---------|
| Total Revenue | KPI card | $988,513 across all clean transactions |
| Total Transactions | KPI card | 7,579 clean rows |
| Avg Transaction | KPI card | $130.43 per transaction |
| Revenue by Category | Bar chart | Butchers leads at $136k; Milk Products lowest at $117k |
| Revenue by Location | Pie chart | Online 51% ($503k) vs In-store 49% ($486k) |
| Revenue by Payment Method | Bar chart | Cash leads ($344k), Digital Wallet lowest ($321k) |
| Monthly Revenue Trend | Line chart | 37 months Jan 2022 – Jan 2025; peak in Jan 2022 |

---

## Workbook structure

The file `pivot-dashboard.xlsx` has three sheets:

### 1. Data (hidden)
All 7,579 clean rows — the single source of truth for every formula in the
workbook. Hidden so the dashboard is the only thing an end user interacts with.

### 2. PivotTables (hidden)
![Revenue by Category](/screenshots/revenue_by_category.png

![Revenue by Location](/screenshots/evenue_by_location.png

![Monthly Revenue Trend](/screenshots/monthly_revenue_trend.png

![Revenue by Payment Method](/screenshots/revenue_by_payment_method.png

## 3. Charts

### 4. Dashboard
One-page view with KPI cards and 4 charts. Grid lines are hidden to give a
clean, report-ready appearance. Charts pull directly from the Calculations sheet.

---


---

### Charts
Each chart is linked to a named range in the Calculations sheet:

| Chart  
|-------|
| Revenue by Category 
| Revenue by Location  
| Revenue by Payment Method  
| Monthly Revenue Trend 
Charts update automatically when the underlying data changes — no manual
refresh needed.

---

## Key findings

- **Revenue is evenly spread** across all 8 categories — the gap between
  highest (Butchers $136k) and lowest (Milk Products $117k) is only $19k.
  No single category dominates, which suggests a balanced product mix.
- **Online and In-store are nearly equal** at 51% vs 49%. Neither channel
  has a clear advantage — both deserve continued investment.
- **All three payment methods are within $23k of each other** — Cash ($344k),
  Credit Card ($324k), Digital Wallet ($321k). No single method is dominant.
- **Monthly revenue stabilised after Jan 2022** — the opening month was
  unusually high (~$35k). Revenue has since settled in the $22k–$30k band
  consistently through 2023 and 2024, suggesting a maturing, stable business.

---

## Design decisions

| Decision | Reason |
|----------|--------|
| Data and Calculations sheets hidden | End users only need the dashboard — hiding source sheets prevents accidental edits |
| Grid lines removed on Dashboard | Gives a clean, presentation-ready appearance |
| SUMIF over PivotTables | Formulas are auditable and update without a manual refresh |
| Two-layer formula structure | Separates logic (Calculations) from presentation (Dashboard) |
| Footer with data source | Gives credit and shows transparency about where numbers come from |

---

## Files
- `raw-data/retail_store_sales.csv` — source dataset (same as Project 1)
- `pivot-dashboard.xlsx` — workbook with Data, Calculations, and Dashboard sheets
- `screenshots/Dashboard.png` — dashboard view
