# Hotel Revenue & Operations Intelligence

A Power BI Project (PBIP) for examining hotel booking performance, realised revenue, occupancy, property-level results, booking status, and operational KPIs from an OLX-provided real-time hotel booking source. The repository publishes the inspectable report definition and semantic model rather than only a static dashboard image.

> **Data availability:** The public repository contains the Power BI report and semantic-model definitions, but not the source CSV files. Numeric findings are therefore intentionally not presented here until the source data is supplied and the report is refreshed.

## Business Problem

Hotel operators need a consistent view of booking volume, realised revenue, room capacity, occupancy, booking status, property performance, and booking-platform mix. This project organizes those questions into a two-page Power BI report with a dimensional model and DAX measures that can support executive monitoring and property-level operational review.

## Business Objectives

The published report is structured to:

- Monitor recorded revenue, booking volume, capacity, successful bookings, occupancy, ADR, and related operating measures.
- Compare hotel properties, cities, room classes, booking platforms, and booking-status categories.
- Examine monthly and weekly movement in revenue, occupancy, ADR, realisation, and capacity-related measures.
- Provide a property-and-operations view for investigating cancellations, no-shows, ratings, platform mix, and property-level performance.
- Make the report logic auditable through visible PBIP, TMDL, DAX, page, visual, and theme definitions.

## Dashboard Storytelling

The report contains two pages arranged from broad performance monitoring to operational investigation.

| Page | Decision focus | Published visual coverage |
| --- | --- | --- |
| **Executive Overview** | Establish a high-level view of hotel performance. | Total revenue, booking volume, occupancy, ADR, average rating, monthly revenue trend, revenue by room class, city and room filters, and status-related monitoring. |
| **Property & Operations** | Compare properties and investigate operating drivers. | Property performance detail, booking-platform mix, booking-status volume, cancellation and no-show measures, ratings, realisation, room and property filters, and operational KPI cards. |

## KPI Framework

The following measures are present in the semantic model. Definitions below use the published calculation logic.

| KPI | Definition | Business meaning |
| --- | --- | --- |
| **Realised Revenue** | Sum of `fact_bookings[revenue_realized]`. | Recorded realised revenue from the booking fact table. |
| **Total Bookings** | Count of `fact_bookings[booking_id]`. | Booking volume at booking-record level. |
| **Total Capacity** | Sum of `fact_aggregated_bookings[capacity]`. | Capacity recorded in the aggregated bookings table. |
| **Successful Bookings** | Sum of `fact_aggregated_bookings[successful_bookings]`. | Successful-booking volume recorded in the capacity table. |
| **Occupancy %** | Successful bookings divided by total capacity. | Capacity utilisation under the model definition. |
| **Cancelled %** | Cancelled bookings divided by total bookings. | Share of booking records with Cancelled status. |
| **No-show Rate %** | No-show bookings divided by total bookings. | Share of booking records with No Show status. |
| **Booking % by Platform** | Current platform booking count divided by total booking count with the platform filter removed. | Platform share of booking records. |
| **Booking % by Room Class** | Current room-class booking count divided by total booking count with the room-class filter removed. | Room-class share of booking records. |
| **ADR** | Realised revenue divided by total bookings. | Average realised revenue per booking record under the model definition. |
| **RevPAR** | Realised revenue divided by total capacity. | Revenue relative to recorded capacity under the model definition. |
| **Realisation %** | `1 - [Cancelled %] - [No Show rate %]`. | Share of booking records remaining after recorded cancellations and no-shows. This is a model-derived operational KPI, not a profitability measure. |
| **Average Rating** | Average of `fact_bookings[ratings_given]`. | Average rating value in the booking records. |
| **DBRN / DSRN / DURN** | Bookings, capacity, and checkout volume divided by the model's number of days. | Daily-rate measures based on the available date span. |
| **Week-over-week changes** | Current-week versus prior-week comparison for revenue, occupancy, ADR, realisation, and DSRN. | Directional movement between weekly periods. |

### Model QA — resolved

The percentage measures that were previously ambiguous have been corrected in the semantic model:

- **Booking % by Platform** now uses `DIVIDE([Total Booking], CALCULATE([Total Booking], ALL(fact_bookings[booking_platform])), 0)` instead of multiplying an unfiltered count by 100.
- **Booking % by Room Class** now uses `DIVIDE([Total Booking], CALCULATE([Total Booking], ALL(dim_rooms[room_class])), 0)` instead of multiplying an unfiltered count by 100.
- **Realisation %** now uses `1 - [Cancelled %] - [No Show rate %]`, so cancellations and no-shows both reduce the realised share.

