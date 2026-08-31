# FP&A Starter Kit Data Model

This starter kit uses a classic star schema: two fact tables in the center and conformed dimensions around them. All keys are integer surrogate keys except `DateKey`, which uses `YYYYMMDD` format.

## Tables

### Fact tables

| Table | Grain | Use |
| --- | --- | --- |
| `FactFinancials` | Monthly financial amount by date, account, product, region, department, and scenario | P&L, variance, revenue, opex, and cash-flow reporting |
| `FactHeadcount` | Monthly workforce metrics by date, department, and scenario | Headcount, hiring, attrition, and personnel-cost reporting |

### Dimensions

| Table | Key | Description |
| --- | --- | --- |
| `DimDate` | `DateKey` | Calendar and fiscal attributes for every day in 2024 |
| `DimAccount` | `AccountKey` | P&L and cash-flow accounts with sign and display order metadata |
| `DimProduct` | `ProductKey` | Six sample SaaS products across Enterprise, SMB, and Consumer segments |
| `DimRegion` | `RegionKey` | Five geographic regions |
| `DimDepartment` | `DepartmentKey` | Eight departments mapped to COGS, R&D, S&M, and G&A functions |
| `DimScenario` | `ScenarioKey` | Actual, Budget, and Forecast flags and ordering |

## Relationships

These relationships are included in `FPA_PowerBI_Starter.SemanticModel/definition/relationships.tmdl`. If you load the CSVs manually, create them in Power BI Desktop Model view.

| From table | From column | To table | To column | Cardinality | Cross-filter direction | Active |
| --- | --- | --- | --- | --- | --- | --- |
| `FactFinancials` | `DateKey` | `DimDate` | `DateKey` | Many-to-One (*:1) | Single | Yes |
| `FactFinancials` | `AccountKey` | `DimAccount` | `AccountKey` | Many-to-One (*:1) | Single | Yes |
| `FactFinancials` | `ProductKey` | `DimProduct` | `ProductKey` | Many-to-One (*:1) | Single | Yes |
| `FactFinancials` | `RegionKey` | `DimRegion` | `RegionKey` | Many-to-One (*:1) | Single | Yes |
| `FactFinancials` | `DepartmentKey` | `DimDepartment` | `DepartmentKey` | Many-to-One (*:1) | Single | Yes |
| `FactFinancials` | `ScenarioKey` | `DimScenario` | `ScenarioKey` | Many-to-One (*:1) | Single | Yes |
| `FactHeadcount` | `DateKey` | `DimDate` | `DateKey` | Many-to-One (*:1) | Single | Yes |
| `FactHeadcount` | `DepartmentKey` | `DimDepartment` | `DepartmentKey` | Many-to-One (*:1) | Single | Yes |
| `FactHeadcount` | `ScenarioKey` | `DimScenario` | `ScenarioKey` | Many-to-One (*:1) | Single | Yes |

## Model View Layout

Place `FactFinancials` and `FactHeadcount` in the center. Arrange shared dimensions around them:

- Top: `DimDate`
- Left: `DimAccount`, `DimProduct`, `DimRegion`
- Right: `DimDepartment`, `DimScenario`

Use single-direction filtering from dimensions to facts. Do not create relationships between dimension tables.

## Sign Convention

`FactFinancials[Amount]` stores revenue and profit lines as positive values and cost/cash-outflow lines as negative values. `DimAccount[Sign]` marks CFO-favorable direction:

- `Sign = 1`: higher is favorable, such as revenue, gross profit, EBITDA, EBIT, net income, operating cash flow, and cash balance.
- `Sign = -1`: lower spend is favorable, such as COGS, opex, D&A, interest, and CapEx.

Use the supplied favorable variance measures so revenue and cost variances are evaluated correctly from an FP&A perspective.

## PBIP Semantic Model

The PBIP project stores the semantic model as TMDL under `FPA_PowerBI_Starter.SemanticModel/definition/`. Set the `RepoRoot` parameter in Power BI Desktop to the absolute path of this repository before refreshing (forward slashes recommended) so each table can import its CSV from `data/`.
