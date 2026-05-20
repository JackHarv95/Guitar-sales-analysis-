# DAX Measures — Guitar Handedness Analysis

This file documents all DAX measures used in the Power BI dashboard.

---

## Core Sales Measures

### Total Sales
```dax
Total Sales = SUM(guitar_dataset_clean[sales])
```
Sum of all units sold across the filtered selection.

---

### Left-Handed Sales
```dax
Left Sales = 
CALCULATE(
    SUM(guitar_dataset_clean[sales]),
    guitar_dataset_clean[handedness] = "Left"
)
```

---

### Right-Handed Sales
```dax
Right Sales = 
CALCULATE(
    SUM(guitar_dataset_clean[sales]),
    guitar_dataset_clean[handedness] = "Right"
)
```

---

## Representation Measures

### Left-Handed Sales %
```dax
Left Sales % = 
DIVIDE(
    [Left Sales],
    [Total Sales],
    0
)
```
Returns the proportion of total sales that are left-handed. Formatted as a percentage in the report.

---

### Benchmark (Population Assumption)
```dax
Benchmark % = 'Benchmark'[Benchmark Value] / 100
```
Reads the value from the `Benchmark` parameter table, which is driven by the slicer in the report. Default value: 10 (representing the ~10% left-handed population estimate).

---

### Representation Gap
```dax
Representation Gap = 
[Benchmark %] - [Left Sales %]
```
Positive value = left-handed guitars are underrepresented relative to benchmark.  
Negative value = left-handed guitars exceed the benchmark.

---

### Gap Label (for card visual)
```dax
Gap Label = 
VAR Gap = [Representation Gap]
RETURN
IF(
    Gap > 0,
    FORMAT(Gap, "0.0%") & " below benchmark",
    FORMAT(ABS(Gap), "0.0%") & " above benchmark"
)
```

---

## Price Measures

### Avg Price — Left
```dax
Avg Price Left = 
CALCULATE(
    AVERAGE(guitar_dataset_clean[price]),
    guitar_dataset_clean[handedness] = "Left"
)
```

---

### Avg Price — Right
```dax
Avg Price Right = 
CALCULATE(
    AVERAGE(guitar_dataset_clean[price]),
    guitar_dataset_clean[handedness] = "Right"
)
```

---

### Price Premium (Left vs Right)
```dax
Price Premium Left = 
[Avg Price Left] - [Avg Price Right]
```
Positive value indicates left-handed models cost more on average — common in the market due to limited production runs.

---

## Review Score Measures

### Avg Review — Left
```dax
Avg Review Left = 
CALCULATE(
    AVERAGE(guitar_dataset_clean[review_score]),
    guitar_dataset_clean[handedness] = "Left"
)
```

---

### Avg Review — Right
```dax
Avg Review Right = 
CALCULATE(
    AVERAGE(guitar_dataset_clean[review_score]),
    guitar_dataset_clean[handedness] = "Right"
)
```

---

## Notes

- All measures respect the active filter context from the **Manufacturer** and **Guitar Type** slicers.
- The **Benchmark** parameter is created as a disconnected table with a single numeric column, connected to a slicer with min = 1, max = 30, step = 1.
- `DIVIDE()` is used instead of `/` throughout to handle divide-by-zero gracefully (returns 0 when denominator is blank).
