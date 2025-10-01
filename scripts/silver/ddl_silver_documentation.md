
---

# 📘 Documentation: Silver Layer DDL Script

## Purpose

This script is responsible for creating the **Silver Layer tables** in the data warehouse.
The Silver Layer represents the **cleansed and standardized data**, which is modeled based on the Bronze (raw) layer but prepared for transformations, analytics, and integration into the Gold Layer later.

It ensures:

* Old versions of the tables are dropped (clean reset).
* New tables are created with appropriate columns, data types, and a metadata field (`dwh_create_date`) to track load time.
* Structures match the data coming from the Bronze Layer but in a **refined schema**.

---

## Workflow

1. **Check and Drop Existing Tables**

   * For each Silver Layer table, the script checks if the table already exists using `OBJECT_ID`.
   * If found, the table is dropped.
   * This ensures a clean state before recreating the tables.

   ```sql
   IF OBJECT_ID('silver.crm_cust_info', 'U') IS NOT NULL
       DROP TABLE silver.crm_cust_info;
   ```

2. **Create New Tables**

   * Each table is defined with its respective schema.
   * Columns are chosen based on business meaning (customer, product, sales, ERP data).
   * `dwh_create_date` is added to track the ETL load timestamp.

   Example:

   ```sql
   CREATE TABLE silver.crm_cust_info (
       cst_id             INT,
       cst_key            NVARCHAR(50),
       cst_firstname      NVARCHAR(50),
       cst_lastname       NVARCHAR(50),
       cst_marital_status NVARCHAR(50),
       cst_gndr           NVARCHAR(50),
       cst_create_date    DATE,
       dwh_create_date    DATETIME2 DEFAULT GETDATE()
   );
   ```

---

## Tables Created

### 1. **silver.crm_cust_info**

Stores cleansed CRM customer data.

* `cst_id` → Customer ID (primary identifier)
* `cst_key` → Unique customer reference key
* `cst_firstname`, `cst_lastname` → Name attributes
* `cst_marital_status` → Marital status (single, married, etc.)
* `cst_gndr` → Gender
* `cst_create_date` → Date customer record created in source
* `dwh_create_date` → Metadata load timestamp

---

### 2. **silver.crm_prd_info**

Stores standardized CRM product information.

* `prd_id` → Product ID
* `cat_id` → Category reference ID
* `prd_key` → Product unique key
* `prd_nm` → Product name
* `prd_cost` → Product cost (numeric)
* `prd_line` → Product line (e.g., R = Road, S = Sporting, etc.)
* `prd_start_dt`, `prd_end_dt` → Product validity period
* `dwh_create_date` → Metadata load timestamp

---

### 3. **silver.crm_sales_details**

Stores cleaned CRM sales transaction data.

* `sls_ord_num` → Sales order number
* `sls_prd_key` → Product key (joins with product info)
* `sls_cust_id` → Customer ID (joins with customer info)
* `sls_order_dt` → Order date
* `sls_ship_dt` → Shipping date
* `sls_due_dt` → Payment due date
* `sls_sales` → Sales amount
* `sls_quantity` → Quantity sold
* `sls_price` → Price per unit
* `dwh_create_date` → Metadata load timestamp

---

### 4. **silver.erp_loc_a101**

Stores location-related ERP data.

* `cid` → Country ID / code
* `cntry` → Country name
* `dwh_create_date` → Metadata load timestamp

---

### 5. **silver.erp_cust_az12**

Stores ERP customer demographic data.

* `cid` → Customer ID / reference
* `bdate` → Birthdate
* `gen` → Gender
* `dwh_create_date` → Metadata load timestamp

---

### 6. **silver.erp_px_cat_g1v2**

Stores ERP product category mapping.

* `id` → Category ID
* `cat` → Category name
* `subcat` → Subcategory name
* `maintenance` → Maintenance status/category (business-specific)
* `dwh_create_date` → Metadata load timestamp

---

## How It Works in the Pipeline

* **Bronze → Silver:**

  * Data from the Bronze Layer (raw CSV bulk inserts) is transformed and cleaned, then inserted into these Silver tables.
  * Silver ensures **data quality, consistent types, and relationships**.

* **Silver → Gold:**

  * These Silver tables act as the **staging layer** for further business transformations, aggregations, and reporting in the Gold Layer.

---

✅ In short:

* Bronze = Raw (direct from source)
* Silver = Cleansed (structured, business-ready)
* Gold = Curated (for analytics, dashboards, ML models)

---
