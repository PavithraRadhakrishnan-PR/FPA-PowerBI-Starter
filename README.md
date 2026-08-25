# FP&A Power BI Starter Kit

A self-contained Power BI starter kit for building a CFO-quality FP&A management report. It includes realistic SaaS sample data, a star-schema model, reusable DAX measures, a Power BI theme, and detailed report layout guidance.

## 1. Overview

This repository is for finance analysts, FP&A teams, startup finance leaders, and Power BI builders who need a fast starting point for management reporting. The sample data represents a 2024 SaaS/technology company with Actuals from January through September and full-year Budget and Forecast scenarios.

All monetary values are in **USD thousands**. The fiscal year is **January–December 2024**.

## 2. Repository Structure

```text
/
├── README.md
├── data/
│   ├── FactFinancials.csv       # Financial actual, budget, and forecast fact data
│   ├── FactHeadcount.csv        # Headcount, hiring, attrition, and personnel-cost fact data
│   ├── DimDate.csv              # Daily date dimension for Jan 1-Dec 31 2024
│   ├── DimAccount.csv           # P&L and cash-flow account dimension
│   ├── DimProduct.csv           # Product and segment dimension
│   ├── DimRegion.csv            # Region dimension
│   ├── DimDepartment.csv        # Department/function dimension
│   └── DimScenario.csv          # Actual, Budget, Forecast scenario dimension
├── dax/
│   └── fpa_measures.dax         # Reusable DAX measures
├── theme/
│   └── fpa_theme.json           # Power BI theme file
└── docs/
    ├── report_layout.md         # Six-page report build specification
    └── data_model.md            # Star-schema relationships and model notes
```

## 3. Quick Start

1. Open **Power BI Desktop**.
2. Select **Get Data → Text/CSV** and load every CSV from the `data/` folder.
3. In **Model view**, create the relationships listed in `docs/data_model.md`.
4. Apply the theme via **View → Browse for themes** and choose `theme/fpa_theme.json`.
5. Create a blank table named `Measures` and add the formulas from `dax/fpa_measures.dax`.
6. Build the six pages using `docs/report_layout.md` as the blueprint.
7. Format money as `$#,##0K` for detailed visuals or `$#,##0.0M` for executive cards.

A finance analyst with basic Power BI knowledge can complete the initial build in about 15 minutes.

## 4. Step-by-Step Setup

### Load CSVs into Power BI Desktop

1. Open Power BI Desktop.
2. Choose **Home → Get Data → Text/CSV**.
3. Select one CSV from `data/`.
4. Confirm delimiter is comma and data types look correct.
5. Click **Load**.
6. Repeat for all files in `data/`.

Recommended data types:

| Column pattern | Data type |
| --- | --- |
| `DateKey`, `AccountKey`, `ProductKey`, `RegionKey`, `DepartmentKey`, `ScenarioKey` | Whole number |
| `Date` | Date |
| `Amount`, `PersonnelCost` | Decimal number |
| `Headcount`, `Hires`, `Attritions`, flags | Whole number |
| Names/codes/categories | Text |

### Set Up Relationships

In **Model view**, create these relationships, all **Many-to-One (*:1)** and **Single** direction:

- `FactFinancials[DateKey]` → `DimDate[DateKey]`
- `FactFinancials[AccountKey]` → `DimAccount[AccountKey]`
- `FactFinancials[ProductKey]` → `DimProduct[ProductKey]`
- `FactFinancials[RegionKey]` → `DimRegion[RegionKey]`
- `FactFinancials[DepartmentKey]` → `DimDepartment[DepartmentKey]`
- `FactFinancials[ScenarioKey]` → `DimScenario[ScenarioKey]`
- `FactHeadcount[DateKey]` → `DimDate[DateKey]`
- `FactHeadcount[DepartmentKey]` → `DimDepartment[DepartmentKey]`
- `FactHeadcount[ScenarioKey]` → `DimScenario[ScenarioKey]`

See `docs/data_model.md` for the full relationship map.

### Apply the Theme

1. Go to **View**.
2. Select the theme dropdown.
3. Choose **Browse for themes**.
4. Select `theme/fpa_theme.json`.
5. Confirm the theme is applied without errors.

### Copy DAX Measures

1. In Power BI Desktop, select **Home → Enter data**.
2. Create a one-column blank table named `Measures`.
3. Right-click the `Measures` table and choose **New measure**.
4. Copy each measure from `dax/fpa_measures.dax` one at a time.
5. Set formatting:
   - Revenue, cost, profit, cash, and personnel cost: `$#,##0K` or `$#,##0.0M`.
   - Margins, variance %, attrition rate: `0.0%`.
   - Headcount, hires, attritions: whole number.

