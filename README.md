# 📊 Superstore Sales Dashboard

## Project Overview
Analysis of the Kaggle Superstore dataset using Microsoft Excel.
The goal was to clean, analyse, and visualise sales data to identify 
revenue trends, regional performance, and profit drivers.

---

## Dataset
- **Source:** [Kaggle — Superstore Sales Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)
- **Original format:** CSV
- **Period covered:** 03/01/2014 to 30/12/2017
- **Rows after cleaning:** 5,009 unique orders

---

## Phase 1 — Data Preparation

### File Conversion
Original data was provided as CSV. Converted to `.xlsx` immediately to preserve 
formatting, formulas, and prevent encoding issues on re-open.

- `Superstore_RAW.csv` — original downloaded file, never modified
- `Superstore_CLEAN.xlsx` — working file for all cleaning and analysis

### Removing Duplicates
Duplicates were removed based on **Order ID** column, as each order should appear only once.

- Duplicate rows removed: **4,985**
- Unique rows remaining: **5,009**
- Row ID column contains gaps after removal — this is expected and confirms 
  duplicates were successfully removed

### Column Formatting
The following columns were formatted before running any analysis:

| Column | Format Applied |
|---|---|
| Order Date | Date (DD/MM/YYYY) |
| Ship Date | Date (DD/MM/YYYY) |
| Sales | Currency |
| Profit | Currency |
| Quantity | Number |
| Discount | Percentage |

### Date Cleaning
The date columns required significant cleaning due to two separate issues:

1. **Mixed formats** — dataset contained a combination of real dates and text strings 
   in the same column
2. **American date format** — original data used MM/DD/YYYY which was inconsistent 
   with regional settings (DD/MM/YYYY). Dates were standardised to DD/MM/YYYY.

The following formula was used to handle all date cases in a single step:
```
=IF(ISNUMBER(C2), DATE(YEAR(C2), DAY(C2), MONTH(C2)), 
DATE(MID(C2,FIND("/",C2,FIND("/",C2)+1)+1,4), 
LEFT(C2,FIND("/",C2)-1), 
MID(C2,FIND("/",C2)+1,FIND("/",C2,FIND("/",C2)+1)-FIND("/",C2)-1)))
```

This formula handles three scenarios:
- Real dates stored as numbers → swaps day and month to correct MM/DD to DD/MM
- Text dates with single digit month e.g. `4/15/2017` → extracts using slash position
- Text dates with leading zero e.g. `06/16/2016` → extracts using slash position

### Added Columns
Two new calculated columns were added to support analysis:

- **Profit Margin** → `=Profit/Sales` formatted as percentage
- **Shipping Days** → `=Ship Date - Order Date` formatted as number

---

## Phase 2 — Data Exploration

Before building any analysis the following checks were completed to validate data quality:

| Check | Result | Status |
|---|---|---|
| Total rows | 5,009 | ✅ |
| Unique Order IDs | 5,009 | ✅ Matches total rows — no duplicates |
| Date range | 03/01/2014 → 30/12/2017 | ✅ |
| Unique customers | 793 | ✅ |
| Blank cells | 0 | ✅ No missing values |

### Rows by Category
| Category | Rows | % of Total |
|---|---|---|
| Office Supplies | 3,043 | 60.7% |
| Furniture | 1,102 | 22.0% |
| Technology | 864 | 17.3% |
| **Total** | **5,009** | **100%** ✅ |

> Category totals sum exactly to 5,009 — confirming no blank or misspelled 
> values exist in the Category column.

---

## Phase 3 — Analysis & Key Findings

### 📅 Monthly Revenue & Profit Trends

**Business is growing year on year:**

| Year | Revenue | Profit |
|---|---|---|
| 2014 | $207,996 | $20,807 |
| 2015 | $221,879 | $24,544 |
| 2016 | $312,727 | $42,456 |
| 2017 | $357,260 | $44,709 |

Revenue grew from $208k to $357k — **72% growth over 4 years.**

**Negative profit months:**

Three months recorded negative overall profit — all caused by excessive discounting:

- **July 2014 (-$2,199)** — driven by a single order with 80% discount generating 
  -$3,701 loss, wiping out all positive profit from remaining July orders combined
- **January 2015 (-$1,963)** — almost entirely caused by one bulk order of 13 
  Furniture Tables at 40% discount (-$1,862 loss — 95% of monthly loss)
- **March 2016 (-$228)** — no single catastrophic order but multiple moderately 
  to heavily discounted orders (20%-80%) whose combined losses outweighed all 
  profitable orders — a "death by a thousand cuts" pattern

**Seasonal patterns:**
- Q4 (October-December) is consistently the strongest quarter every year — 
  classic retail holiday seasonality
- February consistently shows the lowest or near-lowest monthly revenue each year — 
  typical post-holiday retail slowdown
- High revenue months do not always produce high profit — December 2015 generated 
  4x more revenue than August 2015 but had less than half the profit margin 
  (8.3% vs 20.3%), likely driven by seasonal discounting

**Notable outliers:**
- **December 2016 ($58,606)** — significantly higher than any other December, 
  driven by a mix of large high-margin orders at 0% discount and heavily discounted 
  orders (up to 80% discount, -165% margin). Month remained profitable overall 
  because high-margin orders outweighed losses from excessive discounting
- **February 2016 ($17,603)** — 2.4x higher than any other February. Investigation 
  revealed a single bulk order of 5 HP Designjet printers ($8,749.95) accounted 
  for ~50% of that month's revenue — classified as an outlier, not a genuine trend

