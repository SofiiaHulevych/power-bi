# 🏠 Airbnb Global Market Insights

> An end-to-end Power BI analytical report for an investor exploring the global short-term rental market — covering pricing dynamics, host performance, review trends, and city-level comfort for travelers.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Time%20Intelligence-blue?style=for-the-badge)
![Data Sources](https://img.shields.io/badge/7%20Data%20Sources-real%20world-green?style=for-the-badge)
![Cities](https://img.shields.io/badge/10%20Cities-6%20Continents-orange?style=for-the-badge)

---

## 📌 Project Overview

Built as a **capstone project** for the DAN IT Data Analytics course. The report answers four key investor questions:

- Which cities offer the best pricing opportunities?
- How do Superhosts compare to regular hosts in performance and pricing?
- What seasonal trends exist in guest review activity?
- Which city offers the best overall experience for travelers?

**Dataset:** 250,000+ listings · 5M+ reviews · 10 global cities · 6 continents

---

## 🌍 Cities Covered

| City | Country | Continent |
|------|---------|-----------|
| Bangkok | Thailand | Asia |
| Cape Town | South Africa | Africa |
| Hong Kong | China | Asia |
| Istanbul | Turkey | Europe/Asia |
| Mexico City | Mexico | North America |
| New York | USA | North America |
| Paris | France | Europe |
| Rio de Janeiro | Brazil | South America |
| Rome | Italy | Europe |
| Sydney | Australia | Australia |

---
| Table | Role | Key Columns |
|-------|------|-------------|
| `factListings` | Fact | price, room_type, host_id, city, lat/lon, availability, rating |
| `factReviews` | Fact | listing_id, date, reviewer_id |
| `dimHosts` | Dimension | host_name, superhost status, response rate, listings count |
| `dimCities` | Dimension | country, continent, coordinates, currency |
| `dim_room_types` | Dimension | room type with Ukrainian labels, is_private flag |
| `dimComfort` | Dimension | Numbeo indices: Safety, Health Care, Cost of Living, Quality of Life |
| `dimCalendar` | Dimension | DAX date table: year, quarter, month, week, is_weekend |
| `dimCurrency` | Technical | NBU official exchange rates (UAH/USD/EUR) |
| `_Measures` | Technical | All DAX measures centralized |

---

## 📐 Key DAX Measures

```dax
-- Base
# Listings        = COUNTROWS(factListings)
# Hosts           = DISTINCTCOUNT(factListings[host_id])
# Reviews         = COUNTROWS(factReviews)
Avg Price         = AVERAGE(factListings[price])
% Superhost       = DIVIDE(CALCULATE([# Hosts], factListings[host_is_superhost]="Yes"), [# Hosts])

-- CALCULATE + FILTER
Avg Price Superhost = CALCULATE([Avg Price], factListings[host_is_superhost] = "Yes")
Avg Price Regular   = CALCULATE([Avg Price], factListings[host_is_superhost] = "No")

-- Time Intelligence
Reviews YoY %      = DIVIDE([# Reviews] - CALCULATE([# Reviews], SAMEPERIODLASTYEAR(dimCalendar[date])),
                     CALCULATE([# Reviews], SAMEPERIODLASTYEAR(dimCalendar[date])))
Reviews TOTALYTD   = TOTALYTD([# Reviews], dimCalendar[date])
Reviews prev year  = CALCULATE([# Reviews], PREVIOUSYEAR(dimCalendar[date]))
Reviews QTD        = TOTALQTD([# Reviews], dimCalendar[date])

-- Comfort Index (weighted)
Comfort Index =
VAR _city = SELECTEDVALUE(dimCities[city])
RETURN DIVIDE(
    CALCULATE(AVERAGE(dimComfort[Safety Index]),          dimComfort[city]=_city) * 0.30 +
    CALCULATE(AVERAGE(dimComfort[Health Care Index]),     dimComfort[city]=_city) * 0.25 +
    CALCULATE(AVERAGE(dimComfort[Property Price to Income Ratio]), dimComfort[city]=_city) * 0.25 +
    CALCULATE(AVERAGE(dimComfort[Quality of Life Index]), dimComfort[city]=_city) * 0.20, 1)
```

---

## 📄 Report Pages

### 1. 🗺️ Menu
Navigation hub with buttons to all report sections.

### 2. 🌐 Overview
- KPI cards: # Listings, # Hosts, # Reviews, Avg Price
- World map with bubble size proportional to listing count
- Top 5 cities bar chart
- City / Country / Continent slicers
- UAH / USD / EUR currency switcher

### 3. 💰 Pricing
- Average price trend by room type
- Top listings table with conditional formatting
- Bookmarks toggle: chart view ↔ table view
- Field parameters: switch between Price / Rating / # Listings on Y-axis
- Price category slicer (Budget / Mid / Premium / Luxury)

### 4. 👤 Hosts
- Superhost % KPI
- Top 5 hosts by listing count
- Superhost vs Regular price comparison
- Host search filter
- Drillthrough to Host Details page

### 5. 📅 Reviews
- YoY % line chart (dual axis: # Reviews + YoY %)
- TOTALYTD cumulative area chart
- Current vs previous year comparison
- Date hierarchy drill-down: Year → Quarter → Month

### 6. 🔍 Host Details *(Drillthrough)*
Individual host profile with listings breakdown table.

### 7. 💬 Tooltip *(Report Page Tooltip)*
Custom tooltip for map visual: Avg Price · # Listings · # Reviews

---

## 🔐 Row-Level Security

| Role | Filter Logic |
|------|-------------|
| `Paris Manager` | City = "Paris" |
| `Asia Manager` | Continent = "Asia" |
| `Europe Budget Analyst` | Continent = "Europe" AND price_category IN ("Budget", "Mid") |

---

## ⚙️ Power Query Highlights

- Replace Values — `t/f` → `Yes/No` for boolean fields
- Conditional Column — `price_category` buckets based on EUR thresholds
- Unpivot — Numbeo wide-format → long for `dimComfort`
- Reference Query — `dimHosts` derived from `factListings`
- Web Connection — NBU exchange rates loaded live
- Merge — city currency codes added to `factListings`

---

## 🛠️ Tech Stack

`Power BI Desktop` · `Power Query (M)` · `DAX` · `GitHub`

**Data Sources:** Maven Analytics · World Bank · NBU · Numbeo · Enter Data

---

*Data Analytics Course · Capstone Project · 2026*
## 🗂️ Data Model

Star schema with **2 fact tables**, **5 dimension tables**, and **3 technical tables**.
