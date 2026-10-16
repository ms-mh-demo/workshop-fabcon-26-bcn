# Power BI Artifact Catalog

## Overview

This repository supports a three-lab workshop on agentic Power BI development. Its source-controlled Power BI Project is the **sales** example in [Lab 2 resources](../lab2/resources/), used for brownfield modeling, documentation, and report authoring. It analyzes retail sales, product performance, customers, stores, costs, and margin under the Northwind Retail Group name.

The project contains one semantic model and one report. The report resolves its model through a local relative-path binding, not a published Fabric semantic model.

Documentation reflects inspection on **2026-10-16**. Semantic metadata was inspected through the Power BI Modeling MCP against the local model definition; report metadata was inspected with the report authoring CLI and supporting PBIR JSON. No model, report, data source, or deployment was changed.

## Artifacts

| Artifact | Type | Source | Connected artifact(s) | Purpose | Documentation |
|---|---|---|---|---|---|
| sales | Semantic model | [lab2/resources/sales.SemanticModel](../lab2/resources/sales.SemanticModel/) | sales report | Imported retail sales star schema with revenue, cost, margin, and time comparisons | [Model guide](sales.semantic-model.md) |
| sales | Report | [lab2/resources/sales.Report](../lab2/resources/sales.Report/) | sales semantic model | Sales analysis and category-by-year KPI matrix | [Report guide](sales.report.md) |

The [sales.pbip entry point](../lab2/resources/sales.pbip) opens the report. Its [definition.pbir](../lab2/resources/sales.Report/definition.pbir) binds to `../sales.SemanticModel`.

### Packaged workshop resources

These PBIX files are packaged resources, not additional source-controlled PBIP definitions. Their embedded models and reports were not opened or extracted during this inspection; they are not assumed to match the sales PBIP.

| Package | Workshop use | Inspection coverage |
|---|---|---|
| [ManufacturingOps.pbix](../lab1/resources/ManufacturingOps.pbix) | [Lab 1](../lab1/lab.md): web modeling and agentic authoring | Package identified; internal schema and pages not inspected |
| [sales.pbix](../lab2/resources/sales.pbix) | [Lab 2](../lab2/lab.md): starting report for conversion to PBIP | Package identified; use the model/report guides for the adjacent PBIP, not as proof of binary equivalence |
| [Contoso Sales.pbix](../lab3/resources/Contoso%20Sales.pbix) | [Lab 3](../lab3/lab.md): Fabric Data Apps starting point | Package identified; internal schema and pages not inspected |

Detailed artifact documents cover the one model and one report exposed as project source. Documenting the PBIX internals requires opening or exporting those packages separately.

## Repository map

| Location | Responsibility |
|---|---|
| [Workshop README](../README.md), [prerequisites](../pre-requisites.md), [troubleshooting](../troubleshooting.md) | Workshop navigation, environment setup, and recovery guidance |
| [Lab 1](../lab1/lab.md) | Browser-based semantic model authoring |
| [Lab 2](../lab2/lab.md) | Existing PBIP development and a separate greenfield Fabric exercise |
| [Lab 3](../lab3/lab.md) | Data application creation from an existing semantic model |
| [sales.SemanticModel/definition](../lab2/resources/sales.SemanticModel/definition/) | Semantic model source in TMDL |
| [sales.Report/definition](../lab2/resources/sales.Report/definition/) | PBIR report, page, visual, and filter definitions |
| [Report static resources](../lab2/resources/sales.Report/StaticResources/) | Base theme and registered custom theme |
| [DAXQueries](../lab2/resources/sales.SemanticModel/DAXQueries/) | Saved DAX query; the supplied example selects up to 100 Calendar rows |
| [TMDLScripts](../lab2/resources/sales.SemanticModel/TMDLScripts/) | Saved authoring scripts; not a replacement for the current model definition |
| [Sample data](../resources/sample-data/) | CSV resources for the workshop's separate sales and lakehouse examples |

## Open and develop the project

1. Complete the [workshop prerequisites](../pre-requisites.md). Lab 2 specifies Power BI Desktop August 2026 or later.
2. Open [sales.pbip](../lab2/resources/sales.pbip) in Power BI Desktop. Keep both sibling artifact folders in place so the relative model binding resolves.
3. Review the [data source and refresh notes](sales.semantic-model.md#data-sources-and-refresh) before refreshing. The configured CSV source is a remote GitHub URL; the repository's local CSV files are not automatically read.
4. For agent-assisted work, follow [AGENTS.md](../lab2/resources/AGENTS.md), the [team rules](../lab2/resources/team-rules.md), and the [documentation skill](../lab2/resources/.github/skills/powerbi-documentation/SKILL.md). Load semantic model/report skills for their respective surfaces. Semantic model changes must use the modeling MCP rather than direct TMDL edits.
5. Review external changes in Desktop before saving. For actual model or report changes, validate the affected calculations, filters, and rendered pages using the corresponding authoring workflow.
6. Review changes in Git. The repository's [ignore rules](../.gitignore) exclude local settings and model caches; preserve that separation from portable project source.

Opening the project does not publish it. A Fabric deployment, credentials, refresh schedule, service connections, and workspace permissions are separate concerns and were not inspected.

## Business context versus implemented behavior

The [company context](../lab2/resources/company-context.md) supplies terminology and desired business conventions, not evidence that every convention has been implemented.

- Revenue is described by the business as net sales. The model multiplies quantity by net price; it has no separate returns or discounts calculation.
- Business fiscal years start in February. The current Calendar query instead changes fiscal year in July and uses a separate fiscal-quarter formula; see the [calendar caveat](sales.semantic-model.md#calendar-and-fiscal-periods).
- Country-restricted access is a stated business expectation, but the source model has no security roles.
- Currency conversion, sales per square meter, an About table, and uppercase measure names are not implemented in this baseline. Later lab instructions or team rules must not be mistaken for the current artifact state.
- The [lakehouse notebook](../lab2/resources/notebook.py) belongs to the greenfield exercise: it loads six `dimension_*` / `fact_sale` CSVs into Delta tables in `Lakehouse_01`. It is not a dependency of the current Import-mode sales PBIP, which reads `RAW-*` CSVs.

## Coverage and limitations

- Model inventory: **5 tables, 63 columns, 13 measures, 5 relationships, 1 hierarchy, 0 calculation groups, 0 security roles**.
- Report inventory: **2 pages, 9 visuals**; every page and visual is covered in the [report guide](sales.report.md).
- No measure values, source downloads, refresh execution, service security, or binary PBIX internals were verified. The folder connection is metadata-only.
- **Screenshots are unavailable for both pages.** The report authoring CLI's Desktop host probe reported `available: false` with `bridge.manifest probe timed out after 3000ms`. No fresh page capture or rendered-output review was possible. Existing workshop illustrations were not presented as current-project screenshots.
- Each report page has an explicit capture limitation. No empty image links or fabricated screenshots are included.

## Maintaining these documents

After changing project source, refresh the model inventory, DAX explanations, relationship diagram, report page/visual inventory, and filter descriptions together. Capture each report page once Desktop preview is available, store permanent documentation images under `docs/assets/sales/`, and embed links relative to [sales.report.md](sales.report.md), such as `assets/sales/sales.png`.

Check relative links and Mermaid rendering when reviewing documentation. Re-inspect any PBIX package independently before extending the catalog with its internal artifacts.
