# FP&A Management Report Layout

All pages are designed for a 16:9 canvas at **1280 × 720**. Use the supplied `theme/fpa_theme.json` for colors, fonts, card styling, and matrix defaults.

## Shared Design Standards

- Font: Segoe UI.
- Header color: Navy `#1F3864`.
- Favorable variance: Green `#107C10`.
- Unfavorable variance: Red `#D13438`.
- Warning/neutral: Amber `#F4B942`.
- Money formatting: `$#,##0K` for detail values and `$#,##0.0M` for executive cards.
- Percent formatting: `0.0%;-0.0%`.
- Use `DimDate[MonthShort]` sorted by `DimDate[Month]`.
- Apply conditional formatting using `[Favorable Color]` on variance values where supported.

## Page 1: Executive Summary

**Purpose:** One-page CFO view of performance, variance, and key business drivers.

### Canvas

- Size: 1280 × 720.
- Background: White.
- Top bar: Navy rectangle, height 64.

### Top Bar

- Left: Company logo placeholder, 120 × 40.
- Center-left title: **FP&A Management Report**.
- Right slicers:
  - Period slicer: Month / Quarter / YTD toggle using `DimDate` fields or a disconnected helper table.
  - Scenario slicer: `DimScenario[ScenarioName]`.

### KPI Cards Row

Place five cards across the top below the header:

1. Revenue — `[Actual Revenue]`, variance `[Revenue Variance %]`.
2. Gross Margin % — `[Gross Margin %]`, variance vs `[Budget Gross Margin %]`.
3. EBITDA — `[EBITDA]`, variance vs `[Budget EBITDA]`.
4. EBITDA Margin % — `[EBITDA Margin %]`, variance vs `[Budget EBITDA Margin %]`.
5. Headcount — `[Actual Headcount]`, variance `[Headcount Variance]`.

Use dark card background, white callout text, and red/green variance labels.

### Main Chart

- Visual: Line and clustered column chart.
- Axis: `DimDate[MonthShort]`.
- Columns: `[Actual Revenue]`, `[Budget Revenue]`.
- Lines: `[Forecast Revenue]` and `[Rolling 3M Revenue]`.
- Tooltip: revenue variance $, revenue variance %, EBITDA margin %.

### P&L Summary Matrix

- Rows: `DimAccount[AccountName]`, sorted by `DimAccount[PLOrder]`.
- Columns/values: `[Actual P&L Amount]`, `[Budget P&L Amount]`, `[Variance $ (Favorable)]`, `[Variance % (Favorable)]`.
- Conditional formatting: `[Favorable Color]` on variance columns.

### Variance Bridge Waterfall

- Visual: Waterfall.
- Categories: Revenue, COGS, Gross Profit, R&D, Sales & Marketing, G&A, EBITDA.
- Values: favorable variance measure or a small bridge helper table if preferred.
- Increase color: Green; decrease color: Red; totals: Navy.

### Insights Text Box

Add a text box titled **Management Commentary** with placeholders:

- Revenue performance vs plan.
- Gross margin drivers.
- Opex and hiring actions.
- Cash-flow risks/opportunities.

## Page 2: P&L Statement

**Purpose:** Full income-statement review with budget, forecast, and year-over-year context.

- Main visual: Matrix spanning most of the page.
- Rows: Revenue → COGS → Gross Profit → Opex by department/function → EBITDA → D&A → EBIT → Interest → Net Income.
- Values: Actual, Budget, Forecast, Var $ vs Bud, Var % vs Bud, PY Actual, YoY $.
- Slicers: `DimDate[MonthName]`, `DimDate[Year]`.
- Optional sparkline: monthly trend for `[Actual P&L Amount]`; if unavailable, use a small line chart beside the matrix.
- Conditional formatting: Green/red on variance columns using favorable variance logic.

## Page 3: Revenue Analysis

**Purpose:** Explain revenue mix, growth, and variance by commercial dimensions.

- Revenue by Product: clustered bar chart using `DimProduct[ProductName]` with `[Actual Revenue]` and `[Budget Revenue]`.
- Revenue by Region: donut or map using `DimRegion[RegionName]` and `[Actual Revenue]`.
- Revenue by Segment: stacked bar using `DimProduct[Segment]`, `DimDate[MonthShort]`, and `[Actual Revenue]`.
- Revenue Growth Waterfall: prior-year revenue to current-year revenue by product using `[Revenue Growth YoY $]` when prior-year data is added.
- Month-over-month trend: line chart with `[Actual Revenue]`, `[MoM Revenue %]`, and `[Rolling 3M Revenue]`.
- Slicers: Region, Product, Segment.

## Page 4: Opex Analysis

**Purpose:** Monitor spending discipline and budget utilization.

- Opex by Department: clustered bar with `[Actual Opex]`, `[Budget Opex]`, and `[Forecast Opex]` by `DimDepartment[DepartmentName]`.
- Opex trend: line chart by month.
- Opex as % of Revenue: line chart using `DIVIDE([Actual Opex], [Actual Revenue])`.
- Opex variance matrix: rows = department, columns = month, values = `[Variance $ (Favorable)]`.
- Budget utilization: gauge or stacked progress bar using actual opex divided by budget opex.
- Slicers: Department, Function, Month.

## Page 5: Cash Flow

**Purpose:** Show cash generation, investment, and runway.

- Cash balance waterfall: opening cash → operating cash flow → investing/capex → financing placeholder → closing cash.
- Free Cash Flow trend: line chart with `[Free Cash Flow]` by month.
- FCF margin overlay: line and clustered column chart with `[Actual Revenue]` columns and `[FCF Margin %]` line.
- KPI cards: `[Cash Burn Rate]` and `[Months of Runway]`.
- Operating vs CapEx: stacked bar with `[Operating Cash Flow]` and `[CapEx]` by month.
- Slicers: Month, Quarter.

## Page 6: Workforce

**Purpose:** Connect headcount, hiring, attrition, and personnel cost to financial performance.

- Headcount trend: line chart using `[Actual Headcount]` and `[Budget Headcount]` by month.
- Hires vs Attritions: clustered column chart by month.
- Headcount by Department: donut chart by `DimDepartment[DepartmentName]`.
- Attrition Rate % trend: line chart with `[Attrition Rate %]`.
- Personnel Cost variance: bar chart with `[Actual Personnel Cost]` and `[Budget Personnel Cost]`.
- Cost per Head trend: line chart with `[Cost per Head]`.
- KPI cards: Total HC, Net Hires, Attrition Rate, Personnel Cost.
- Slicers: Department, Month.