These measures are formatted as percentages in the semantic model. They should still be interpreted according to the documented model grain and business definition.

## Dashboard Preview

The PBIP contains the two report pages **Executive Overview** and **Property & Operations**. The public repository does not include the source CSV files, so fresh report screenshots require a local Power BI Desktop refresh with the source data. No synthetic dashboard screenshot is substituted for the real report.

## Dataset and Analytical Grain

The model identifies the source as an OLX-provided real-time hotel booking dataset. The raw files are not included in the repository, and the public model currently expects them in a local `data` folder. The repository therefore documents the model structure and calculation logic without inventing row counts, column counts, date coverage, or numeric outcomes.

| Object | Role | Documented fields |
| --- | --- | --- |
| `dim_date` | Calendar and weekly analysis dimension. | Date, month label, week number, calculated week number, and calculated weekday/weekend classification. |
| `dim_hotels` | Property lookup dimension. | Property ID, property name, category, and city. |
| `dim_rooms` | Room-class lookup dimension. | Room ID and room class. |
| `fact_bookings` | Booking-level fact table. | Booking ID, property ID, booking date, check-in date, checkout date, guest count, room category, booking platform, rating, booking status, generated revenue, and realised revenue. |
| `fact_aggregated_bookings` | Capacity and successful-booking fact table. | Property ID, check-in date, room category, successful bookings, and capacity. |

## Analytical Methodology

The repository follows this workflow:

> **CSV ingestion → header promotion and type casting → dimensional relationships → derived date fields → DAX KPI calculation → executive monitoring → property and operations comparison**

The PBIP structure separates report presentation from the semantic model, allowing a reviewer to trace visible visuals back to fields, measures, relationships, and source queries.

## Key Insights and Recommendations

Because the source CSV files are not included, this repository does not publish numeric findings or claim a measured performance result. The report is designed to quantify revenue, booking volume, occupancy, capacity, status outcomes, platform mix, room classes, ratings, and property performance once the source data is refreshed.

## Limitations

- The raw CSV files are not distributed, so numeric results cannot be independently reproduced from this public repository alone.
- The source provenance is documented as OLX-provided real-time data, but no public source URL or data dictionary is included in the repository.
- The model contains booking-level and aggregated-capacity tables, so measures should be interpreted at their respective grains.
- Cost, profit, acquisition cost, and margin variables are not documented in the published schema; revenue measures should not be treated as profitability measures.
- Missing-value, duplicate, outlier, and invalid-value treatment is not fully documented in the published Power Query definitions.

## Reproducibility

### Requirements

- Power BI Desktop with PBIP support.
- Access to the five source CSV files expected by the semantic model.
- A Windows environment capable of opening the PBIP project in Power BI Desktop.

### Local setup

1. Clone or download this repository.
2. Create a local `data` folder beside the PBIP project.
3. Add `dim_date.csv`, `dim_hotels.csv`, `dim_rooms.csv`, `fact_aggregated_bookings.csv`, and `fact_bookings.csv` to that folder.
4. Open the TMDL source definitions and update the `Folder.Files(...)` path if the local folder differs from the placeholder path.
5. Open `olx_hotel_booking_analytics.pbip` in Power BI Desktop.
6. Refresh the model and review **Executive Overview** and **Property & Operations**.
7. Export or capture both refreshed report pages for the README when publishing a visual preview.

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

This project's portfolio value comes from the business framing, the separation of executive and operational questions, the inspectable dimensional model, the KPI definitions, the documented reproducibility path, and explicit disclosure of data availability and model limitations.

## References

[1]: ./olx_hotel_booking_analytics.SemanticModel/definition/tables/Measures%20%282%29.tmdl "Published DAX measure definitions"
[2]: ./olx_hotel_booking_analytics.SemanticModel/definition/tables/fact_bookings.tmdl "Published booking fact-table definition"
[3]: ./olx_hotel_booking_analytics.SemanticModel/definition/tables/fact_aggregated_bookings.tmdl "Published capacity fact-table definition"
[4]: ./olx_hotel_booking_analytics.SemanticModel/definition/relationships.tmdl "Published semantic-model relationships"
[5]: ./olx_hotel_booking_analytics.Report/definition/pages/pages.json "Published report page order"
