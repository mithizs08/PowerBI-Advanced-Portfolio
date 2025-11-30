# Enterprise Data Analytics Portfolio: IT Asset & Employee Metrics

This repository showcases two production-ready Power BI dashboards using **anonymized data** to demonstrate advanced data modeling, complex DAX, and robust data transformation (M-code) capabilities.

---

## 🚀 Executive Summary & Project Scope

This dual-dashboard project handles two critical, highly sensitive enterprise domains: **IT Asset Management** and **Human Resources** simultaneously.

| Scope Detail | Technical Context |
| :--- | :--- |
| **Data Scale** | Modeled and cleansed transactional data streams (simulated at 10,000+ records) across multiple linked tables. |
| **Primary Challenge** | Implementing dynamic reporting logic while adhering to strict **data governance** rules (anonymization) and overcoming blocked data source access (simulated table join). |
| **Result** | Delivered two functional, secure, and highly scalable dashboards proving competency in end-to-end data pipeline ownership. |

---

## 1. IT Hardware Management Dashboard (IT_Hardware_Report.pbix)

### Focus: Asset Tracking, Maintenance, and Data Governance

This dashboard was built to resolve critical data modeling and reporting issues common in large enterprise environments.

| Challenge Solved | Technical Solution & Impact |
| :--- | :--- |
| **Dynamic Card Highlighting** | Implemented advanced DAX (using `ISFILTERED` and `VALUES`) to create conditional formatting logic. This forces KPI Cards to visually fade/highlight based on the Status Slicer selection, **improving user interaction and analysis speed.** |
| **Responsive Bar Chart Fix** | Corrected a common context transition error in the bar chart's X-axis with a simple `COUNTROWS()` measure. The chart is now fully responsive, ensuring **100% filtering accuracy** by reflecting selected counts, not table totals. |
| **Bulk Supplier Anonymization** | Developed custom **M-code (Merge Queries + Table.Distinct)** to create an *internal, dynamic lookup table*. This eliminated dozens of manual 'Replace Values' steps, allowing for a **single-step, automated replacement of 100+ unique confidential supplier names**, demonstrating superior efficiency and data security. |
| **Data Standardization** | Standardized the key `Management` field values (e.g., converting codes like `9 - MANAGED` to `1 - MANAGED`) to ensure reporting consistency and **improve query performance** across the 10K+ record dataset. |

## 2. Employee Metrics Dashboard (Employee_Report.pbix)

### Focus: Workforce Analytics, Tenure, and HR Confidentiality

This report demonstrates the ability to handle confidential HR data and implement Time Intelligence logic.

| Key Metric / Challenge | Technical Solution & Impact |
| :--- | :--- |
| **Average Tenure Calculation** | Implemented Time Intelligence DAX to calculate the average length of service across the organization in years, based on the `Hire Date` column, providing a **key HR metric for workforce planning.** |
| **HR Data Anonymization** | Applied the same **M-code (Merge Queries)** methodology used in the IT report to securely create generic user names, **mitigating privacy risks associated with PII** before publishing the file. |

---

## 🛠 Project Assets and Skills

The `.pbix` files contain the fully developed data model. **Download and open the files in Power BI Desktop to inspect the M-code and DAX measures directly.**

* **Source Files:** `Source_Data/` (Includes mock Excel files)
* **Report Files:** `PBI_Reports/` (Includes `.pbix` source and `.pdf` previews)

### Core Skills Demonstrated:

* **Advanced DAX:** Context transition control (`COUNTROWS`), Conditional Formatting logic (`ISFILTERED`), and Time Intelligence.
* **Power Query (M):** Dynamic table generation, custom column creation, advanced merging, and robust data cleansing.
* **Data Governance:** Data security via internal bulk anonymization and data standardization.
* **Data Modeling:** Establishing functional relationships for cross-table lookups on large datasets.