---

### 🌎 Sales by Region

| Region | Revenue | Profit | Avg Margin | Avg Discount |
|---|---|---|---|---|
| Central | $246,315 | $11,357 | -12% | 25% |
| East | $330,695 | $52,446 | 17% | 15% |
| South | $190,409 | $21,293 | 16% | 15% |
| West | $332,444 | $47,420 | 23% | 10% |

**Key finding:** Strong inverse relationship between discounting and profitability 
across all regions. Central region applies the highest average discount (25%) and 
is the only region with a negative profit margin (-12%). West region applies the 
lowest average discount (10%) and achieves the highest profit margin (23%). 
This suggests the company should urgently review its Central region discounting strategy.

---

### 📦 Sales by Category

| Category | Revenue | Profit | Avg Discount |
|---|---|---|---|
| Technology | $380,641 | $70,158 | 13% |
| Furniture | $373,505 | $7,569 | 17% |
| Office Supplies | $345,716 | $54,789 | 16% |

**Key findings:**
- Technology and Furniture have near-identical revenue (~$373-380k) but Technology 
  generates **9x more profit** ($70k vs $7.5k) — driven by lower discounting (13% vs 17%)
- The inverse relationship between discounting and profitability is consistent at 
  every level of analysis — by region, by category, and by sub-category
- **Tables sub-category** loses -$10,997 despite $104k revenue — the most 
  loss-making sub-category, driven by 27% average discount
- **Furniture's 17% average discount masks a deeper problem** — orders with 
  negative profit in Furniture had an average discount of 37% (more than double 
  the category average). The overall average is distorted by many small low-discount 
  orders, hiding the true damage caused by heavily discounted orders.
  > Calculated using `=SUBTOTAL(1,...)` on filtered negative profit rows only — 
  > ensuring the average reflected loss-making orders rather than the entire category

---

### 💸 Discount vs Profit

| Discount Band | Profit | Avg Margin | Orders |
|---|---|---|---|
| 0%-20% | $155,130 | 33% | 2,482 |
| 20%-40% | $39,828 | 17% | 1,947 |
| 40%-60% | -$25,458 | -32% | 148 |
| 60%-80% | -$36,985 | -115% | 432 |

**Key finding:** Orders with discounts above 40% are systematically loss-making. 
580 orders (11.6% of all orders) fall into discount bands of 40%-80%, generating 
a combined loss of **-$62,443**. Without these orders total profit would be 
**$194,959 — 47% higher** than actual profit of $132,516. A strict discount cap 
of 40% could dramatically improve profitability.

**Overall:** 905 orders (18% of all orders) have negative profit — almost all 
driven by discounts above 40%.

---

### 👥 Top 10 Customers by Revenue

| Customer | Revenue | Profit | Orders | Avg Margin |
|---|---|---|---|---|
| Adrian Barton | $12,121 | $5,065 | 10 | 12% |
| Hunter Lopez | $11,714 | $5,367 | 6 | 32% |
| Tom Ashbrook | $11,649 | $3,853 | 4 | -6% |
| Sanjit Engle | $10,640 | $2,651 | 11 | 17% |
| Bill Shonely | $10,351 | $2,558 | 5 | 18% |
| Christopher Conant | $8,953 | $1,156 | 5 | -17% |
| Grant Thornton | $8,175 | -$3,781 | 3 | 7% |
| Tom Boeckenhauer | $7,292 | $2,327 | 7 | 31% |
| Joseph Holt | $6,696 | -$1,110 | 6 | 19% |
| Maria Etezadi | $6,221 | $992 | 10 | -25% |

**Key findings:**
- 3 of the top 10 customers by revenue are loss-making: Grant Thornton (-$3,780), 
  Joseph Holt (-$1,110), and Maria Etezadi (-$991). Serving these customers costs 
  the business more than not serving them
- Hunter Lopez and Tom Boeckenhauer generate the best returns — high revenue with 
  31-32% profit margins and fewer orders, making them the most efficiently 
  profitable customers

---

## Business Recommendations

Based on the analysis, three actionable recommendations emerge:

1. **Cap discounts at 40%** — orders above this threshold are systematically 
   loss-making across all categories and regions. Implementing a hard cap could 
   increase total profit by up to 47%

2. **Review Central region discounting strategy** — Central applies 25% average 
   discount vs 10% in West, resulting in the only region with negative profit margin. 
   Aligning Central discounting closer to West levels could significantly improve 
   regional profitability

3. **Review pricing for loss-making customers** — 3 of the top 10 revenue customers 
   generate negative profit. A customer profitability review should identify whether 
   these relationships can be made profitable or should be discontinued

---

## Tools Used
- **Microsoft Excel**
  - Data cleaning and formatting
  - Pivot Tables
  - Charts and Dashboard
  - Conditional Formatting
  - Custom date cleaning formulas (`IF`, `ISNUMBER`, `FIND`, `MID`, `LEFT`, `RIGHT`, `DATE`)
  - `SUBTOTAL(1,...)` for filtered average calculations
  - `SUMPRODUCT(1/COUNTIF(...))` for unique customer count

---

## Files
| File | Description |
|---|---|
| [Superstore_RAW.csv](Superstore_RAW.csv) | Original downloaded file, unchanged |
| [Superstore_CLEAN.xlsx](Superstore_CLEAN.xlsx) | Cleaned dataset, analysis and dashboard |

---

## Dashboard Preview
![Dashboard Preview](dashboard.png)
