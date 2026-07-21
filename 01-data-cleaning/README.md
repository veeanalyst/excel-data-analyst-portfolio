# Project 1 — Data Cleaning & Validation

**Skill demonstrated:** Perform data cleaning and validation to ensure accuracy and consistency.

---

## Business question
A raw retail store sales export arrived with thousands of missing values across
key fields — product names, prices, quantities, and discount flags — making it
unreliable for reporting or decision-making.

The goal was to identify every data quality issue, flag and isolate the affected
rows, and deliver a clean dataset ready for analysis — plus put a system in place
to prevent the same problems from recurring.

---

## Dataset
**Dirty Retail Store Sales** — [Kaggle](https://www.kaggle.com/datasets/ahmedmohamed2003/retail-store-sales-dirty-for-data-cleaning)
- 12,575 rows of synthetic retail transaction data
- 11 columns: Transaction ID, Customer ID, Category, Item, Price Per Unit,
  Quantity, Total Spent, Payment Method, Location, Transaction Date, Discount Applied
- License: CC BY-SA 4.0

---

## Issues found

| Issue | Count |
|-------|-------|
| Missing Item values | 1,213 |
| Missing Price Per Unit | 609 |
| Missing Quantity | 604 |
| Missing Total Spent | 604 |
| Missing Discount Applied | 4,199 |
| Total Spent ≠ Price × Quantity | 0 |
| **Total rows flagged** | **4,996** |
| **Clean rows** | **7,579** |

---

## Workbook structure

The file `data-cleaning.xlsx` has four sheets:

### 1. Data Quality Checklist
The first thing you see when you open the file. Documents every issue type
found, the count of affected rows, and the fix applied. Results update
automatically if the raw data changes.

### 2. Raw Data
The original 12,575 rows with eight helper columns (L to S) added to the right.
Rows with any issue are highlighted in red.

### 3. Cleaned Data
Only the 7,579 rows that passed all checks. Pulled automatically from Raw Data
using INDEX/MATCH — no copy-paste. Includes a summary of total clean rows and
total transaction value.

### 4. New Entry Template
A blank form for adding new transactions going forward. Dropdown lists and
validation rules are built in so the same data quality problems cannot recur.

---

## How the cleaning logic works

Eight helper columns (L to S) are added to the right of the raw data.
Each one asks a simple yes/no question about that row:

| Column | Name | Question it asks |
|--------|------|-----------------|
| L | Flag_Blank_Item | Is the Item name missing? |
| M | Flag_Blank_Price | Is the Price Per Unit missing? |
| N | Flag_Blank_Qty | Is the Quantity missing? |
| O | Flag_Blank_Total | Is the Total Spent missing? |
| P | Flag_Blank_Discount | Is the Discount Applied missing? |
| Q | Flag_Total_Mismatch | Does Price × Quantity not equal Total Spent? |
| R | Issue_Count | How many of the above flags are TRUE? |
| S | Row_Status | **Clean** (0 issues) or **Needs Review** (1+ issues) |

Rows marked **Needs Review** are highlighted red using conditional formatting
tied to column S. The Cleaned Data sheet pulls only rows where S = "Clean".

---

## How the conditional formatting rule works

The red highlight is controlled by one formula applied to the entire data range (columns A to K):

```
=$S2="Needs Review"
```

Breaking it down:

| Part | Meaning |
|------|---------|
| `=` | Tells Excel this is a formula, not a value |
| `$S` | Always look in column S (Row_Status) — the `$` locks the column |
| `2` | Start from row 2 — no `$` here so the row number slides down automatically |
| `="Needs Review"` | If the value equals "Needs Review", apply the red fill |

**Why the `$` before S matters:**
Without it, the rule would drift sideways and check the wrong column as it
moves across the row. The `$` keeps it locked on column S for every cell it checks:

- Row 2 → checks **$S2**
- Row 3 → checks **$S3**
- Row 4 → checks **$S4**

**The full chain in plain English:**

```
Empty cell found            →  Flag column = TRUE
One or more flags TRUE      →  Issue_Count (R) goes up
Issue_Count > 0             →  Row_Status (S) = "Needs Review"
Row_Status = "Needs Review" →  Entire row turns red
```

This means the highlight is fully dynamic — fix a blank cell in the raw data
and the row automatically turns from red to white without touching the formatting.

---

## New Entry Template explained

The New Entry Template is a blank form designed for entering new transactions
going forward. It has seven built-in rules that make it impossible to submit
the same types of errors found in the raw data:

| Field | Rule |
|-------|------|
| Category | Dropdown — only the 8 valid categories accepted |
| Payment Method | Dropdown — Digital Wallet, Credit Card, or Cash only |
| Location | Dropdown — Online or In-store only |
| Discount Applied | Dropdown — TRUE or FALSE only (no more blanks) |
| Price Per Unit | Must be a number greater than 0 |
| Quantity | Must be a whole number greater than 0 |
| Transaction Date | Must be a valid date between 2020 and 2030 |
| Total Spent | Auto-calculated (Price × Quantity) — no manual entry needed |

> The Raw Data sheet shows what went wrong in the past.
> The New Entry Template prevents it from happening in the future.

---

## Files
- `raw-data/retail_store_sales.csv` — original dataset from Kaggle
- `data-cleaning.xlsx` — workbook with all four sheets
- `screenshots/` — before/after views of the data
