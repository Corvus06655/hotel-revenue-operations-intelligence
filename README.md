# OLX Hotel Booking Analytics Dashboard

A portfolio-ready **OLX Hotel Booking Analytics Dashboard** built as a Power BI Project (PBIP). The project combines a dimensional semantic model, DAX measures, report-page definitions, and embedded visual assets to turn hotel reservation data into executive and operational insights.

## Dataset source

This project analyzes a real-time hotel booking dataset provided by **OLX**. The dataset is used as the source for the booking, property, room, capacity, revenue, status, platform, and rating analysis modeled in the report. The raw source files are not redistributed in this repository; the source-file expectations and refresh instructions are documented below.

## Project overview

The dashboard is designed for hotel-management questions such as:

- How are revenue, bookings, occupancy, and average daily rate changing over time?
- Which properties and room classes contribute most to performance?
- What proportion of bookings are cancelled, marked as no-show, or successfully realised?
- How do booking platforms, room categories, ratings, and property characteristics relate to performance?
- Which operational KPIs should a hotel manager monitor week over week?

## Dashboard pages

| Page | Purpose | Included analysis |
| --- | --- | --- |
| **Executive Overview** | High-level performance monitoring | Revenue, bookings, occupancy, ADR, city-level performance, room-class analysis, and booking-status trends |
| **Property & Operations** | Detailed operational review | Cancellation and no-show rates, booking-platform mix, property-level performance, ratings, realisation, and operational filters |

## Key analytical measures

The semantic model includes measures for revenue, total bookings, total capacity, successful bookings, occupancy percentage, cancellation percentage, checkout volume, no-show rate, booking mix, average daily rate (ADR), realisation percentage, revenue per available room (RevPAR), daily booking and capacity rates, average rating, and week-over-week KPI changes.

## Data model

The model follows a star-schema design with separate date, hotel, and room dimensions connected to booking fact tables.

| Model object | Role |
| --- | --- |
| `dim_date` | Calendar and week-based time analysis |
| `dim_hotels` | Hotel or property attributes |
| `dim_rooms` | Room-class attributes |
| `fact_bookings` | Booking-level dates, status, platform, guests, ratings, and revenue fields |
| `fact_aggregated_bookings` | Capacity and successful-booking aggregates used for occupancy analysis |
| `Measures (2)` | DAX measures used by the report visuals |

## Repository structure

```text
.
├── olx_hotel_booking_analytics.pbip
├── olx_hotel_booking_analytics.Report/
│   ├── definition/
│   │   ├── pages/
│   │   ├── report.json
│   │   └── version.json
│   └── StaticResources/
└── olx_hotel_booking_analytics.SemanticModel/
    ├── definition/
    │   ├── tables/
    │   ├── model.tmdl
    │   └── relationships.tmdl
    └── diagramLayout.json
```

## How to open the project

1. Install a current version of [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
2. Obtain the source CSV files expected by the model: `dim_date.csv`, `dim_hotels.csv`, `dim_rooms.csv`, `fact_aggregated_bookings.csv`, and `fact_bookings.csv`.
3. Place the files in a local `data` folder and update the `Folder.Files(...)` source location in the relevant TMDL files if your folder location differs.
4. Open `olx_hotel_booking_analytics.pbip` in Power BI Desktop.
5. Review the model relationships, refresh the data, and navigate between **Executive Overview** and **Property & Operations**.

## Complete project download

The repository root includes `olx-hotel-booking-analytics-project.zip`, a downloadable copy of the complete PBIP project with its nested report, semantic-model, visual, theme, and metadata files preserved.

## Data availability note

The supplied archive did not contain the raw CSV files. To keep the public repository complete without pretending that unavailable data is included, this repository contains the PBIP report definition, semantic-model definition, DAX measures, report visuals, themes, and design assets. The dataset is identified as an OLX-provided real-time source for portfolio context; a reviewer can inspect the full implementation on GitHub and open the report after supplying the expected source files.

## Portfolio value

This project demonstrates practical capability across Power BI report development, semantic modeling, DAX, dimensional data design, KPI definition, operational analysis, visual layout, and business-oriented storytelling. It is intentionally published as a structured PBIP project so reviewers can inspect the model and report definitions rather than seeing only a static dashboard image.

## Public-repository hygiene

Local Power BI cache and settings files are excluded through `.gitignore`. Personal machine-specific source paths have been replaced with a neutral placeholder path in the public model definition. No credentials, tokens, or raw private data are included in this repository.

## Suggested resume entry

**OLX Hotel Booking Analytics Dashboard | Power BI, DAX, Power Query, Data Modeling** — Built a two-page hotel performance dashboard with executive and operational views, star-schema modeling, booking and occupancy KPIs, cancellation/no-show analysis, property and room-class comparisons, and week-over-week measures.
