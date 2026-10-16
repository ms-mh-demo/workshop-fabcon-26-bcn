# sales Report

## Overview

The sales report presents Northwind Retail Group sales analysis, followed by a category-by-year matrix of revenue, margin, and cost. It supports comparisons across products, brands, customer gender, stores, and time.

| Property | Value |
|---|---|
| Source | [lab2/resources/sales.Report](../lab2/resources/sales.Report/) |
| Semantic model | [sales](sales.semantic-model.md), resolved locally through `../sales.SemanticModel` |
| Entry point | [sales.pbip](../lab2/resources/sales.pbip) |
| Pages | 2: Sales, KPI |
| Visuals | 9 total: 7 on Sales, 2 on KPI |
| Canvas | Both pages 1280 x 720, FitToPage |
| Landing / saved active page | Sales |
| Base theme | Fluent2-CY26SU08 |
| Custom theme | [CopilotDefault.json](../lab2/resources/sales.Report/StaticResources/RegisteredResources/CopilotDefault.json) |

This guide describes saved report metadata, not a verified live rendering. The model is Import mode; the local binding does not identify a published workspace or service semantic model.

## Report Flow

1. Start on **Sales**, headed "Northwind Retail Group - Sales Analysis." Its saved layout places the main cards and date slicer above product/brand/store breakdowns, with the time trend below.
2. Use date selections and page filters to establish the analysis context. Drill down through product categories or brands for more detailed comparisons.
3. Switch to **KPI**, headed "Northwind Retail Group - KPI," for the matrix of Sales Amount, Margin, and Cost by product category and calendar year.

Page order and landing behavior come from [pages.json](../lab2/resources/sales.Report/definition/pages/pages.json). No custom page-navigation buttons or report bookmarks were found. Normal page tabs provide the page-to-page route.

## Pages

### Sales

Source: [Sales page definition](../lab2/resources/sales.Report/definition/pages/ReportSection89a9619c7025093ade1c/page.json).

**Screenshot limitation:** No current-project screenshot was captured. The Desktop preview availability probe reported `available: false` because `bridge.manifest probe timed out after 3000ms`. Visual appearance, actual slicer control style, populated values, and runtime interactions remain unverified. Workshop illustrations are not substituted for current-page evidence.

#### Main Metrics

- **Sales Amount:** Quantity times net price, summed over filtered order lines. Appears in the cards, breakdown charts, and time trend.
- **Sales Amount Avg per Day:** Average daily Sales Amount over nonblank daily results in the date context; not necessarily total divided by every calendar day.
- **# Customers (w/ Sales):** Distinct customer keys in filtered sales rows. Appears in the cards and scatter-chart bubble size.
- **Margin:** Absolute quantity-weighted net price less unit cost; a currency amount, not a percentage.
- **Sales Amount (LY):** Prior-year sales shown only when current-context Sales Amount is positive.
- **Moving average:** Area-chart visual calculation `MOVINGAVERAGE([Sales Amount], 12)`. It operates over twelve visual-axis positions; with Year-Month on the axis, it represents a visual monthly-position moving average, not the semantic model's trailing daily-average measures. Missing axis positions and interaction behavior were not evaluated.

