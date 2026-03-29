<div align="center">

# 🚦 UK Road Accident Analysis (2019–2022)

### A Tableau Dashboard for Year-on-Year Road Safety Intelligence

[![Tableau](https://img.shields.io/badge/Tableau-2025.3-E97627?style=for-the-badge&logo=tableau&logoColor=white)](https://www.tableau.com/)
[![Dataset](https://img.shields.io/badge/Dataset-UK%20Road%20Accidents-blue?style=for-the-badge&logo=databricks&logoColor=white)]()
[![Years](https://img.shields.io/badge/Coverage-2019–2022-green?style=for-the-badge)]()
[![Geography](https://img.shields.io/badge/Geography-United%20Kingdom-red?style=for-the-badge)]()

<br/>

*An interactive Tableau workbook analyzing fatal, serious, and slight road accident casualties across the United Kingdom — with dynamic Year-on-Year comparison, severity filtering, and geographic mapping.*

**Author: Supriyo Maity**

</div>

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Dataset](#-dataset)
- [Workbook Structure](#️-workbook-structure)
- [Parameters & Interactivity](#-parameters--interactivity)
- [Calculated Fields](#-calculated-fields)
- [Key Findings](#-key-findings)
- [Recommendations](#-recommendations)
- [Getting Started](#-getting-started)
- [Repository Structure](#-repository-structure)

---

## 🎯 Project Overview

This project delivers a **single-page interactive Tableau dashboard** built on UK road accident data spanning 2019 to 2022. It enables analysts, policy researchers, and road safety officers to:

- Compare **any two years** side-by-side with dynamic Year-on-Year (YoY) percentage changes
- Drill into casualties by **severity level** — Fatal, Serious, Slight, or All combined
- Understand the role of **vehicle type, road surface, road type, and weather** on accident outcomes
- Spot **geographic accident hotspots** across UK districts via an interactive map

> **Core Question:** *Are UK roads getting safer — and which factors drive the risk?*

---

## 📂 Dataset

**Source file:** `accident data.csv`  
**Encoding:** UTF-8 · **Separator:** `,` · **Locale:** `en_GB`

| # | Column | Type | Description |
|---|--------|------|-------------|
| 1 | `Index` | String | Unique accident record ID |
| 2 | `Accident_Severity` | String | `Fatal` / `Serious` / `Slight` |
| 3 | `Accident Date` | Date | Date the accident occurred |
| 4 | `Latitude` | Float | GPS latitude |
| 5 | `Longitude` | Float | GPS longitude |
| 6 | `Light_Conditions` | String | Daylight, darkness, street lighting, etc. |
| 7 | `District Area` | String | UK administrative district |
| 8 | `Number_of_Casualties` | Integer | Total casualties per accident |
| 9 | `Number_of_Vehicles` | Integer | Vehicles involved |
| 10 | `Road_Surface_Conditions` | String | Dry, Wet, Snow, Ice, Flood, etc. |
| 11 | `Road_Type` | String | Single carriageway, Dual carriageway, Roundabout, etc. |
| 12 | `Urban_or_Rural_Area` | String | `Urban` or `Rural` |
| 13 | `Weather_Conditions` | String | Fine, Rain, Snow, Fog, High winds, etc. |
| 14 | `Vehicle_Type` | String | Car, Motorcycle, Bus, Cyclist, HGV, etc. |

---

## 🏗️ Workbook Structure

**1 Dashboard · 16 Worksheets**

### 📊 KPI Cards & Sparklines

| Worksheet | Description |
|-----------|-------------|
| `TOTAL CASUALTIES` | Headline casualty count for the selected Current Year |
| `TOTAL CASUALTIES SPARKLINE` | Monthly trend line for total casualties |
| `TOTAL ACCIDENTS` | Total accident count (CY) |
| `ACCIDENT SPARKLINE` | Monthly trend line for accidents |
| `FATAL CASUALTIES` | Fatal casualty KPI with YoY % badge |
| `FATAL SPARKLINE` | Fatal trend sparkline |
| `SERIOUS CASUALTIES` | Serious casualty KPI with YoY % badge |
| `SERIOUS SPARKLINE` | Serious trend sparkline |
| `SLIGHT CASUALTIES` | Slight casualty KPI with YoY % badge |
| `SLIGHT SPARKLINE` | Slight trend sparkline |

### 📈 Breakdown Charts

| Worksheet | Chart Type | Dimension |
|-----------|-----------|-----------|
| `VEHICLE TYPE KPI` | Horizontal bar / tile | Vehicle Type |
| `VEHICLE TYPE IMAGES` | Image layer | Vehicle Type (icons) |
| `BY LOCATION` | Map | Latitude / Longitude |
| `ROAD TYPE` | Bar chart | Road Type |
| `ROAD SURFACE` | Bar chart | Road Surface Conditions |
| `WEATHER CONDITION` | Donut chart | Weather Conditions |

---

## ⚙️ Parameters & Interactivity

Three parameters drive all dynamic calculations across the dashboard:

| Parameter | Label | Default | Allowed Values |
|-----------|-------|---------|----------------|
| `Parameter 1` | **CURRENT YEAR** | `2022` | 2019, 2020, 2021, 2022 |
| `CURRENT YEAR (copy)` | **PREVIOUS YEAR** | `2019` | 2019, 2020, 2021, 2022 |
| `Parameter 2` | **SELECT ACCIDENT SEVERITY** | `Fatal` | Fatal, Serious, Slight, All |

> Switching either year parameter instantly recalculates every KPI, YoY %, and sparkline across the entire dashboard. The severity filter applies globally to all breakdown charts.

---

## 🧮 Calculated Fields

All metrics follow a strict **CY → PY → YoY %** pattern for consistency.

### Accident Counts

```
CY ACCIDENTS   = COUNT( IF YEAR([Accident Date]) = [CURRENT YEAR]  THEN [Index] END )
PY ACCIDENTS   = COUNT( IF YEAR([Accident Date]) = [PREVIOUS YEAR] THEN [Index] END )
YOY ACCIDENTS  = ( CY ACCIDENTS - PY ACCIDENTS ) / PY ACCIDENTS
```

### Total Casualties

```
CY CASUALTIES  = SUM( IF YEAR([Accident Date]) = [CURRENT YEAR]  THEN [Number_of_Casualties] END )
PY CASUALTIES  = SUM( IF YEAR([Accident Date]) = [PREVIOUS YEAR] THEN [Number_of_Casualties] END )
YOY CASUALTIES = ( CY CASUALTIES - PY CASUALTIES ) / PY CASUALTIES
```

### Fatal Casualties

```
CY FATAL  = SUM( IF [Accident_Severity]='Fatal' AND YEAR([Accident Date])=[CURRENT YEAR]  THEN [Number_of_Casualties] END )
PY FATAL  = SUM( IF [Accident_Severity]='Fatal' AND YEAR([Accident Date])=[PREVIOUS YEAR] THEN [Number_of_Casualties] END )
YOY FATAL = ( CY FATAL - PY FATAL ) / PY FATAL
```

### Serious & Slight Casualties

Same pattern as Fatal — filter `Accident_Severity = 'Serious'` or `'Slight'` respectively.

### Severity Filter (Global)

```
ACCIDENT SEVERITY FILTER = [Parameter 2] = [Accident_Severity]  OR  [Parameter 2] = "All"
```

This filter is applied across all breakdown charts so that changing the severity parameter redraws every view simultaneously.

---

## 📊 Key Findings

### Overall Trends

- Accident and casualty counts **peaked around 2020** before declining consistently.
- A **sharp and sustained reduction** from 2021 → 2022 across all severity categories.
- Road safety interventions introduced post-2020 show a **measurable, data-backed impact**.

---

### ☠️ Fatal Casualties — YoY Change

| Period | Change | Direction |
|--------|--------|-----------|
| 2019 → 2020 | **+17.81%** | 📈 Increase |
| 2020 → 2021 | **−11.80%** | 📉 Decrease |
| 2021 → 2022 | **−26.40%** | 📉 Sharp Decline |

**Key drivers:** Single carriageways · Dry roads · Fine weather · Cars as dominant vehicle type  
*Clear evidence that human behaviour — not environment — is the primary risk factor.*

---

### 🟠 Serious Casualties — YoY Change

| Period | Change | Direction |
|--------|--------|-----------|
| 2019 → 2020 | **+5.84%** | 📈 Increase |
| 2020 → 2021 | **−4.93%** | 📉 Decrease |
| 2021 → 2022 | **−16.30%** | 📉 Major Decline |

**Key drivers:** Cars and cyclists most affected · Normal (fine) weather conditions · Single carriageways

---

### 🟡 Slight Casualties — YoY Change

| Period | Change | Direction |
|--------|--------|-----------|
| 2019 → 2020 | **+6.69%** | 📈 Increase |
| 2020 → 2021 | **−3.41%** | 📉 Decrease |
| 2021 → 2022 | **−10.82%** | 📉 Significant Reduction |

**Key drivers:** Largest share of total casualties · Dry roads · Single carriageways · Cars

---

### 🌦️ Environmental & Road Insights

| Factor | Finding |
|--------|---------|
| Weather | **75–85%** of all accidents occur in *fine* weather — weather is not the primary risk |
| Road Surface | **Dry roads** dominate accident counts across all severities |
| Road Type | **Single carriageways** are the most dangerous road type in the UK |
| Urban vs Rural | Rural single carriageways are disproportionately represented in fatal accidents |

---

## 💡 Recommendations

| # | Recommendation |
|---|----------------|
| 1 | 🚔 **Increase speed enforcement** on single carriageways — the highest-risk road type |
| 2 | 🏗️ **Improve road infrastructure** — better lighting, lane markings, and signage on high-risk corridors |
| 3 | 🚗 **Promote vehicle safety technologies** — ADAS, automatic emergency braking, lane-keeping assist |
| 4 | 📢 **Strengthen awareness campaigns** targeting car drivers, especially on rural single carriageways |
| 5 | 📍 **Use the geographic heatmap** to identify district-level hotspots for targeted local interventions |

---

## 🚀 Getting Started

### Prerequisites

- [Tableau Desktop](https://www.tableau.com/products/desktop) **2018.1 or later** (workbook schema version `18.1`, built on Tableau `2025.3.2`)
- `accident data.csv` accessible on your local machine

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/uk-road-accident-tableau.git
cd uk-road-accident-tableau
```

### Opening the Workbook

1. Open **`ROAD_ACCIDENT_PROJECT.twb`** in Tableau Desktop.
2. If Tableau shows a data source error, click **Edit Connection** and point it to your local `accident data.csv`.
3. Navigate to the **DASHBOARD** tab.

### Using the Dashboard

| Control | Action |
|---------|--------|
| **CURRENT YEAR** parameter | Select the year you want to analyse |
| **PREVIOUS YEAR** parameter | Select the baseline year for comparison |
| **SELECT ACCIDENT SEVERITY** | Filter between Fatal / Serious / Slight / All |
| Map interaction | Drill into geographic accident clusters by district |
| Sparkline hover | See monthly breakdown trends per metric |

---

## 📁 Repository Structure

```
uk-road-accident-tableau/
│
├── 📊 ROAD_ACCIDENT_PROJECT.twb                   # Tableau workbook (XML)
├── 📄 accident data.csv                           # Source dataset (required)
├── 📑 UK-Road-Accident-Analysis-2019-2022.pptx    # Project presentation
└── 📝 README.md                                   # This file
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **Tableau Desktop 2025.3** | Dashboard design, calculated fields, parameters |
| **CSV** | Raw accident data source |
| **Tableau Parameters** | Dynamic CY / PY year switching and severity filtering |
| **Tableau Calculated Fields** | YoY % formulas, severity-based aggregations |

---

<div align="center">

Made with ❤️ by **Supriyo Maity**

*If you found this project useful, consider giving the repo a ⭐*

</div>
