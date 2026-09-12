# FMCG Sales and Customer Analytics

## Overview

Compare category performance, sales periods, regional contribution and customer engagement.

**Intended audience:** Sales leaders, category managers and customer-analysis teams.

**Scope:** Sales and customer analysis; category → subcategory → product drill-down; period-selection reporting; customer quantity and sales ranking.

## Evidence and implementation status

This documentation was derived from the supplied PBIX archive: report layout, visual query bindings, bookmark configuration, model diagram metadata and archive contents. It is more detailed than a screenshot review, but it is not a runtime test in Power BI Desktop.

| Item | Status |
|---|---|
| Report pages, visual types and referenced fields | Inspected directly |
| Model table names | Inspected in DiagramLayout |
| Bookmarks | Inspected in report configuration |
| DAX source expressions and complete measure catalog | Not decoded from the binary DataModel |
| Power Query M, connectors and refresh credentials | Not independently extracted |
| Relationship cardinality, direction and active status | Require model export or Desktop inspection |
| RLS roles, expressions and effective access | Require role export and tests |
| Power BI Service deployment / refresh history | Not established by this PBIX review |
| Numerical results and performance timings | Not benchmarked |

## Report pages

| Page | Visual containers, including decoration | Visual inventory |
|---|---:|---|
| Home | 9 | image (4), textbox (5) |
| Info | 2 | textbox (2) |
| Sales Analysis | 19 | image (3), textbox (2), card (7), pivotTable (1), slicer (3), kpi (1), lineStackedColumnComboChart (1), donutChart (1) |
| Customer Analysis | 24 | image (3), textbox (1), card (7), pivotTable (1), slicer (1), tableEx (4), actionButton (4), donutChart (1), pieChart (1), columnChart (1) |
| Qty Tooltip | 2 | lineChart (1), card (1) |
| Sales Tooltip | 2 | lineChart (1), card (1) |

## Analytical visual specification

The bindings below are exact references used by report visuals. They identify the reporting surface and should be reconciled with the exported semantic model. A reference confirms use in a visual, not the underlying formula or its correctness.

### Home

Navigation, explanatory text and decorative report elements; no analytical field bindings in the inspected page.

### Info

Navigation, explanatory text and decorative report elements; no analytical field bindings in the inspected page.

### Sales Analysis

| Visual | Bound fields / measures |
|---|---|
| card | Values: `Keymeasures.Total Sales` |
| card | Values: `Keymeasures.Total Order` |
| card | Values: `Keymeasures.Total Quantity Purchased` |
| card | Values: `Keymeasures.Average Order Value` |
| card | Values: `Keymeasures.Last 3 months sales` |
| pivotTable | Rows: `Products.Category`, `Products.SubCategory`, `Products.ProductName`; Values: `Keymeasures.Total Sales`, `Keymeasures.All Selected Sales %`, `Keymeasures.Sales QTD`, `Keymeasures.Sales MTD` |
| slicer | Values: `Products.Category` |
| slicer | Values: `Date.Quarter` |
| kpi | Indicator: `Keymeasures.Sales QTD`; Goal: `Keymeasures.Previous QTD`; TrendLine: `Date.Quarter` |
| lineStackedColumnComboChart | Category: `Date.Month`; Y: `Keymeasures.Total Sales`; Y2: `Keymeasures.Total Orders` |
| donutChart | Category: `Customers.Region`; Y: `Keymeasures.Period Sales` |
| card | Values: `Keymeasures.Period Sales` |
| slicer | Values: `Period Selection.Period` |
| card | Values: `Keymeasures.Donut Title` |

### Customer Analysis