Sales Amount and Margin use dollar formats, while Sales Amount (LY) uses a euro format. This is a formatting mismatch, not implemented currency conversion. See the [measure definitions and caveats](sales.semantic-model.md#measures).

#### Analysis Breakdowns

| Visual ID | Type | Fields / measures | Analysis behavior encoded in source |
|---|---|---|---|
| `59c8c6375c6a2e3c31b0` | Card visual | Sales Amount; Sales Amount Avg per Day; # Customers (w/ Sales); Margin | Four summary metrics in one visual; visual title disabled |
| `8b8727ff328bdc49692c` | Slicer | Calendar.Date | Date selection, sorted ascending; no explicit saved selection predicate or control mode |
| `5acb1caf298449a8acb4` | Clustered bar chart | Product.Category > Subcategory > Product; Sales Amount | Category active initially; additional product levels available; sorted by Sales Amount descending |
| `32b08d7f32cb5f9816cc` | Bar chart | Product.Brand > Product.Product; Customer.Gender series; Sales Amount | Brand active initially; compare customer-gender series and drill to products; sorted by Sales Amount descending |
| `4b18758374093c2e23fb` | Scatter chart | Store.Store series; Calendar.Month Name X; Sales Amount Y; # Customers (w/ Sales) size | Saved field roles are as listed; title says "Sales Amount vs # Customers," but customer count is size, not X |
| `484fbdd73143c5bf71fa` | Area chart | Calendar.Year-Month; Sales Amount; Sales Amount (LY); Moving average | Time comparison sorted by Year-Month ascending |
| `eb5c360e357e8b54eb88` | Textbox | Northwind Retail Group - Sales Analysis | Static page heading |

The scatter chart's Month Name X binding is categorical even though it is a scatter visual. Do not interpret it as a verified numeric customer-count/revenue correlation plot; rendering needs to be checked in Desktop.

Some query reference labels retain older strings such as `Measure Table.Sales Amount` or `Sales.# Customers (with Sales)`. The actual field expressions bind to Sales.Sales Amount and Sales.# Customers (w/ Sales); those expressions, rather than the legacy labels, determine the documentation above.

#### Available Filters

| Scope | Filter or slicer | Behavior / default |
|---|---|---|
| On-canvas slicer | Calendar.Date | Restricts the date context; no saved range/selection predicate found |
| Page | Calendar.Year | Categorical filter configured to require single selection; no explicit selected year persisted in the definition |
| Page | Product.Category | Categorical filter with no saved selection predicate |
| Page | Product.Subcategory | Categorical filter with no saved selection predicate |
| Page | Store.Store | Categorical filter with no saved selection predicate |
| Visual: brand/gender bar chart | Calendar.Year | Additional categorical filter card; no saved selection predicate |

Presence of a filter card does not establish an active restriction. No specific default year, store, or category is asserted by this guide. When selected, Calendar.Date and Calendar.Year intersect; both filter Sales through Order Date rather than Delivery Date.

#### Interactions and Navigation

- Sales is also configured as a **drillthrough destination** on Calendar.Year, bound to its Year filter, with a single-year requirement. No dedicated Back button is present.
- Category > Subcategory > Product and Brand > Product are visual drill paths. They are not separate semantic model hierarchies.
- Cross-highlighting is not globally disabled, and the report enables filtering other visuals during drill. No explicit per-visual interaction overrides were found; actual effects depend on visual support and need runtime verification.
- Enhanced tooltips are enabled globally; no dedicated tooltip page was found.
- No synchronized slicer metadata or bookmark-driven states were found.

### KPI

Source: [KPI page definition](../lab2/resources/sales.Report/definition/pages/ReportSection6687c48ea8a9b7740002/page.json).

**Screenshot limitation:** No current-project screenshot was captured for KPI either. The same Desktop bridge timeout prevented rendered-page inspection. Matrix values, totals, formatting, and overflow were not visually verified.

#### Main Metrics

- **Sales Amount:** Net-price-based sales amount by category and year.
- **Margin:** Absolute sales margin amount under the same category/year context.
- **Cost:** Quantity-weighted transaction cost.

This page uses a matrix despite its KPI name; it does not define KPI indicator/target visuals, targets, attainment ratios, or margin percentages.

#### Analysis Breakdowns

| Visual ID | Type | Fields / measures | Purpose |
|---|---|---|---|
| `4ebf2d5595cb5ae43396` | Matrix (`pivotTable`) | Product.Category rows; Calendar.Year columns; Sales Amount, Margin, Cost values | Compare the three financial metrics across categories and calendar years; title "KPI table" |
| `92658440a6c902ba3d82` | Textbox | Northwind Retail Group - KPI | Static page heading |

The matrix uses calendar years, not the model's fiscal fields. No additional row or column drill levels are defined.

#### Available Filters

| Scope | Filter or slicer | Behavior / default |
|---|---|---|
| Page | None defined | No saved page filters |
| Visual | None defined | No saved visual filter cards |
| On-canvas slicer | None | No slicer on this page |

The Sales page's filters and date slicer are not defined as report-wide filters or synchronized to KPI. Do not assume changing the Sales slicer will constrain this matrix.

#### Interactions and Navigation

Use normal page tabs to return to Sales. No page-level drillthrough binding, navigation buttons, bookmarks, or dedicated tooltip behavior were found on KPI. Matrix selection behavior was not exercised.

## Global Filters and Navigation

- No report-level filters are defined.
- The filter pane is configured **visible but collapsed** at report level; Sales sets a pane width of 257.
- Report settings enable enhanced tooltips, inline exploration, filter-type changes, and drill filtering of other visuals.
- Cross-highlighting is not globally disabled. Selection/apply-all button settings are off.
- There are no saved report bookmarks, custom page-navigation controls, or hidden/tooltip pages in the inspected source.
- Default page order is Sales, then KPI. Desktop/service user state can differ from the saved project state; persisted service selections were not inspected.

## Theme and Presentation

The registered custom theme uses a blue-led palette with pink, purple, orange, yellow, and red accents. Text classes use Segoe UI / Segoe UI Semibold. It also defines page background/wallpaper and visual styling; page/visual overrides can change the rendered outcome.

Both pages use a 16:9 canvas and static Northwind headings. These observations come from metadata, not screenshot validation. The separate [design reference image](../lab2/resources/contoso-design-standards.png) is workshop context, not proof that a later restyling exercise has already been applied.

## Screenshot Completion and Verification

The [catalog coverage notes](README.md#coverage-and-limitations) record the preview availability failure. Once the Desktop bridge is responsive:

1. Open the current [sales.pbip](../lab2/resources/sales.pbip) in a supported Power BI Desktop build and resolve any unsaved state manually.
2. Use the report authoring skill's supported Desktop preview/capture workflow without changing or publishing the project.
3. Capture **both** Sales and KPI. Review the screenshots against the current source and the bindings described above.
4. Save permanent documentation images as `docs/assets/sales/sales.png` and `docs/assets/sales/kpi.png`; embed them here using `assets/sales/...` relative links.
5. Replace each screenshot limitation only after its page has a fresh reviewed capture.

No report definitions, filters, bindings, or themes were modified by this documentation task.
