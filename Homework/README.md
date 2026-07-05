# 📊 Sales & Purchase Analytics Dashboard

> A multi-page Power BI report analyzing retail sales performance, purchase costs, and profitability — with advanced DAX time intelligence, plan vs. actual tracking, and seasonal analysis.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Advanced-blue?style=for-the-badge)
![Tables](https://img.shields.io/badge/16%20Tables-Star%20Schema-purple?style=for-the-badge)

---

## 📌 Project Overview

A homework assignment for the DAN IT Data Analytics course. The report covers two analytical perspectives:

- **City Sales Dashboard** — geographic breakdown of sales volume and purchase costs across store locations
- **Sales & Purchase Analysis** — DAX-driven analysis with time intelligence, plan tracking, and provider/category purchase breakdown

---

## 🗂️ Data Model

16 tables including fact tables, dimension tables, and technical helper tables.

| Table | Type | Description |
|-------|------|-------------|
| `Sale` | Fact | Individual sales transactions |
| `Purchase` | Fact | Procurement records |
| `All Sales` | Aggregated | Combined sales with all DAX measures |
| `Operating cost` | Fact | Operational expense data |
| `Cost plan` | Reference | Planned cost targets |
| `Sales plan` | Reference | Planned sales targets |
| `DimProduct` | Dimension | Product name, category, country of origin |
| `DimStore` | Dimension | Store location and city |
| `DimCustomers` | Dimension | Customer profiles |
| `DimProviders` | Dimension | Supplier information |
| `DimCalendar` | Dimension | DAX date table: year, quarter, month, day |
| `Exchange Rates` | Reference | Currency exchange data |
| `DimCurrency` | Technical | Currency selector |
| `% Summer` / `% notSummer` | Technical | Seasonal weight slicers |

---

## 📐 Key DAX Measures

```dax
-- Time Intelligence
salesPantsQTD        = TOTALQTD([salesPants], Календар[Date])
salesPantsPrevDay    = CALCULATE([salesPants], DATEADD(Календар[Date], -1, DAY))
salesPantsDiffDay    = [salesPants] - [salesPantsPrevDay]
salesPantsDiffDayPct = DIVIDE([salesPantsDiffDay], [salesPantsPrevDay])
salesPantsLY         = CALCULATE([salesPants], SAMEPERIODLASTYEAR(Календар[Date]))
salesPantsLYYTD      = TOTALYTD([salesPants], SAMEPERIODLASTYEAR(Календар[Date]))
aver3month period    = AVERAGEX(
                         DATESINPERIOD(Календар[Date], LASTDATE(Календар[Date]), -3, MONTH),
                         [salesPants])

-- Plan vs Actual
plan7%               = [salesPantsPlan] * 1.07
plan7/10%            = [salesPantsPlan] * 1.07 / 1.10

-- Purchase Analysis
PurchaseShare        = DIVIDE([PurchaseTotal by provider], [PurchaseTotal])
ProfitLviv2018       -- filtered profit: city = Lviv, year = 2018
```

---

## 📄 Report Pages

### 1. 🏙️ City Sales Dashboard
- Advanced city slicer, date range picker, customer and category filters
- Matrix: sales quantity by City × Quarter × Year
- Combo chart: Revenue + Purchase Cost by Month / Year / Category
- Treemap: purchase cost breakdown by product name

### 2. 📈 Sales & Purchase Analysis
- Full time intelligence matrix: QTD · Prev Day · Day Diff % · LY · Plan · LYTD · 3-month avg · Plan +7% · Plan +7/10%
- KPI card: Profit for Lviv store, 2018
- Matrix: provider × category purchase share %
- Treemap: purchase total by country of origin
- Seasonal slicers: Summer weight / Non-summer weight

---

## ⚙️ Power Query Highlights

- Append / Merge — combining Sale and Purchase into unified `All Sales`
- Conditional Columns — seasonal flags and category groupings
- Unpivot — wide plan tables restructured to long format
- Reference Queries — dimension tables derived from fact sources
- Change Type with Locale — decimal parsing for Ukrainian locale
- Fill Down — handling sparse hierarchical data

---

## 🛠️ Tech Stack

`Power BI Desktop` · `Power Query (M)` · `DAX`

---

*Data Analytics Course · Homework Assignment · 2026*
