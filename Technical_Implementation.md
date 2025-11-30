# 🛠️ Technical Implementation (M-Code & DAX)

This document provides a detailed technical breakdown of the advanced Power BI techniques implemented in the project, including DAX measures, M-Code transformations, and problem-solving strategies.

---

<details>
<summary><strong>Challenge 1: Dynamic Visual Feedback</strong></summary>

**Problem:** KPI cards remained static regardless of filter selection.  
**Solution:** Implemented advanced DAX with `ISFILTERED()` and `VALUES()` for conditional formatting.  
**Impact:** Cards now highlight/fade based on active filters, improving user analysis speed.

```DAX
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
</details> <details> <summary><strong>Challenge 2: Responsive Bar Chart Fix</strong></summary>

Problem: Bar chart showing incorrect counts, not responding to slicer selections.
Solution: Corrected context transition using COUNTROWS() instead of direct table reference.
Impact: 100% filtering accuracy; charts now reflect selected data correctly.

</details> <details> <summary><strong>Challenge 2: Responsive Bar Chart Fix</strong></summary>

Problem: Bar chart showing incorrect counts, not responding to slicer selections.
Solution: Corrected context transition using COUNTROWS() instead of direct table reference.
Impact: 100% filtering accuracy; charts now reflect selected data correctly.
