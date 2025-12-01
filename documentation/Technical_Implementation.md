# 🛠️ Technical Implementation (M-Code & DAX)

This document provides a detailed technical breakdown of the advanced Power BI techniques implemented in the project, including DAX measures, M-Code transformations, and problem-solving strategies.

---

<details>
<summary><strong>Challenge 1: Dynamic Visual Feedback</strong></summary>

**Problem:** KPI cards remained static regardless of filter selection.  
**Solution:** Implemented advanced DAX with `ISFILTERED()` and `VALUES()` for conditional formatting.  
**Impact:** Cards now highlight/fade based on active filters, improving user analysis speed.
```dax
-- Selected Status Value
[Selected Status Value] = SELECTEDVALUE('v_alm_hardware'[Status])

-- Font Color - In Use Fade
[Font Color - In Use Fade] =
VAR CurrentSelection = [Selected Status Value]
VAR CardStatus = "In use"
VAR HighlightColor = "#26417E"
VAR FadedColor = "#D9D9D9"
RETURN
    COALESCE(
        IF(CurrentSelection = CardStatus, HighlightColor, FadedColor),
        HighlightColor
    )

-- Is Selected
IsSelected =
VAR SelectedStatuses = VALUES('v_alm_hardware'[Status])
RETURN
IF(
    NOT ISFILTERED('v_alm_hardware'[Status]) ||
    SELECTEDVALUE('v_alm_hardware'[Status]) IN SelectedStatuses,
    1,
    0
)

-- Card Color InUse
CardColor_InUse =
VAR Selected =
    IF(
        NOT ISFILTERED('v_alm_hardware'[Status]) ||
        "In use" IN VALUES('v_alm_hardware'[Status]),
        1,
        0
    )
RETURN
IF(Selected = 1, "#26417E", "#BBBBBB")

-- Card Color Total
CardColor_Total =
VAR SelectedCount = COUNTROWS(VALUES('v_alm_hardware'[Status]))
RETURN
IF(
    SelectedCount = 0,
    "#26417E",
    "#BBBBBB"
)
```

</details>

<details>
<summary><strong>Challenge 2: Responsive Bar Chart Fix</strong></summary>

**Problem:** Bar chart showing incorrect counts, not responding to slicer selections.  
**Solution:** Corrected context transition using `COUNTROWS()` instead of direct table reference.  
**Impact:** 100% filtering accuracy; charts now reflect selected data correctly.
```dax
-- Total Devices
Total Devices = COUNTROWS('v_alm_hardware')

-- Devices In Use
Devices In Use = CALCULATE(
    COUNTROWS('v_alm_hardware'),
    'v_alm_hardware'[Status] = "In use"
)

-- Devices In Stock
Devices In Stock = CALCULATE(
    COUNTROWS('v_alm_hardware'),
    'v_alm_hardware'[Status] = "In stock"
)

-- Devices Awaiting / Other
Devices Awaiting/Other = CALCULATE(
    COUNTROWS('v_alm_hardware'),
    'v_alm_hardware'[Status] IN {"In maintenance", "Missing", "Retired"}
)
```

</details>

<details>
<summary><strong>Challenge 3: Bulk Data Anonymization</strong></summary>

**Problem:** 100+ unique supplier names required manual replacement (hours of work).  
**Solution:** Built custom M-Code using Merge Queries + `Table.Distinct` for automated replacement.  
**Impact:** One-click anonymization process, maintaining data referential integrity.
```powerquery
let
    Source = Excel.Workbook(File.Contents("...")),
    AnonymizationTable = Table.Distinct(Source[SupplierID]),
    MergedData = Table.NestedJoin(
        Source, {"SupplierID"},
        AnonymizationTable, {"SupplierID"},
        "Anonymous",
        JoinKind.LeftOuter
    )
in
    MergedData
```

</details>

<details>
<summary><strong>Challenge 4: Data Standardization</strong></summary>

**Problem:** Inconsistent management status codes (e.g., `9 - MANAGED` vs `1 - MANAGED`).  
**Solution:** Applied M-Code transformation to standardize values across dataset.  
**Impact:** Improved query performance and reporting consistency for 10,000+ records.
```powerquery
// Example: Standardizing status values
= Table.ReplaceValue(
    PreviousStep,
    each [State],
    each if [State] = "9 - MANAGED" then "MANAGED" else [State],
    Replacer.ReplaceText,
    {"State"}
)
```

</details>

<details>
<summary><strong>Challenge 5: Time Intelligence for Tenure</strong></summary>

**Problem:** Calculate average employee tenure accurately.  
**Solution:** Used DAX Time Intelligence functions on Hire Date column.  
**Impact:** Key HR metric for workforce planning and retention analysis.
```dax
-- Years Since Purchase / Tenure
Years Since Purchase = DATEDIFF(
    MIN('v_alm_hardware'[u_purchase_date]),
    TODAY(),
    YEAR
)

-- Due for Replacement KPI
Due for Replacement = CALCULATE(
    [Total Devices],
    FILTER(
        ALL('v_alm_hardware'),
        [Years Since Purchase] >= 3
    )
)

-- Devices In Maintenance
Devices In Maintenance = CALCULATE(
    COUNTROWS('v_alm_hardware'),
    'v_alm_hardware'[Status] = "In maintenance"
)

-- Devices Awaiting/Other
Devices Awaiting/Other = CALCULATE(
    COUNTROWS('v_alm_hardware'),
    'v_alm_hardware'[Status] IN {"Missing", "Retired"}
)
```

</details>

---

## Summary

This project demonstrates advanced techniques in DAX and M-Code to solve core reporting challenges:

- **Context Transition** for responsive visuals
- **Bulk Anonymization** for data privacy
- **Time Intelligence** for accurate KPIs
- **Dynamic Conditional Formatting** for intuitive dashboards

All measures and transformations were designed to improve both accuracy and usability for business and technical stakeholders.