### Build Report Pages

Use standard Power BI visuals only: Cards, Matrix, Line and Clustered Column Chart, Bar Chart, Waterfall, Donut/Map, Gauge, and Slicers. Follow the exact six-page layout in `docs/report_layout.md`:

1. Executive Summary
2. P&L Statement
3. Revenue Analysis
4. Opex Analysis
5. Cash Flow
6. Workforce

## 5. DAX Measures Reference

| Measure | Formula summary | Description |
| --- | --- | --- |
| Total Amount | `SUM(FactFinancials[Amount])` | Base financial amount in USD thousands. |
| Signed P&L Amount | `SUMX(... Amount * RELATED(Sign))` | Presents expense rows as positive values for matrix reporting. |
| Actual Revenue | Actual scenario + Revenue accounts | Actual revenue for Jan-Sep sample data. |
| Budget Revenue | Budget scenario + Revenue accounts | Full-year budget revenue. |
| Forecast Revenue | Forecast scenario + Revenue accounts | Full-year forecast revenue. |
| Revenue Variance $ | Actual Revenue - Budget Revenue | Dollar variance to budget. |
| Revenue Variance % | Revenue Variance $ / ABS(Budget Revenue) | Percent variance to budget. |
| PY Actual Revenue | SAMEPERIODLASTYEAR Actual Revenue | Prior-year comparison hook for real data replacement. |
| Revenue Growth YoY $ | Actual Revenue - PY Actual Revenue | Year-over-year growth dollars. |
| Revenue Growth YoY % | Growth $ / ABS(PY Actual Revenue) | Year-over-year growth percent. |
| Actual COGS / Budget COGS / Forecast COGS | Scenario + COGS accounts | COGS shown as positive spend. |
| Gross Profit | Actual Revenue - Actual COGS | Gross profit. |
| Budget Gross Profit / Forecast Gross Profit | Revenue - COGS by scenario | Scenario gross profit. |
| Gross Margin % | Gross Profit / Actual Revenue | Gross margin rate. |
| Actual Opex / Budget Opex / Forecast Opex | Scenario + opex excluding D&A | Operating expense before D&A. |
| DA / Budget DA / Forecast DA | D&A account by scenario | Depreciation and amortization. |
| EBITDA / Budget EBITDA / Forecast EBITDA | Gross Profit - Opex | EBITDA by scenario. |
| EBITDA Margin % | EBITDA / Actual Revenue | EBITDA margin. |
| EBIT / Budget EBIT / Forecast EBIT | EBITDA - D&A | EBIT by scenario. |
| Interest Expense | Interest account | Interest cost shown as positive spend. |
| Net Income / Budget Net Income / Forecast Net Income | Net income account by scenario | Bottom-line income. |
| Actual P&L Amount / Budget P&L Amount / Forecast P&L Amount | Signed P&L Amount by scenario | Matrix-ready values by account line. |
| Variance $ | Actual P&L Amount - Budget P&L Amount | Raw variance. |
| Variance $ (Favorable) | Variance $ × account sign | CFO-favorable variance. |
| Variance % (Favorable) | Favorable variance / ABS(Budget P&L Amount) | CFO-favorable variance percent. |
| Is Favorable | Favorable vs unfavorable label | Conditional formatting helper. |
| Favorable Color | Green/red hex result | Conditional formatting color measure. |
| Actual Headcount / Budget Headcount / Forecast Headcount | Latest headcount by scenario | Workforce snapshot. |
| Headcount Variance | Actual - Budget | Headcount variance. |
| Total Hires / Total Attritions | Actual scenario sums | Hiring and attrition counts. |
| Net Hires | Hires - Attritions | Net workforce additions. |
| Attrition Rate % | Attritions / Actual Headcount | Attrition rate. |
| Actual Personnel Cost / Budget Personnel Cost | Personnel cost by scenario | Workforce cost. |
| Personnel Cost Variance $ | Actual - Budget | Personnel cost variance. |
| Cost per Head | Personnel cost / headcount | Productivity/cost metric. |
| Operating Cash Flow | Actual operating cash flow account | Operating cash generation. |
| CapEx | Actual capital expenditures | CapEx shown as positive spend. |
| Free Cash Flow | Operating Cash Flow - CapEx | FCF. |
| FCF Margin % | Free Cash Flow / Actual Revenue | FCF margin. |
| Cash Balance | Actual cash balance account | Cash balance. |
| Cash Burn Rate | Average negative FCF, floored at zero | Burn-rate KPI. |
| Months of Runway | Cash Balance / Cash Burn Rate | Runway when burn is positive. |
| YTD Actual Revenue / YTD Budget Revenue / YTD Forecast Revenue | DATESYTD revenue | Year-to-date revenue views. |
| Rolling 3M Revenue | DATESINPERIOD last 3 months | Rolling trend measure. |
| MoM Revenue $ / MoM Revenue % | Month-over-month revenue change | Revenue momentum. |

