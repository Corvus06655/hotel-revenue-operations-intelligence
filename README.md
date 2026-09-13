# Hotel Revenue & Operations Intelligence

Power BI portfolio project focused on hotel revenue, booking performance, occupancy, property analysis, and operational KPIs.

## 📊 Dashboard

The **Power BI dashboard is ready to download and open**.

### Open the dashboard

1. Click **Code → Download ZIP** on this repository.
2. Extract the downloaded ZIP.
3. Open **`olx_hotel_booking_analytics.pbip`** with **Power BI Desktop**.
4. Keep the extracted folder structure unchanged.
5. Add the required source CSV files, refresh the model, and open the report.

### Report pages

- **Executive Overview** — revenue, bookings, occupancy, ADR, ratings, trends, city and room analysis.
- **Property & Operations** — property comparison, platform mix, booking status, cancellations, no-shows, ratings, and operational KPIs.

> **Data note:** The public repository does not include the source CSV files. The PBIP project contains the report definition, semantic model, DAX measures, relationships, themes, and visual configuration.

## 🧮 Key KPIs

Revenue · Total Bookings · Capacity · Successful Bookings · Occupancy % · Cancelled % · No-show Rate % · ADR · RevPAR · Realisation % · DBRN · DSRN · DURN · Week-over-week changes

## 🛠️ Tech Stack

**Power BI · DAX · Power Query · PBIP/TMDL · Data Modelling**

## ✅ Model QA

The category-share measures use a proper total-bookings denominator:

- `Booking % by Platform`
- `Booking % by Room Class`

`Realisation %` is defined as:

```text
1 - [Cancelled %] - [No Show rate %]
```

## 📁 Project structure

```text
.
├── README.md
├── .gitignore
├── olx_hotel_booking_analytics.pbip
├── olx_hotel_booking_analytics.Report/
└── olx_hotel_booking_analytics.SemanticModel/
```

The repository keeps the PBIP components together so that **GitHub → Download ZIP → Extract → Open `.pbip`** is the complete workflow.
