# Project 1 — Data Cleaning & Validation

**Skill demonstrated:** Perform data cleaning and validation to ensure accuracy and consistency.

---

## Business question
A raw retail store sales export arrived with thousands of missing values across
key fields — product names, prices, quantities, and discount flags — making it
unreliable for reporting or decision-making.

The goal was to identify every data quality issue, clean and standardize the
affected fields, flag and isolate incomplete rows, and deliver a clean dataset
ready for analysis — plus put a system in place to prevent the same problems
from recurring.

---

## Dataset
**Dirty Retail Store Sales** — [Kaggle](https://www.kaggle.com/datasets/ahmedmohamed2003/retail-store-sales-dirty-for-data-cleaning)
- 12,575 rows of synthetic retail transaction data
- 11 columns: Transaction ID, Customer ID, Category, Item, Price Per Unit,
  Quantity, Total Spent, Payment Method, Location, Transaction Date, Discount Applied
- License: CC BY-SA 4.0

---

## Issues found

| Issue | Count | Result |
|-------|-------|--------|
| Missing Item values | 1,213 | Flagged — rows excluded |
| Missing Price Per Unit | 609 | Flagged — rows excluded |
| Missing Quantity | 604 | Flagged — rows excluded |
| Missing Total Spent | 604 | Flagged — rows excluded |
| Missing Discount Applied | 4,199 | Flagged — rows excluded |
| Total Spent ≠ Price × Quantity | 0 | ✅ No mismatches found |
| Duplicate Transaction IDs | 0 | ✅ No duplicates found |
| Extra spaces — All Text Fields | 0 | ✅ TRIM applied — none found |
| **Total rows flagged** | **4,996** | |
| **Clean rows** | **7,579** | |

> TRIM was applied across all text fields as standard practice. No extra spaces
> were found in this dataset, confirming the export was clean in that regard.
> Duplicate checks and Total Spent cross-validation were also run — both clear.

---

## Workbook structure

### 1. Data Quality Checklist
Documents every issue checked, the count found, and the fix applied.
Results update automatically if the raw data changes.

![Data Quality Checklist](./screenshots/.png)

---

### 2. Raw Data
The original 12,575 rows with four TRIM cleaning columns (L–O) and eleven flag
columns (P–AB) added to the right. Rows with any issue are highlighted in red.

![Raw Data](./screenshots/.png)

---

### 3. Cleaned Data
Only the 7,579 rows that passed all checks. Pulled automatically from Raw Data
using INDEX/MATCH — no copy-paste. Uses TRIM-cleaned versions of Category,
Item, Payment Method, and Location.

![Cleaned Data](./screenshots/.png)

---

### 4. New Entry Template
A blank form for entering new transactions going forward. Dropdown lists and
validation rules are built in so the same data quality problems cannot recur.

![New Transaction_Entry Template](./screenshots/.png)

---

## How the cleaning works

### Step 1 — Apply TRIM to all text fields (columns L–O)

TRIM was applied to every text column as a standard first step, even before
checking for issues. This ensures no hidden spaces affect lookups, grouping,
or any downstream analysis.

| Column | Name | Formula | Field cleaned |
|--------|------|---------|---------------|
| L | Clean_Category | `=TRIM(C2)` | Category |
| M | Clean_Payment_Method | `=TRIM(H2)` | Payment Method |
| N | Clean_Item | `=TRIM(D2)` | Item |
| O | Clean_Location | `=TRIM(I2)` | Location |

**What TRIM does:** removes all leading and trailing spaces from a cell value.
> `"  Credit Card  "` → `"Credit Card"`

**Result:** No extra spaces were found in any column in this dataset.
The TRIM check confirms the source export was consistently formatted.

---

### Step 2 — Flag every issue (columns P–Z)

| Column | Flag | What it checks |
|--------|------|----------------|
| P | Flag_Blank_Item | Is Item missing? |
| Q | Flag_Blank_Price | Is Price Per Unit missing? |
| R | Flag_Blank_Qty | Is Quantity missing? |
| S | Flag_Blank_Total | Is Total Spent missing? |
| T | Flag_Blank_Discount | Is Discount Applied missing? |
| U | Flag_Total_Mismatch | Does Price × Quantity ≠ Total Spent? |
| V | Flag_Duplicate | Does this Transaction ID appear more than once? |
| W | Flag_Spaces_Category | Does raw Category differ from TRIM result? |
| X | Flag_Spaces_Payment | Does raw Payment Method differ from TRIM result? |
| Y | Flag_Spaces_Item | Does raw Item differ from TRIM result? |
| Z | Flag_Spaces_Location | Does raw Location differ from TRIM result? |

---

### Step 3 — Count and label (columns AA, AB)

| Column | Name | Formula | What it does |
|--------|------|---------|-------------|
| AA | Issue_Count | `=COUNTIF(P2:Z2,TRUE)` | Counts how many flags are TRUE |
| AB | Row_Status | `=IF(AA2=0,"Clean","Needs Review")` | Labels the row |

---

## How the conditional formatting rule works

The red highlight on the Raw Data sheet is controlled by one formula
applied across columns A to K:

```
=$AB2="Needs Review"
```

| Part | Meaning |
|------|---------|
| `=` | Tells Excel this is a formula |
| `$AB` | Always check column AB (Row_Status) — `$` locks the column |
| `2` | Row number — no `$` so it slides down automatically for each row |
| `="Needs Review"` | If true, apply the red fill |

**The full chain:**
```
Dirty or empty cell         →  Flag column = TRUE
One or more flags TRUE      →  Issue_Count (AA) > 0
Issue_Count > 0             →  Row_Status (AB) = "Needs Review"
Row_Status = "Needs Review" →  Row turns red
```

Fix a blank cell and the row automatically clears from red to white.

---

## New Entry Template explained

Seven built-in rules prevent the same errors recurring in future data entry:

| Field | Rule |
|-------|------|
| Category | Dropdown — 8 valid categories only |
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
- `screenshots/` — views of each sheet
