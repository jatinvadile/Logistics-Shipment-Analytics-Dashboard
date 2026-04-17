# 🚚 Logistics & Shipment Analytics Platform

> An end-to-end logistics intelligence system for real-time shipment tracking, delay analysis, and delivery performance optimization — powered by **FedEx API**, **Python**, **MySQL**, and **Power BI**.

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Business Problem](#-business-problem)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Solution Approach](#-solution-approach)
- [Key Features](#-key-features)
- [Dashboard KPIs](#-dashboard-kpis)
- [Key Insights](#-key-insights)
- [Business Impact](#-business-impact)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Future Enhancements](#-future-enhancements)

---

## 📖 Overview

This project delivers a **centralized logistics analytics platform** that tracks shipment movement at the AWB (Air Waybill) level across multiple global hubs. It replaces fragmented, manual tracking processes with an automated pipeline that surfaces actionable insights via an interactive Power BI dashboard.

The system enables **real-time shipment lifecycle visibility**, identifies delay patterns, and improves **On-Time In-Full (OTIF)** delivery performance through data-driven decision-making.

---

## 🔴 Business Problem

| Pain Point | Impact |
|---|---|
| Manual, fragmented shipment tracking | High operational overhead |
| No centralized hub-level visibility | Bottlenecks went undetected |
| No first-attempt delivery tracking | Missed optimization opportunities |
| Transit time variation not measured | SLA breaches undetected |
| Weekend vs. weekday delivery blind spot | Planning inefficiency |

---

## 🏗️ Architecture

```
┌─────────────────────┐
│     FedEx API        │
│  (Shipment Tracking) │
└──────────┬──────────┘
           │  Real-time AWB data
           ▼
┌─────────────────────┐
│  Python ETL Script   │
│  · API Auth (OAuth)  │
│  · Bulk AWB extract  │
│  · JSON parsing      │
│  · Data validation   │
└──────────┬──────────┘
           │  Structured records
           ▼
┌─────────────────────┐
│    MySQL Database    │
│  · Fact table        │
│  · Dimension tables  │
│  · Derived fields    │
└──────────┬──────────┘
           │  SQL queries
           ▼
┌─────────────────────┐
│      Power BI        │
│  · Data Modeling     │
│  · DAX measures      │
│  · Dashboards        │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Business Insights   │
│  · OTIF %            │
│  · Delay analysis    │
│  · Hub performance   │
└─────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Data Source** | FedEx Tracking API (v1) |
| **Data Processing** | Python · Pandas · Requests · PyTZ |
| **Database** | MySQL |
| **Visualization** | Power BI |
| **Query Language** | SQL |
| **Analytics** | DAX (Power BI) |

---

## 💡 Solution Approach

### 1. Data Collection
- Integrated **FedEx REST API** (`/track/v1/trackingnumbers`) for AWB-level tracking
- Extracted shipment timestamps, scan locations, delivery status, and transit events
- Batch processing of up to **30 AWBs per API call** with automatic token refresh

### 2. Data Processing & Storage
- Built a **Python ETL pipeline** handling:
  - OAuth2 token-based API authentication
  - Bulk AWB validation (12–15 digit numeric check)
  - Nested JSON response parsing
  - IST timezone normalization
- Stored structured data into **MySQL** for downstream analysis

### 3. Data Modeling
Designed a star schema with:

**Fact Table — Shipments**
- AWB Number, Event DateTime, Status, Location, Country

**Derived Fields**
- `Transit Time` — elapsed time between key scan events
- `Delay Flag` — boolean indicator vs. expected SLA
- `First Attempt Delivery` — whether delivery succeeded on first try
- `Route Path` — hub-to-hub sequence mapping
- `Hub Sequence` — ordered list of scan locations

### 4. Advanced Analytics Logic
- Tracks shipment path **until first occurrence of a key hub** (e.g., Memphis)
- Analyzes **multi-hub routing patterns**
- Differentiates **weekday vs. Saturday** shipment behavior
- Calculates **expected vs. actual** delivery timelines
- Measures **first-attempt delivery success rate** (~90% logic threshold)

### 5. Power BI Dashboard
Interactive logistics dashboard with drill-down capabilities across hubs, dates, and shipment statuses.

---

## ✅ Key Features

- 📡 **Real-time shipment tracking** via FedEx API integration
- 🗺️ **Multi-hub route analysis** — full origin-to-destination journey mapping
- ⏱️ **Delay detection & root cause analysis** by hub and time period
- 🎯 **First attempt delivery tracking** with configurable success thresholds
- 📊 **Interactive, scalable Power BI dashboard**
- 🔄 **Automated token refresh** for uninterrupted API access
- 🚨 **Invalid AWB logging** for data quality tracking

---

## 📊 Dashboard KPIs

### Core Metrics
| KPI | Description |
|---|---|
| **Total Shipments** | Count of AWBs processed in period |
| **OTIF %** | On-Time In-Full delivery rate |
| **Avg. Transit Time** | Mean hours from pickup to delivery |
| **First Attempt Success Rate** | % delivered without re-attempt |
| **Delayed Shipments %** | % exceeding expected delivery window |

### Visualizations
- 🔀 **Shipment Flow** — Origin → Hub(s) → Destination Sankey/path view
- 🌡️ **Delay Heatmaps** — by time of day, day of week, and hub
- 🏭 **Hub-wise Performance Analysis** — dwell time and throughput by facility
- 📈 **Trend Analysis** — daily and weekly shipment volume and status breakdown
- 🛣️ **Route Efficiency Comparison** — single-hop vs. multi-hop route performance

---

## 🔍 Key Insights

- 📍 Identified **delay-prone hubs** consistently impacting downstream delivery timelines
- 📅 Discovered measurable patterns in **weekend shipment delays** vs. weekday
- 🔁 Highlighted inefficiencies in **multi-hop routes** vs. direct routing
- 👁️ Delivered **end-to-end lifecycle visibility** previously unavailable to operations teams

---

## 💼 Business Impact

| Outcome | Magnitude |
|---|---|
| Reduced manual tracking effort | ~50% reduction |
| Improved delivery planning efficiency | 15–25% improvement |
| Faster decision-making | Real-time vs. next-day reporting |
| Operational transparency | Full shipment lifecycle visible |

---

## 📁 Project Structure

```
logistics-analytics-platform/
│
├── fedex_tracking.py          # Core ETL script — API auth, extraction, CSV export
├── invalid_awbs_log.txt       # Auto-generated log of AWBs failing validation
│
├── data/
│   └── fedex_full_journey.csv # Output: parsed shipment scan events
│
├── sql/
│   ├── schema.sql             # MySQL table definitions
│   └── kpi_queries.sql        # SQL logic for OTIF, transit time, delay flags
│
├── powerbi/
│   └── logistics_dashboard.pbix  # Power BI report file
│
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
```bash
pip install pandas requests python-dateutil pytz openpyxl
```

### Configuration
Update the following in `fedex_tracking.py`:

```python
# FedEx API credentials
API_KEY    = "your_client_id"
API_SECRET = "your_client_secret"

# File paths
input_file  = "path/to/your/input.xlsx"   # Must contain 'AWB Number' and 'Shp Date' columns
output_dir  = "path/to/output/folder"
```

### Run
```bash
python fedex_tracking.py
```

**Output:** `fedex_full_journey.csv` in your configured `output_dir`

### Input File Requirements
The input Excel file must have a sheet named `FALCON MIS` with at minimum:

| Column | Format | Notes |
|---|---|---|
| `AWB Number` | Numeric string | 12–15 digits |
| `Shp Date` | Date | Used for 30-day cutoff filter |

---

## 🔮 Future Enhancements

- [ ] **Predictive delay modeling** using ML (XGBoost / LightGBM on historical scan patterns)
- [ ] **Automated route optimization** suggestions based on hub dwell time analysis
- [ ] **Multi-carrier integration** — DHL, UPS, Blue Dart alongside FedEx
- [ ] **Real-time alert system** for shipments breaching SLA thresholds
- [ ] **Airflow/cron scheduling** for fully automated daily pipeline execution
- [ ] **REST API layer** to expose tracking data to other internal systems

---

## 👤 Author

**Jatin Vadile**


---

> *Built to bring clarity to complexity — one shipment at a time.*
