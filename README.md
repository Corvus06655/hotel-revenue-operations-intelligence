# Hotel Revenue & Operations Intelligence

A Power BI Project (PBIP) for examining hotel booking performance, realised revenue, occupancy, property-level results, booking status, and operational KPIs from an OLX-provided real-time hotel booking source. The repository publishes the inspectable report definition and semantic model rather than only a static dashboard image.

> **Data availability:** The public repository contains the Power BI report and semantic-model definitions, but not the source CSV files. Numeric findings are therefore intentionally not presented here until the source data is supplied and the report is refreshed.

## Business Problem

Hotel operators need a consistent view of booking volume, realised revenue, room capacity, occupancy, booking status, property performance, and booking-platform mix. This project organizes those questions into a two-page Power BI report with a dimensional model and DAX measures that can support executive monitoring and property-level operational review.

The analysis is designed to help a reviewer inspect how the model connects booking activity with hotel, room, and date dimensions, and how the report surfaces revenue, occupancy, status, platform, rating, and property comparisons. It does not claim a measured business impact because the source CSV files are not included in the public repository.

## Business Objectives

The published report is structured to:

- Monitor recorded revenue, booking volume, capacity, successful bookings, occupancy, ADR, and related operating measures.
- Compare hotel properties, cities, room classes, booking platforms, and booking-status categories.
- Examine monthly and weekly movement in revenue, occupancy, ADR, realisation, and capacity-related measures.
- Provide a property-and-operations view for investigating cancellations, no-shows, ratings, platform mix, and property-level performance.
- Make the report logic auditable through visible PBIP, TMDL, DAX, page, visual, and theme definitions.

## Decision-Oriented Business Questions

The report structure supports questions such as:

1. How does realised revenue change across the available reporting period?
2. Which properties, cities, and room classes contribute most to recorded revenue and booking volume?
3. How do capacity and successful bookings translate into the model's occupancy percentage?
4. What is the booking-status mix across checked-out, cancelled, and no-show bookings?
5. Which booking platforms account for the largest booking volumes in the available data?
6. How do property, room-class, platform, rating, and status dimensions relate to the operating KPIs shown in the report?
7. How do revenue, occupancy, ADR, realisation, and capacity-related measures change week over week?

These are analytical questions supported by the published schema and visual bindings. They are not presented as findings because the underlying CSV data is not distributed with this repository.

## Dashboard Storytelling

The report contains two pages arranged from broad performance monitoring to operational investigation.

| Page | Decision focus | Published visual coverage |
| --- | --- | --- |
| **Executive Overview** | Establish a high-level view of hotel performance. | Total revenue, booking volume, occupancy, ADR, average rating, monthly revenue trend, revenue by room class, city and room filters, and status-related monitoring. |
| **Property & Operations** | Compare properties and investigate operating drivers. | Property performance detail, booking-platform mix, booking-status volume, cancellation and no-show measures, ratings, realisation, room and property filters, and operational KPI cards. |

The report pages use a consistent dark visual theme and embedded report assets. Page and visual definitions are retained in the PBIP folders so the presentation can be reviewed alongside the underlying model logic.

## Dataset and Analytical Grain

### Source and availability

The model identifies the source as an OLX-provided real-time hotel booking dataset. The raw files are not included in the repository, and the public model currently expects them in a local `data` folder. The repository therefore documents the model structure and calculation logic without inventing row counts, column counts, date coverage, or numeric outcomes.

### Raw tables and dimensions

| Object | Role | Documented fields |
| --- | --- | --- |
| `dim_date` | Calendar and weekly analysis dimension. | Date, month label, week number, calculated week number, and calculated weekday/weekend classification. |
| `dim_hotels` | Property lookup dimension. | Property ID, property name, category, and city. |
| `dim_rooms` | Room-class lookup dimension. | Room ID and room class. |
| `fact_bookings` | Booking-level fact table. | Booking ID, property ID, booking date, check-in date, checkout date, guest count, room category, booking platform, rating, booking status, generated revenue, and realised revenue. |
| `fact_aggregated_bookings` | Capacity and successful-booking fact table. | Property ID, check-in date, room category, successful bookings, and capacity. |

