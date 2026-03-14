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
3. **CSV encoding issue** — slash `/` characters were imported incorrectly from the 
   CSV file. Used `=CODE()` function to identify the incorrect character, then used 
   Find & Replace to correct it across all date columns.

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

## Tools Used
- **Microsoft Excel**
  - Data cleaning and formatting
  - Pivot Tables
  - Charts and Dashboard
  - Conditional Formatting
  - Custom date cleaning formulas

---

## Files
| File | Description |
|---|---|
| `Superstore_RAW.csv` | Original downloaded file, unchanged |
| `Superstore_CLEAN.xlsx` | Cleaned dataset, analysis and dashboard |

---

## Dashboard Preview
*Screenshot coming soon*

---

## Key Findings
*To be updated after analysis and dashboard are complete*

