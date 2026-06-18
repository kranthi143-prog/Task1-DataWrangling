# Task 1 – Data Immersion & Wrangling
**ApexPlanet Software Pvt. Ltd. — Data Analytics Internship (60-Day Program)**

## Objective
The goal of this task was to take a raw sales transactions dataset, identify data quality issues, clean and transform it, and produce a final analysis-ready dataset — along with documentation of every variable and every fix made.

## Dataset
The dataset (`ApexPlanet_DataAnalytics_Dataset.xlsx`) contains 1,000 sales order line items with details on customers, products, and revenue. Full details on every column are in `data_dictionary.md`.

## What's in this repository
| File | Description |
|------|--------------|
| `data_dictionary.md` | Describes every column in the raw and cleaned dataset, plus a summary of every data quality issue found and how it was fixed |
| `clean_data.py` | Python (pandas) script that profiles the raw data, fixes the issues, engineers new columns, and outputs the cleaned file |
| `Sales_Dataset_Cleaned.xlsx` | The final, analysis-ready dataset |

## Data quality issues found and fixed
- 20 missing `Age` values → imputed with the median age
- 13 missing `City` values → imputed as `"Unknown"` to avoid fabricating location data
- 1 `Order_ID` ("ORD100050") mistakenly reused across 9 different real transactions → corrected with new unique IDs
- `Order_Date` stored as plain text → converted to a proper date type
- 19 statistically high `Total_Sales` values → flagged (not deleted), since they represent genuine high-value orders

## New columns added
`Order_Year`, `Order_Month`, `Order_Month_Name`, `Order_Quarter`, `Age_Group`, `Is_High_Value_Order`, `Data_Quality_Flag`

## How to run
```bash
pip install pandas openpyxl
python clean_data.py
```

## Tools used
Python, pandas, openpyxl, Excel

## Author
Nidiganti kranthi goud — Data Analytics Intern, ApexPlanet Software Pvt. Ltd.