The model joins the fact tables to hotel and room dimensions and connects check-in dates to `dim_date`. Booking and checkout dates also retain local date relationships for date-specific analysis.

### Raw, cleaned, and derived data

The published Power Query definitions show the following preparation steps:

- **Raw inputs:** five CSV files expected in a local data folder: `dim_date.csv`, `dim_hotels.csv`, `dim_rooms.csv`, `fact_aggregated_bookings.csv`, and `fact_bookings.csv`.
- **Cleaning and typing:** CSV content is imported, headers are promoted, and the documented date, integer, and text fields are assigned explicit data types.
- **Derived fields:** `dim_date` recomputes a week number and a `day_type` classification in the model. The model also derives KPI measures through DAX.
- **Aggregations:** capacity and successful-booking values are summed from `fact_aggregated_bookings`; booking and revenue measures are calculated from `fact_bookings`.

The public definitions do not document explicit missing-value imputation, duplicate removal, category standardization, outlier treatment, or invalid-value remediation. Those checks should be completed after the source files are supplied and refreshed.

## KPI Framework

The following measures are present in the semantic model. Definitions below use the model's current calculation logic.

| KPI | Current model definition | Business meaning |
| --- | --- | --- |
| **Realised Revenue** | Sum of `fact_bookings[revenue_realized]`. | Recorded revenue realised from the booking fact table. |
| **Total Bookings** | Count of `fact_bookings[booking_id]`. | Booking volume at the booking-record level. |
| **Total Capacity** | Sum of `fact_aggregated_bookings[capacity]`. | Capacity recorded in the aggregated bookings table. |
| **Successful Bookings** | Sum of `fact_aggregated_bookings[successful_bookings]`. | Successful-booking volume recorded in the capacity table. |
| **Occupancy %** | Successful bookings divided by total capacity. | The model's capacity-utilisation measure. |
| **Cancelled Bookings / Cancelled %** | Count of bookings where status is `Cancelled`; divided by total bookings for the percentage. | Cancellation volume and its share of booking records. |
| **Checkout Volume** | Total bookings filtered to `Checked Out`. | Booking records with a checked-out status. |
| **No-show Volume / No-show Rate %** | Total bookings filtered to `No Show`; divided by total bookings for the rate. | No-show volume and its share of booking records. |
| **ADR** | Realised revenue divided by total bookings. | Average realised revenue per booking record under the model's definition. |
| **RevPAR** | Realised revenue divided by total capacity. | Revenue relative to recorded capacity under the model's definition. |
| **Average Rating** | Average of `fact_bookings[ratings_given]`. | Average rating value present in the booking records. |
| **DBRN / DSRN / DURN** | Total bookings, total capacity, and checkout volume divided by the model's number of days. | Daily-rate measures based on the available date span. |
| **Week-over-week changes** | Current-week versus prior-week comparison for revenue, occupancy, ADR, realisation, and DSRN. | Directional movement between the model's weekly periods. |

### Model QA notes

Two existing definitions require validation before being presented as polished percentage KPIs. `Booking % by Platform` and `Booking % by Room Class` currently multiply a filtered booking count by 100 after removing the relevant category filter; they do not calculate a category share using a total-bookings denominator. In addition, `Realisation %` is currently defined as `1 - [Cancelled %] + [No Show rate %]`. The report presentation should preserve these measures as model evidence but should not describe either formula as a validated business rate until the logic is reviewed against the intended definitions.

## Analytical Methodology

The repository follows this evidence-based workflow:

> **CSV ingestion → header promotion and type casting → dimensional relationships → derived date fields → DAX KPI calculation → executive monitoring → property and operations comparison**

The PBIP structure separates report presentation from the semantic model, allowing a reviewer to trace visible visuals back to fields, measures, relationships, and source queries. This is especially useful for verifying whether a claimed KPI is actually implemented rather than merely described in the README.

## Key Insights and Recommendations

