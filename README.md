

---

# Supply Chain Analytics & Logistics Bottleneck Detection

**Author:** Debopam Ghosh

## Project Overview

This repository contains an end-to-end Business Intelligence and Data Architecture solution designed to identify and quantify supply chain bottlenecks. By processing over 181,000 raw logistics records, this project establishes a normalized data model and utilizes custom Data Analysis Expressions (DAX) to deliver real-time insights into late delivery risks and profit variance across geospatial and product dimensions.

## Architecture & Data Pipeline (ETL)

The original dataset consisted of a denormalized, massive flat `.csv` file. To optimize for analytics engine performance and ensure data integrity, a rigorous ETL (Extract, Transform, Load) pipeline was engineered using Power Query:

* **Data Extraction & Cleaning:** Removed redundant columns, corrected data types, and handled missing values.
* **Dimensional Modeling:** Deconstructed the flat file into a standard **Star Schema** to reduce data redundancy and improve query performance.
* **Schema Structure:**
* **Fact Table:** `Fact_Orders` (Contains transactional metrics: Order ID, Quantities, Delivery Status, Real/Scheduled Shipping Days).
* **Dimension Tables:** `Dim_Customer` (Geospatial and segment data) and `Dim_Product` (Category and pricing data).
* **Relationships:** Engineered 1-to-Many (`1:*`) relationships connecting the primary keys of the dimensions to the foreign keys in the fact table.



## Advanced Analytics (DAX)

To move beyond implicit calculations, a dedicated `_Key Measures` table was established to house explicit DAX formulas for enterprise-level reporting:

```dax
// 1. Baseline Volume
Total Orders = COUNT(Fact_Orders[Order Id])

// 2. Bottleneck Metric: Calculates the actual deviation from scheduled delivery
Avg Shipping Variance (Days) = 
    AVERAGE(Fact_Orders[Days for shipping (real)]) - AVERAGE(Fact_Orders[Days for shipment (scheduled)])

// 3. Overall Risk Assessment
Risk Rate % = SUM(Fact_Orders[Late_delivery_risk]) / [Total Orders]

```

## Dashboard & Visualizations

The frontend Power BI dashboard serves as an interactive control panel for the underlying data model, allowing stakeholders to isolate performance metrics:

* **KPI Cards:** High-level aggregation of Total Orders, Risk Rate (54.83%), and Average Shipping Variance (0.57 days).
* **Geospatial Matrix:** Tracks delivery variance specific to customer regions (e.g., EE. UU. vs. Puerto Rico).
* **Product Risk Bar Chart:** Ranks product categories by their propensity for late delivery.
* **Financial Impact Column Chart:** Maps order profit margins against customer segments to determine the fiscal impact of logistical delays.
* **Time-Series Analysis:** A dynamic line chart tracking order volume across fiscal years, actively filtered to remove incomplete annual data for accurate trend analysis.
* **Interactive Slicers:** Dropdown filtering allowing localized recalculation of all visuals based on specific product categories.

## Files in this Repository

* `SupplyChainAnalytics.pbix`: The complete Power BI Desktop file containing the data model and dashboard.
* `Dashboard_Preview.pdf`: A high-resolution export of the final interactive dashboard.
* `DataCoSupplyChainDataset.csv`: The raw, denormalized data source.

---

**Next Steps for GitHub Upload:**

1. Save this text as `README.md`.
2. Ensure you have exported the PDF preview as discussed earlier and name it `Dashboard_Preview.pdf` (or update the filename in the README to match what you saved it as).
3. Push the `.pbix`, `.pdf`, and `.md` files to your new repository.
