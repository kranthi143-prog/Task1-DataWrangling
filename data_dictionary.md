# Data Dictionary — Sales Transactions Dataset

**Source file:** ApexPlanet_DataAnalytics_Dataset.xlsx (sheet: `Sales_Dataset`)
**Rows:** 1,000 | **Columns:** 12 (raw) → 19 (after cleaning/feature engineering)
**Grain:** One row = one sales order line item

| # | Column | Data Type | Description | Business Relevance |
|---|--------|-----------|--------------|---------------------|
| 1 | Order_ID | Text (string) | Unique identifier for each order | Primary key; used to track and audit individual transactions |
| 2 | Order_Date | Date (YYYY-MM-DD) | Date the order was placed | Enables trend, seasonality, and time-series analysis |
| 3 | Customer_ID | Text (string) | Unique identifier for each customer | Used to track repeat purchases and customer lifetime value |
| 4 | Customer_Name | Text (string) | Customer's display name | Reference only; not used in aggregate analysis (PII-light placeholder names) |
| 5 | Age | Numeric (float) | Customer's age in years | Enables demographic segmentation (e.g., age-group buying patterns) |
| 6 | Gender | Categorical (Male/Female) | Customer's gender | Enables demographic segmentation |
| 7 | City | Categorical (text) | City where the order was placed | Enables geographic/regional sales analysis |
| 8 | Product | Categorical (text) | Product purchased (Rice, Book, Mobile, Laptop, Shoes, Chair) | Drives product-level revenue and demand analysis |
| 9 | Category | Categorical (text) | Product category (Grocery, Education, Electronics, Fashion, Furniture) | Drives category-level revenue and demand analysis |
| 10 | Quantity | Numeric (integer) | Number of units purchased in the order | Used to calculate revenue and demand volume |
| 11 | Unit_Price | Numeric (float, ₹) | Price per single unit of the product | Used to calculate revenue; supports pricing analysis |
| 12 | Total_Sales | Numeric (float, ₹) | Quantity × Unit_Price (order revenue) | Primary revenue metric used across all downstream tasks |

## Engineered columns (added during cleaning — Task 1, Step 3)

| # | Column | Data Type | Description | Business Relevance |
|---|--------|-----------|--------------|---------------------|
| 13 | Order_Year | Integer | Year extracted from Order_Date | Year-over-year comparisons |
| 14 | Order_Month | Integer (1–12) | Month extracted from Order_Date | Monthly trend analysis |
| 15 | Order_Month_Name | Text | Month name (Jan, Feb, …) | Readable axis labels in charts/dashboards |
| 16 | Order_Quarter | Text (Q1–Q4) | Calendar quarter of the order | Quarterly business reporting |
| 17 | Age_Group | Categorical (18-25, 26-35, 36-45, 46-55, 56-65) | Binned customer age | Simplifies age-based segmentation for EDA/dashboards |
| 18 | Is_High_Value_Order | Boolean | Flags orders with Total_Sales above the IQR upper outlier bound (₹438,707) | Lets analysts isolate/exclude high-value orders without deleting real data |
| 19 | Data_Quality_Flag | Text | Notes which fields were imputed/corrected for this row (e.g., "Age imputed", "City imputed", "Order_ID corrected") | Transparency/audit trail for any analyst using the cleaned file |

## Known data quality issues identified and how they were handled

| Issue | Rows affected | Resolution |
|-------|---------------|------------|
| Missing `Age` | 20 (2.0%) | Imputed with the dataset median age (41) — minimal distortion since the distribution is roughly symmetric |
| Missing `City` | 13 (1.3%) | Imputed as `"Unknown"` rather than a guessed city, to avoid introducing false geographic signal |
| Duplicate `Order_ID` ("ORD100050" used 9 times for 9 different real transactions) | 9 | Re-assigned new sequential unique IDs (ORD109001–ORD109008) to all but the first occurrence |
| `Order_Date` stored as text | 1,000 (all rows) | Cast to proper datetime type |
| Statistical outliers in `Total_Sales` (above IQR upper bound) | 19 (1.9%) | Flagged via `Is_High_Value_Order` rather than removed — they reflect genuine high-ticket Electronics/Furniture orders, not data errors |
| Exact duplicate rows | 0 | None found |
| Inconsistent casing/spacing in categorical fields | 0 | None found — Gender, City, Product, Category were already standardized |