Because the source CSV files are not included, this repository does not publish numeric findings or claim a measured performance result. The defensible insight at this stage is about analytical coverage: the report is set up to examine revenue, booking volume, occupancy, capacity, status outcomes, platform mix, room classes, ratings, and property performance through two decision-oriented views.

Once the data is supplied and refreshed, the next analysis should quantify the largest revenue and booking contributors, compare occupancy and realised revenue across properties and room classes, validate cancellation and no-show rates, and investigate week-over-week movements. Management recommendations should be written only after those calculated results and the measure QA notes above have been validated.

## Limitations

- The raw CSV files are not distributed, so numeric results cannot be independently reproduced from this public repository alone.
- The source provenance is documented as OLX-provided real-time data, but no public source URL or data dictionary is included in the repository.
- The model contains booking-level and aggregated-capacity tables, so measures should be interpreted at their respective grains.
- Cost, profit, acquisition cost, and margin variables are not documented in the published schema; revenue measures should not be treated as profitability measures.
- Customer-level attributes beyond guest count and booking dimensions are not documented, limiting customer segmentation.
- Ratings and booking status are available as recorded fields, but the model does not establish causal relationships between them and revenue or occupancy.
- Missing-value, duplicate, outlier, and invalid-value treatment is not documented in the published Power Query definitions.
- The percentage measures identified in the model QA notes require validation before being used for formal management reporting.

## Reproducibility

### Requirements

- Power BI Desktop with PBIP support.
- Access to the five source CSV files expected by the semantic model.
- A Windows environment capable of opening the PBIP project in Power BI Desktop.

### Local setup

1. Clone or download this repository.
2. Create a local `data` folder beside the PBIP project.
3. Add `dim_date.csv`, `dim_hotels.csv`, `dim_rooms.csv`, `fact_aggregated_bookings.csv`, and `fact_bookings.csv` to that folder.
4. Open the TMDL source definitions and update the `Folder.Files(...)` path if the local folder differs from the neutral placeholder path.
5. Open `olx_hotel_booking_analytics.pbip` in Power BI Desktop.
6. Refresh the model and review **Executive Overview** and **Property & Operations**.

### Expected outputs

A successful refresh should make the two report pages available with their configured visuals, filters, semantic relationships, DAX measures, and embedded theme resources. The exact numeric outputs depend on the source files supplied by the reviewer.

## Repository Structure

```text
.
├── olx_hotel_booking_analytics.pbip
├── olx_hotel_booking_analytics.Report/
│   ├── definition/
│   │   ├── pages/
│   │   ├── report.json
│   │   └── version.json
│   └── StaticResources/
├── olx_hotel_booking_analytics.SemanticModel/
│   ├── definition/
│   │   ├── cultures/
│   │   ├── tables/
│   │   ├── model.tmdl
│   │   └── relationships.tmdl
│   └── diagramLayout.json
├── .gitignore
└── README.md
```

The PBIP project is surfaced directly rather than being hidden inside a ZIP archive. This lets recruiters and collaborators inspect the report pages, visual bindings, semantic model, relationships, and DAX measures from GitHub.

## Professional Positioning

This project’s differentiation is not a claim of unique data. Its portfolio value comes from the business framing, the separation of executive and operational questions, the inspectable dimensional model, the KPI definitions, the documented reproducibility path, and the explicit disclosure of data availability and measure-validation constraints.

## References

[1]: ./olx_hotel_booking_analytics.SemanticModel/definition/tables/Measures%20%282%29.tmdl "Published DAX measure definitions"
[2]: ./olx_hotel_booking_analytics.SemanticModel/definition/tables/fact_bookings.tmdl "Published booking fact-table definition"
[3]: ./olx_hotel_booking_analytics.SemanticModel/definition/tables/fact_aggregated_bookings.tmdl "Published capacity fact-table definition"
[4]: ./olx_hotel_booking_analytics.SemanticModel/definition/relationships.tmdl "Published semantic-model relationships"
[5]: ./olx_hotel_booking_analytics.Report/definition/pages/pages.json "Published report page order"
