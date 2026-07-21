# Project 1 — Data Cleaning & Validation

**Skill demonstrated:** Perform data cleaning and validation to ensure accuracy and consistency.

## Business question
A raw retail sales export arrives with duplicate rows, inconsistent date formats,
blank cells, typos in category names, and mixed text/number fields.
Goal: produce a clean, validated dataset ready for analysis.

## Approach
- Standardized text fields using TRIM, CLEAN, and PROPER
- Fixed inconsistent categories/regions via VLOOKUP against a correction table
- Parsed mixed date formats with DATEVALUE
- Stripped currency symbols and commas from price fields using SUBSTITUTE + VALUE
- Flagged blank rows, bad numbers, and duplicates with helper columns
- Applied Data Validation rules to prevent future bad entries

## Files
- `raw-data/` — original messy dataset
- `data-cleaning.xlsx` — workbook with Raw Data, Cleaned Data, Reference, and Checklist tabs
- `screenshots/` — before/after views of the data

## Results
| | Count |
|---|---|
| Rows in raw export | 12,575 |
| Rows passing all checks | TBD |
| Rows excluded | TBD |

## Dataset
Dirty Retail Store Sales — [Kaggle](https://www.kaggle.com/datasets/ahmedmohamed2003/retail-store-sales-dirty-for-data-cleaning)
License: CC BY-SA 4.0
