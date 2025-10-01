
---

# 📘 Documentation: Stored Procedure `silver.load_silver`

## **Purpose**

The procedure **loads and transforms data from Bronze layer tables into Silver layer tables**.
This is the **Transform + Load (T+L)** step in ETL, where raw ingested data is standardized, cleaned, and business rules are applied.

---

## **Key Features**

* **Truncates** Silver tables before loading (ensures fresh data every run).
* **Transforms** Bronze data:

  * Cleans strings (TRIM).
  * Normalizes coded values (e.g., gender `M/F` → `Male/Female`).
  * Handles invalid/missing values (`n/a` or `NULL`).
  * Derives fields (e.g., `prd_end_dt` from product start dates).
  * Removes duplicates, keeps only **most recent records** for customers.
* **Loads** the cleaned results into Silver schema.

---

## **Detailed Step-by-Step Process**

### 🔹 1. CRM Tables

#### a. `silver.crm_cust_info`

* Removes duplicates → keeps latest record per customer (`ROW_NUMBER` with `ORDER BY cst_create_date DESC`).
* Normalizes:

  * `S/M` → `Single/Married`.
  * `M/F` → `Male/Female`.
* Trims first/last names.
* Inserts only valid `cst_id`.

✅ **Outcome:** Clean, deduplicated, standardized customer info.

---

#### b. `silver.crm_prd_info`

* Extracts:

  * `cat_id` from product key (`SUBSTRING` + `REPLACE`).
  * Product key part separately.
* Maps product line codes:

  * `M` → Mountain
  * `R` → Road
  * `T` → Touring
  * `S` → Other Sales
* Converts product start/end dates:

  * `prd_start_dt` = cast to DATE.
  * `prd_end_dt` = one day before next start date (`LEAD` window function).
* Fills missing `prd_cost` with `0`.

✅ **Outcome:** Products tied to categories with descriptive values and clear date ranges.

---

#### c. `silver.crm_sales_details`

* Validates and converts order/shipping/due dates:

  * If `0` or invalid format → `NULL`.
* Fixes sales calculation:

  * If `sls_sales` invalid, recompute as `sls_quantity * ABS(sls_price)`.
* Ensures price:

  * If missing or ≤ 0 → derives from sales ÷ quantity.

✅ **Outcome:** Sales data consistent, dates valid, revenue calculations corrected.

---

### 🔹 2. ERP Tables

#### a. `silver.erp_cust_az12`

* Cleans customer ID:

  * If `NASxxxxx` → strips the `NAS` prefix.
* Birthdate check:

  * If in the **future** → set to `NULL`.
* Gender normalization:

  * `M/male` → `Male`, `F/female` → `Female`, else `n/a`.

✅ **Outcome:** Valid ERP customer master data.

---

#### b. `silver.erp_loc_a101`

* Cleans location codes:

  * Removes dashes from `cid`.
* Normalizes country:

  * `DE` → Germany.
  * `US` / `USA` → United States.
  * Blank/NULL → `n/a`.

✅ **Outcome:** Location data mapped to proper country names.

---

#### c. `silver.erp_px_cat_g1v2`

* Directly inserts data from Bronze (no transformation applied here).

✅ **Outcome:** Category hierarchy carried forward as-is.

---

## **Error Handling**

* Wrapped in `TRY…CATCH`.
* On failure:

  * Prints `"ERROR OCCURRED DURING LOADING SILVER LAYER"`.
  * Logs error message, number, and state.

---

## **Usage Example**

```sql
EXEC silver.load_silver;
```

When executed:

* Logs start/end time.
* Shows progress messages per table.
* Reports duration of each table load and total batch runtime.

---

## **How It Works in ETL Flow**

* **Bronze → Silver**
  Bronze = raw dumps from source (CSV, ERP, CRM).
  Silver = cleansed, business-rule-applied data, ready for **joins, analysis, and downstream Gold layer**.

---

⚡ In short:

* **Bronze** = raw/raw-like.
* **Silver** = cleaned + standardized + business logic applied.
* **Gold** = aggregated, business KPI-ready.

---