## 6. Replacing Sample Data

To swap in real company data, keep the same table and column names so measures and relationships continue to work.

1. Replace rows in the CSVs, not the column headers.
2. Keep all key columns numeric and unique in dimension tables.
3. Maintain `DimAccount[Sign]`:
   - `1` for revenue/profit/cash-inflow lines where higher is favorable.
   - `-1` for expense/cash-outflow lines where lower is favorable.
4. Add new products, regions, departments, or accounts to dimensions first, then reference their keys in facts.
5. Refresh Power BI and verify no relationship errors appear.
6. Validate totals against the source ERP, planning system, or spreadsheet.

## 7. FP&A Variance Logic

This model uses CFO-style favorable variance logic:

- **Revenue favorable:** Actual > Budget.
- **Cost favorable:** Actual < Budget.
- **Profit favorable:** Actual > Budget.
- **Cash outflow favorable:** Actual outflow < Budget outflow.

The measure `[Variance $ (Favorable)]` multiplies the raw variance by `DimAccount[Sign]`. This lets one red/green rule work across revenue and cost lines.

Example:

| Line | Actual | Budget | Raw variance | Favorable variance | Result |
| --- | ---: | ---: | ---: | ---: | --- |
| Revenue | 10,000 | 9,500 | 500 | 500 | Favorable |
| Opex | 4,000 | 4,300 | -300 | 300 | Favorable |
| COGS | 3,700 | 3,500 | 200 | -200 | Unfavorable |

## 8. Customization Guide

### Add accounts

1. Add a row to `DimAccount.csv` with a unique `AccountKey` and `AccountCode`.
2. Set `AccountCategory` to Revenue, COGS, Opex, or Other.
3. Set `PLOrder` to control matrix order.
4. Set `Sign` for favorable variance behavior.
5. Add corresponding fact rows to `FactFinancials.csv`.

### Add regions

1. Add a row to `DimRegion.csv`.
2. Use the new `RegionKey` in `FactFinancials.csv`.
3. Refresh the model and check revenue maps/donuts.

### Add products

1. Add a row to `DimProduct.csv`.
2. Assign `ProductLine` and `Segment`.
3. Use the new `ProductKey` in `FactFinancials.csv`.
4. Refresh revenue visuals.

### Add departments

1. Add a row to `DimDepartment.csv`.
2. Assign the correct `Function` value: COGS, R&D, S&M, or G&A.
3. Use the new `DepartmentKey` in both fact tables where relevant.

## 9. Troubleshooting

| Issue | Likely cause | Fix |
| --- | --- | --- |
| Blank visuals | Missing or inactive relationships | Recreate the relationships in `docs/data_model.md`. |
| DAX errors when pasting | Measures pasted as a block into one measure | Create each measure individually. |
| Months sort alphabetically | `MonthShort` not sorted | Sort `DimDate[MonthShort]` by `DimDate[Month]`. |
| Variance colors look reversed | Using raw variance instead of favorable variance | Use `[Variance $ (Favorable)]`, `[Variance % (Favorable)]`, and `[Favorable Color]`. |
| Totals double count in ad hoc visuals | Detail and subtotal account lines are both selected | Filter by `DimAccount[AccountCategory]` or use the supplied P&L measures. |
| Relationship error on DateKey | `DateKey` loaded as text in one table and whole number in another | Set all `DateKey` columns to Whole number. |
| Theme import error | JSON edited manually and made invalid | Validate `theme/fpa_theme.json` with a JSON validator. |

## 10. License & Contributing

This starter kit is provided as a practical template for FP&A reporting. Adapt it to your organization, chart of accounts, planning process, and reporting calendar.

Contributions are welcome. Keep changes focused, preserve the star schema, and document any new data fields or measures so analysts can continue to use the kit without extra engineering support.
