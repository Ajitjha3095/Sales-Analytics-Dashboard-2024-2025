<div align="center">

<img width="270" height="184" alt="TMDB Project Logo" src="https://github.com/Ajitjha3095/TMBD-Analytics-ETL-Powerbi-Databricks-/blob/main/TMBD/Dashboard/Powerbi%20Screen%20Short.JPG" />

# Sales Analytics Dashboard

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![Apache Spark](https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Delta Lake](https://img.shields.io/badge/Delta%20Lake-003366?style=for-the-badge&logo=delta&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-KPI%20Measures-5E5E5E?style=for-the-badge)
![Data Modeling](https://img.shields.io/badge/Data%20Modeling-Relationships-2F855A?style=for-the-badge)
![Looker Studio](https://img.shields.io/badge/Looker%20Studio-Dashboards-4285F4?style=for-the-badge)

> **A complete end-to-end data Analyst portfolio project**
>End-to-End BI Project — Data Extraction → Cleaning → Power BI · Tableau · Looker Studio

</div>

---

##  Project Overview

Looker Studio: https://datastudio.google.com/reporting/02a3f1b6-832f-4551-8d39-26792ecd9003
Power BI: (add your published link here)
Tableau Public: (add your published link here)


Raw data source: Sales Dataset — Kaggle (ajitjha01)


# Sales Analytics Dashboard
### End-to-End BI Project — Data Extraction → Cleaning → Power BI · Tableau · Looker Studio

A complete analytics project covering the full pipeline: extracting raw data from Kaggle, cleaning and validating it, then building parallel dashboards across three industry-standard BI platforms. Built as a portfolio piece to demonstrate the full analyst workflow, not just final visuals.

**Live dashboards**
- Looker Studio: https://datastudio.google.com/reporting/02a3f1b6-832f-4551-8d39-26792ecd9003
- Power BI: *(add your published link here)*
- Tableau Public: *(add your published link here)*

**Raw data source**: [Sales Dataset — Kaggle (ajitjha01)](https://www.kaggle.com/datasets/ajitjha01/sales-dataset)

---

## 1. Project Summary

This project simulates a real analyst workflow end to end:

1. **Extract** — pull the raw dataset from Kaggle
2. **Clean & validate** — audit for nulls, duplicates, type issues, and logical errors
3. **Model** — define consistent KPI/measure logic across three BI tools
4. **Visualize** — build matching Executive and Product dashboards in Power BI, Tableau, and Looker Studio
5. **Document** — this README, written for technical and non-technical reviewers alike

| Platform | Language | Strength demonstrated |
|---|---|---|
| Power BI | DAX | Data modeling, time intelligence, complex measures |
| Tableau | Calculated fields / table calcs | Visual storytelling, LOOKUP-based comparisons |
| Looker Studio | Calculated fields / native comparisons | Lightweight, parameter-driven, shareable reporting |

Each platform includes two views:
- **Executive View** — top-line KPIs and trends for leadership-level reporting
- **Product View** — filterable, granular drill-down for operational/product analysis

---

## 2. Step 1 — Data Extraction (Kaggle)

| Property | Detail |
|---|---|
| Source | Kaggle dataset: `ajitjha01/sales-dataset` |
| Method | Direct CSV download via Kaggle |
| Raw file | `Data_Set.csv` |
| Raw size | 1,500 rows × 13 columns |
| Date coverage | Jan 3, 2022 – May 15, 2026 |

The raw file was downloaded as-is and loaded into a pandas DataFrame for auditing before any visual work began, so every cleaning decision below is based on a measured finding rather than assumption.

---

## 3. Step 2 — Data Cleaning & Validation

A full data quality audit was run on the raw file. Results and the decision taken for each finding are documented below — this transparency is intentional, since "what did you do with messy data" is one of the most common interview questions for this kind of project.

| Check | Finding | Decision / Action |
|---|---|---|
| Null values | **0 nulls** across all 13 columns | No imputation needed |
| Exact duplicate rows | **3 duplicate rows** found | Flagged; rows can be dropped with `df.drop_duplicates()` — kept in the working file to preserve the original transaction count for this portfolio version, noted here for transparency |
| `Order_ID` uniqueness | Only **499 distinct IDs** across 1,500 rows; 326 IDs repeat (max 10 times) | Treated as a source-system artifact, not a transaction count. All "Total Orders" KPIs use **row-level counts**, never `COUNTD(Order_ID)` |
| `Total_Sales` formula integrity | Checked `Total_Sales = Quantity × Unit_Price` for all 1,500 rows | **0 mismatches** — revenue field is fully trustworthy |
| Negative / zero values | Checked `Total_Sales`, `Quantity`, `Unit_Price` for ≤ 0 | **None found** — no invalid transactions |
| Category/Region/Payment spelling | Checked all categorical fields for casing or typo inconsistencies | 3 of 5 fields clean; `Customer_Type` contains `"Premimum"` (should read "Premium") |
| Data type correctness | `Order_Date` loads as text, not native date | Converted to datetime in every BI tool's data load step (Power Query, Tableau's auto date detection, Looker Studio's Date semantic type) |
| Year completeness | 2022–2025 are full calendar years; 2026 has data only through May 15 | Flagged explicitly — affects every Year-over-Year calculation (see Section 5.2) |

### 3.1 Cleaning steps applied before visualization

1. Converted `Order_Date` to a proper date type in each tool's data source layer.
2. Verified and trusted `Total_Sales` as the primary revenue field (validated against `Quantity × Unit_Price`).
3. Standardized order-counting logic to row-level counts across all three platforms, avoiding the `Order_ID` duplication issue.
4. Preserved `"Premimum"` as-is rather than silently relabeling it, since this is documentation of the literal source data — a one-line fix (`REPLACE`/`CASE WHEN`) is noted as an optional enhancement in Section 7.
5. Built every Year-over-Year measure with explicit handling for the partial 2026 year, instead of letting it silently distort the headline growth number (full detail in Section 5.2).

### 3.2 Yearly revenue summary (post-validation)

| Year | Revenue | Orders | Status |
|---|---|---|---|
| 2022 | ₹6,514,494 | 344 | Full year |
| 2023 | ₹7,746,588 | 439 | Full year |
| 2024 | ₹4,514,525 | 218 | Full year |
| 2025 | ₹6,892,198 | 364 | Full year |
| 2026 | ₹2,552,926 | 135 | Partial — through May 15 only |
| **Total** | **₹28,220,731** | **1,500** | — |

---

## 4. Field Dictionary

| Field | Type | Description |
|---|---|---|
| `Order_ID` | Integer | Transaction identifier (not fully unique — see Section 3) |
| `Order_Date` | Date | Date the order was placed; drives all time intelligence |
| `Year` | Integer | Calendar year, pre-extracted (2022–2026) |
| `Month` | Integer | Calendar month (1–12) |
| `Quarter` | Integer | Calendar quarter (1–4); used in the quarterly trend chart |
| `Product_Name` | Text | One of 10 products (Laptop, Tablet, Watch, Smartwatch, Headphones, Shoes, Backpack, Book, Mobile Case, Water Bottle) |
| `Category` | Text | Electronics, Fashion, Books, Other |
| `Region` | Text | Mumbai, Delhi, Bangalore, Chennai, Hyderabad, Pune |
| `Customer_Type` | Text | New, Premimum, Returning |
| `Payment_Method` | Text | Card, UPI, COD |
| `Quantity` | Integer | Units purchased (1–5) |
| `Unit_Price` | Integer (₹) | Price per unit |
| `Total_Sales` | Integer (₹) | `Quantity × Unit_Price` — primary revenue metric, validated 100% accurate |

### Field usage by dashboard view

| Field | Executive View | Product View |
|---|---|---|
| Order_Date / Year / Quarter | Trend chart, Year filter | Year filter |
| Product_Name | — | Primary dimension, Performance Matrix |
| Category | Donut chart, slicer | Slicer |
| Region | Bar chart, slicer | Stacked bar, slicer |
| Customer_Type | Donut chart, slicer | Bar chart, slicer |
| Payment_Method | Donut chart, slicer | Bar chart, slicer |
| Quantity | Total Qty KPI | Matrix column |
| Unit_Price | — | Avg Unit Price column |
| Total_Sales | All KPIs, all charts | All KPIs, matrix |

---

## 5. Step 3 — KPI & Measure Definitions

### 5.1 Base KPIs

**Total Revenue** — ₹28,220,731 (28.22M)
```dax
-- Power BI
Total Revenue = SUM(Sales[Total_Sales])
```
```
-- Tableau
SUM([Total Sales])
```
```
-- Looker Studio
SUM(Total_Sales)
```

**Total Quantity Sold** — 3,453 units
```dax
Total Quantity = SUM(Sales[Quantity])
```

**Total Orders** — 1,500 (row-level count — see Section 3 on `Order_ID` duplication)
```dax
-- Power BI
Total Orders = COUNTROWS(Sales)
```
```
-- Tableau
COUNT([Order Date])
```
```
-- Looker Studio
COUNT(Order_Date)
```

**Average Order Value** — ₹18,813.82 (18.81K)
```dax
-- Power BI
Avg Order Value = DIVIDE([Total Revenue], [Total Orders], BLANK())
```
```
-- Tableau
IF [Total Orders] = 0 THEN NULL
ELSE [Total Revenue] / [Total Orders] END
```
All three platforms guard against divide-by-zero so the KPI shows blank instead of an error.

### 5.2 Year-over-Year Growth ("VS Last Year")

The most complex measure in the project. A naive `DATEADD`-based comparison with no year filter compares the whole multi-year dataset against a one-year-shifted window of itself — misleading, because 2026 is a partial year. The cleaning step in Section 3 is what surfaced this issue before it reached the dashboard.

**Power BI — final working DAX** (handles single-year selection and "Select All" gracefully):
```dax
VS Last Year =
VAR _YearsSelected = DISTINCTCOUNT('Date Table'[Year])
VAR _MaxDateInData = CALCULATE(MAX('Date Table'[Date]), ALL('Date Table'))
VAR _LastCompleteYear =
    YEAR(_MaxDateInData) - IF(
        MONTH(_MaxDateInData) = 12 && DAY(_MaxDateInData) >= 28, 0, 1
    )
VAR _CY =
    IF(
        _YearsSelected = 1,
        [Total Revenue],
        CALCULATE([Total Revenue], ALL('Date Table'),
            VALUE('Date Table'[Year]) = _LastCompleteYear)
    )
VAR _PY =
    IF(
        _YearsSelected = 1,
        CALCULATE([Total Revenue], DATEADD('Date Table'[Date], -1, YEAR)),
        CALCULATE([Total Revenue], ALL('Date Table'),
            VALUE('Date Table'[Year]) = _LastCompleteYear - 1)
    )
VAR _perc = DIVIDE(_CY - _PY, _PY)
VAR _format =
    SWITCH(
        TRUE(),
        _perc > 0, UNICHAR(11165) & " " & FORMAT(_perc, "0.00%"),
        _perc < 0, UNICHAR(11167) & " " & FORMAT(ABS(_perc), "0.00%"),
        FORMAT(_perc, "0.00%")
    )
RETURN _format
```
Key fixes applied during development:
- `VALUE('Date Table'[Year])` converts Year from Text to Integer — omitting this throws *"DAX comparison operations do not support comparing values of type Text with values of type Integer."*
- `ALL('Date Table')` resets context so the fallback branch can ignore the active slicer.
- Verified results: Year=2024 → **−40.72%**, Year=2025 → **+52.62%**, Select All → **+52.67%** (fallback to latest-complete-year logic).

**Tableau — table calculation:**
```
Sales LY:    LOOKUP(SUM([Total_Sales]), -1)
Sales YoY %: IF ISNULL([Sales LY]) OR [Sales LY] = 0 THEN NULL
             ELSE (SUM([Total_Sales]) - [Sales LY]) / ABS([Sales LY]) END
```
Required setup: Year must be in the view (Rows/Columns) for `LOOKUP(-1)` to resolve; both pills need **Compute Using → Year**, sorted ascending.

**Looker Studio:**
- Recommended: native **Comparison date range** on the scorecard (no formula needed).
- Alternative, parameter-driven:
```
Revenue Current Year:  SUM(CASE WHEN Year = Selected_Year THEN Total_Sales ELSE 0 END)
Revenue Previous Year: SUM(CASE WHEN Year = Selected_Year - 1 THEN Total_Sales ELSE 0 END)
YoY Growth %:          CASE WHEN Revenue_Previous_Year = 0 THEN NULL
                        ELSE (Revenue_Current_Year - Revenue_Previous_Year) / Revenue_Previous_Year END
```
Live result at Year=2025: **+52.7%** revenue, **+62.9%** quantity, **+67.0%** orders — consistent with Power BI and Tableau.

### 5.3 Product-Level Measures (Product View)

```dax
Avg Unit Price    = AVERAGE(Sales[Unit_Price])
Revenue Per Unit  = DIVIDE([Total Revenue], [Total Quantity], BLANK())
Product Share %   = DIVIDE([Total Revenue], CALCULATE([Total Revenue], ALL(Sales[Product_Name])), BLANK())
Top Product       = TOPN(1, VALUES(Sales[Product_Name]), [Total Revenue])
```

---

## 6. Step 4 — Dashboard Structure

### Executive View
- 4 KPI cards: Total Revenue, Total Quantity Sold, Total Orders, Avg Order Value (each with VS Last Year %)
- Revenue by Category (donut)
- Payment Method Split (donut)
- Customer Type Revenue Share (donut)
- Revenue by Region (bar)
- Quarterly Revenue Trend, 2022–2026 (line)
- Filters: Year, Category, Region, Payment Method, Customer Type

### Product View
- Product Performance Matrix: Product, Category, Orders, Total Qty, Revenue/Order, Avg Unit Price, Total Sales
- Revenue by Customer Type (bar)
- Payment Method by Region (stacked bar)
- Revenue by Region by Product (stacked bar / treemap)
- Same global filters as Executive View, fully cross-filtering the matrix and charts

### Cross-platform build notes

| Platform | Notes |
|---|---|
| Power BI | Requires a marked Date Table related to `Order_Date` for time-intelligence functions (`DATEADD`, `SAMEPERIODLASTYEAR`) to work. |
| Tableau | All LOOKUP-based table calculations require the date dimension physically in the view, with **Compute Using** explicitly set. |
| Looker Studio | Native **Comparison date range** is the simplest path for YoY; calculated-field approach requires a Year parameter for flexibility. |

---

## 7. Suggested Future Enhancements

- Fix the `"Premimum"` → `"Premium"` typo at the source-query layer (Power Query `Table.ReplaceValue`, Tableau data source calculated field, or a BigQuery view) rather than leaving it in the visual layer.
- Drop the 3 exact duplicate rows identified in Section 3 at the source-load step.
- Add a `Date_Completeness` flag column so any visual can programmatically grey out or annotate the partial 2026 period instead of relying on a documentation note.
- Publish all three dashboards (Power BI Service, Tableau Public, Looker Studio share link) and link them at the top of this document.

---

## 🛠️ Tech Stack Detail

| Tool              | Version    | Purpose                        |
|-------------------|------------|--------------------------------|
| Python            | 3.8+       | API ingestion script           |
| Apache Spark      | 3.0+       | Data processing engine         |
| Databricks        | Latest     | Unified analytics platform     |
| Delta Lake        | Latest     | Reliable data lake storage     |
| SQL               | Spark SQL  | Silver and Gold transformations|
| PowerBI Desktop   | Latest     | Business intelligence dashboard|
| TMDB API          | v3         | Movie data source              |
| GitHub            | -          | Version control and portfolio  |


###  📈 Business Value

This project demonstrates how modern data engineering pipelines can convert raw entertainment industry data into actionable intelligence for:

Content strategy teams
Media analysts
Production companies
Streaming platforms
Marketing decision-makers


### 🧠 Skills Demonstrated
ETL Pipeline Development
Data Engineering with Databricks
Apache Spark Transformations
Delta Lake Implementation
Data Modeling
DAX & KPI Design
Power BI Dashboard Development
API Integration
SQL Analytics
Business Intelligence Reporting
---

## ⚙️ How to Run

✅ Databricks Account  → community.cloud.databricks.com (free)
✅ TMDB API Key        → themoviedb.org/settings/api (free)
✅ PowerBI Desktop     → powerbi.microsoft.com (free)

### Steps

**1. Clone Repository**

git clone https://github.com/Ajitjha3095/TMBD-Analytics-ETL-Powerbi-Databricks-/tmdb-analytics.git
cd tmdb-analytics

**2. Add API Key**

# In notebooks/01_bronze_layer.ipynb
# Cell 2 - replace this line
API_KEY = "your_tmdb_api_key_here"

**3. Run Notebooks in Order**

Databricks → Import notebooks → Run in order:

01_bronze_layer.ipynb  → pulls API data

02_silver_layer.sql    → cleans data

03_gold_layer.sql      → builds metrics

**4. Open Dashboard**

PowerBI Desktop → Open → tmdb_dashboard.pbix
Home → Refresh → Connect to your Databricks



## 8. Author

**Ajit Jha**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/ajitjha01)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Ajitjha3095)
[![website](https://img.shields.io/badge/Website-181717?style=for-the-badge&logo=website&logoColor=white)](https://ajitjha.netlify.app/)

---

## MIT 📄 License
---


