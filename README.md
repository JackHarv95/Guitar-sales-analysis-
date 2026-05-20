#  Guitar Handedness Representation Analysis

**Tool:** Power BI (DAX) · **Language:** Python · **Domain:** Retail / Music Industry

---

## Overview

This project investigates whether **left-handed guitarists are underrepresented in guitar retail sales** relative to their estimated share of the general population (~10%).

Using a dataset of 515 raw guitar sales records across 11 major manufacturers and 6 guitar types, I cleaned the data with Python and built an interactive Power BI dashboard to explore left/right-handed sales splits — with a dynamic benchmark slider allowing the assumed left-handed population percentage to be adjusted.

---

## Key Finding

> **Left-handed guitars account for ~5.2% of units sold** across the cleaned dataset — roughly half the 10% population benchmark — suggesting meaningful underrepresentation in retail stock and sales.

---
![image](https://github.com/JackHarv95/Guitar-sales-analysis-/blob/main/docs/Screenshot%202026-05-20%20at%2019.26.44.png?raw=true)


## Dashboard Features

The Power BI dashboard includes:

- **Left vs Right Sales KPIs** — unit counts and % share at a glance
- **Benchmark Slider** — adjustable population assumption (default: 10%) to contextualise the gap
- **Manufacturer Filter** — drill into brands: Fender, Gibson, Ibanez, PRS, Martin, and more
- **Guitar Type Filter** — segment by Electric, Acoustic, Bass, Classical, Semi-Hollow
- **Sales & Price Comparison** — average price and units sold by handedness
- **Review Score Analysis** — whether left-handed models differ in customer ratings

---

## Dataset

| Field | Description |
|---|---|
| `brand` | Guitar manufacturer (e.g. Fender, Gibson, Ibanez) |
| `type` | Guitar category (Electric, Acoustic, Bass, Classical, Semi-Hollow) |
| `handedness` | Left or Right |
| `sales` | Units sold |
| `price` | Retail price (GBP) |
| `review_score` | Customer review score (1.0–5.0) |

**Raw:** 515 rows · **Cleaned:** 445 rows (70 removed — duplicates, nulls, invalid values)

---

## Data Cleaning (Python)

The raw dataset contained several quality issues

| Issue | Fix |
|---|---|
| Duplicate rows | `drop_duplicates()` |
| Inconsistent handedness labels (`l`, `lh`, `left-handed`, etc.) | Standardised to `Left` / `Right` |
| Null values in `handedness`, `sales`, `price` | Rows dropped |
| Null `review_score` | Imputed with median |
| Null `brand` / `type` | Filled as `Unknown` |
| Negative sales values | `abs()` applied |
| Extreme price outliers | Clipped at mean + 3× std dev |
| Review scores outside 1–5 range | Clipped to `[1.0, 5.0]` |
| Inconsistent string casing | `.str.title()` on `brand` and `type` |

---

## Repo Structure

```
guitar-handedness-analysis/
│
├── data/
│   ├── raw/                        # Original uncleaned dataset
│   │   └── guitar_dataset_dirty.csv
│   └── processed/                  # Cleaned, analysis-ready dataset
│       └── guitar_dataset_clean.csv
│
├── dashboard/
│   └── Guitar_Handedness_Analysis.pbix   # Power BI dashboard file
│
├── docs/
│   └── dax_measures.md             # All DAX measures documentedassets/
    └── # Screenshots for README / portfolio
│
└── README.md
```

---


## Skills Demonstrated

- **Python** — pandas data cleaning pipeline with outlier handling and string normalisation
- **Power BI / DAX** — calculated measures, dynamic benchmarking, interactive filters
- **Data storytelling** — framing a business question around representation and retail gap analysis
- **Data quality** — systematic approach to identifying and resolving 7+ categories of data issue