| Visual | Bound fields / measures |
|---|---|
| card | Values: `Keymeasures.Total Sales` |
| card | Values: `Keymeasures.Total Customers` |
| card | Values: `Keymeasures.Active Customers` |
| card | Values: `Keymeasures.Loyalty` |
| card | Values: `Keymeasures.Basket Diversity` |
| pivotTable | Rows: `Products.Category`, `Products.SubCategory`, `Products.ProductName`; Values: `Keymeasures.Total Orders`, `Keymeasures.Orders MTD`, `Keymeasures.Orders QTD` |
| slicer | Values: `Products.Category` |
| card | Values: `Keymeasures.Unique Region` |
| tableEx | Values: `Customers.CustomerName`, `Keymeasures.Quantity` |
| tableEx | Values: `Customers.CustomerName`, `Keymeasures.Total Sales` |
| donutChart | Category: `Customers.Region`; Y: `Keymeasures.Total Customers` |
| card | Values: `Keymeasures.Total Customers` |
| pieChart | Category: `Customers.LoyaltyStatus`; Y: `Keymeasures.Total Customers` |
| columnChart | Category: `Customers.Region`; Series: `Customers.LoyaltyStatus`; Y: `Keymeasures.Total Sales` |
| tableEx | Values: `Customers.CustomerName`, `Keymeasures.Quantity` |
| tableEx | Values: `Customers.CustomerName`, `Keymeasures.Total Sales` |

### Qty Tooltip

| Visual | Bound fields / measures |
|---|---|
| lineChart | Category: `Date.Quarter`; Y: `Keymeasures.Total Quantity Purchased` |
| card | Values: `Keymeasures.Quantity tooltip title` |

### Sales Tooltip

| Visual | Bound fields / measures |
|---|---|
| lineChart | Category: `Date.Quarter`; Y: `Keymeasures.Total Sales` |
| card | Values: `Keymeasures.Sales Tooltip` |

## Semantic model inventory

These table names are present in the model diagram. Their names suggest intended responsibilities; diagram positions do not establish relationships, grain or a validated star/snowflake schema.

- `Bridge Table`
- `CurrencyRates`
- `Customers`
- `Date`
- `Dynamic users Table`
- `Keymeasures`
- `Period Selection`
- `Products`
- `Sales`
- `Static users table`

### Referenced measures

This is the report-referenced measure inventory, not a claim that all hidden or unused model measures have been enumerated. Exact DAX should be exported alongside this README.

**Keymeasures**

- `Active Customers`
- `Average Order Value`
- `Basket Diversity`
- `Donut Title`
- `Last 3 months sales`
- `Loyalty`
- `Orders MTD`
- `Orders QTD`
- `Period Sales`
- `Previous QTD`
- `Quantity`
- `Sales %`
- `Sales MTD`
- `Sales QTD`
- `Total Customers`
- `Total Orders`
- `Total Quantity Purchased`
- `Total Sales`
- `Unique Region`

### Referenced business fields

- **Customers**: `CustomerName`, `LoyaltyStatus`, `Region`
- **Date**: `Month`, `Quarter`
- **Period Selection**: `Period`
- **Products**: `Category`, `ProductName`, `SubCategory`

## Interactions and navigation

Report bookmarks: `BTM 5 Left`, `BTM 5 Right`, `Top 5 Left`, `Top 5 Right`.

Use the visual inventory to identify slicers, matrices and detail pages. Test cross-filtering, reset behavior, hover tooltips and back navigation in Desktop. A page named Tooltip or Details alone does not prove every visual is wired to it. Validate each source visual and destination together.

## Calculation and data-quality review

- Reconcile `Total Order` and `Total Orders`: both names appear in visual bindings; determine whether they intentionally differ.
- Define the denominator for `All Selected Sales %` and test category selections versus grand totals.
- Document what counts as an active customer, loyal customer and basket diversity; names alone do not define these calculations.
- Validate current and previous QTD against a marked, continuous date table and a documented as-of date.
- Confirm how CurrencyRates is used: the Home page labels financial values as USD, but conversion logic was not decoded.
- Document static/dynamic user tables and Bridge Table relationships before claiming tested RLS.
- Review the Info page: DirectQuery, SQL Server, incremental refresh and Premium caching are narrative claims, not verified deployment evidence.

### Required calculation documentation

For each exported measure, record its DAX, business definition, numerator/denominator, filter behavior, date logic, blank/zero handling and one manually checked test case. Preserve exact table/column references; do not substitute generic sample DAX for the actual implementation.

### ETL documentation

Export actual Power Query M and describe source location, table grain, type conversion, null handling, deduplication keys, merge/append logic, unmatched joins and currency/date normalization. Include before/after row counts for transformations. SQL scripts belong here only if the project actually uses SQL.

## Security and deployment

The archive contains SecurityBindings metadata; this does not establish that business RLS is configured or working. Document actual role names, filter expressions, user mapping and positive/negative access tests before advertising static or dynamic RLS. Hidden pages, slicers and bookmarks are presentation controls, not access controls.

For a Service deployment, document workspace access, semantic-model permissions, role assignments, source/gateway configuration, refresh schedule and successful refresh evidence. Remove private identifiers from screenshots. These are completion requirements, not claims of an already verified deployment.

## Performance review

| Largest archive entry | Uncompressed bytes |
|---|---:|
| `Report/StaticResources/RegisteredResources/pastel-blue-vignette-concrete-8345733478153178.jpeg` | 18,130,633 |
| `Report/Layout` | 483,040 |
| `DataModel` | 117,527 |

These are archive payload sizes, not query timings. Optimize large decorative assets separately from model/query work. Benchmark cold and warm report interactions with a consistent dataset and filter state before claiming faster performance. Record page-load/visual durations, expensive measures and refresh duration, then compare before/after results.

## Open and reproduce

1. Download `pbix/FMCG_Sales_Analysis_dashboard.pbix` once uploaded by the repository owner.
2. Open the file in a compatible Power BI Desktop installation.
3. Review cached report pages before attempting a refresh.
4. In source settings, point queries to your authorized dataset and supply credentials locally.
5. Export/review the model and transformations; confirm keys, grain and relationship paths.
6. Refresh and reconcile headline KPIs with independently calculated source totals.
7. Run interaction, date-boundary, ranking and security tests before sharing.

The original source datasets were not included as separate files in the supplied ZIP. Reproducing refresh requires those sources or a documented compatible sample dataset. Do not infer synthetic/public provenance solely from portfolio use.

## Recommended repository files

- `pbix/FMCG_Sales_Analysis_dashboard.pbix` — reviewed report file.
- `images/` — screenshots of the main pages, tooltips and model view.
- `data/` — authorized sample data, provenance and data dictionary.
- `model/` — exported model metadata, relationship definitions and roles.
- `dax/measures.md` — actual measure expressions and definitions.
- `power-query/transformations.md` — actual M and ETL notes.
- `sql/` — actual scripts, if applicable.
- `docs/business-requirements.md` — stakeholders, decisions, scope and acceptance criteria.
- `docs/validation.md` — reconciliation, boundary tests and expected results.
- `docs/insights.md` — quantified findings with date/filter context.
- `security/rls-tests.md` — actual roles and access tests, if implemented.

### Screenshot setup

Export each main page into `images/` using lowercase hyphenated filenames. Add a Markdown image only after its file exists, for example:

```markdown
![Report overview](images/overview.png)
```

Do not use decorative PBIX resources as substitutes for screenshots of the functioning report. Do not publish customer identifiers in screenshot examples unless their provenance permits it.

## Acceptance checklist

- [ ] Source totals match dashboard totals under no filters and selected filters.
- [ ] Key uniqueness, orphan records and many-to-many behavior are checked.
- [ ] Period comparisons use a consistent date basis.
- [ ] Empty selections, zero denominators and missing dates behave intentionally.
- [ ] Rankings handle ties and preserve intended grouping.
- [ ] Tooltips and detail pages receive the expected context.
- [ ] User permissions are tested independently of report filtering.
- [ ] Screenshots, model exports and source definitions match the published PBIX.
- [ ] Performance claims include measured before/after timings.

## Insights, contribution and reuse

Add findings only after validating numerical results. For each insight record the metric, time range, filters, comparison, business implication and proposed action. No measured business uplift is asserted by this README.

This project was completed within a shared mentoring curriculum. Each candidate should name the mentor with permission, describe the tasks they personally completed, and distinguish baseline work from their own enhancements. Reusing the documentation does not justify claiming identical employment experience or independent authorship of every component.

## Data provenance and license

Document dataset ownership, origin and reuse permissions before distributing embedded data. A public repository does not itself grant a license to the dataset or third-party assets. Add an appropriate license only after the owner confirms rights.

## Review record

- Source archive: `FMCG Sales Analysis dashboard.pbix`
- SHA-256: `782d06fc91f8b3573ed141628ed3c02270f38958a96e9ccc1fb8ca015330e3e2`
- Review date: 12 September 2026
- Method: static PBIX report/model-layout inspection; no Desktop calculation execution.
