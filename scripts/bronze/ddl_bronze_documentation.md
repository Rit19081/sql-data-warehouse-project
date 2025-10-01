
---

# 📘 Bronze Schema — DDL Documentation

This repository contains the DDL script to create **Bronze Layer Tables** for the data pipeline.
The **Bronze Layer** is the raw ingestion layer, storing source data with minimal transformation.

The script drops existing tables if they already exist and re-creates them.

---

## 🔹 Tables Overview

| Table Name                 | Source | Description                             |
| -------------------------- | ------ | --------------------------------------- |
| `bronze.crm_cust_info`     | CRM    | Customer master details (CRM-facing).   |
| `bronze.crm_prd_info`      | CRM    | Product catalog details.                |
| `bronze.crm_sales_details` | CRM    | Sales order transactions.               |
| `bronze.erp_loc_a101`      | ERP    | Location and country details.           |
| `bronze.erp_cust_az12`     | ERP    | ERP master customer details.            |
| `bronze.erp_px_cat_g1v2`   | ERP    | Product categories and classifications. |

---

## 🔹 Table Definitions

### 1. `bronze.crm_cust_info`

Stores **customer information** from CRM systems.

| Column               | Data Type    | Description                                   |
| -------------------- | ------------ | --------------------------------------------- |
| `cst_id`             | INT          | Unique customer identifier (surrogate key).   |
| `cst_key`            | NVARCHAR(50) | Business/customer key (alternate ID).         |
| `cst_firstname`      | NVARCHAR(50) | Customer first name.                          |
| `cst_lastname`       | NVARCHAR(50) | Customer last name.                           |
| `cst_marital_status` | NVARCHAR(50) | Marital status (`M` = Married, `S` = Single). |
| `cst_gndr`           | NVARCHAR(50) | Gender (`M` = Male, `F` = Female).            |
| `cst_create_date`    | DATE         | Record creation date.                         |

---

### 2. `bronze.crm_prd_info`

Stores **product catalog** details.

| Column         | Data Type    | Description                                                                                              |
| -------------- | ------------ | -------------------------------------------------------------------------------------------------------- |
| `prd_id`       | INT          | Unique product ID.                                                                                       |
| `prd_key`      | NVARCHAR(50) | Product code.                                                                                            |
| `prd_nm`       | NVARCHAR(50) | Product name/description.                                                                                |
| `prd_cost`     | INT          | Standard product cost.                                                                                   |
| `prd_line`     | NVARCHAR(50) | Product line category: `R` = Road, `M` = Mountain, `T` = Touring, `S` = Standard (Accessories/Clothing). |
| `prd_start_dt` | DATETIME     | Start date when product became active.                                                                   |
| `prd_end_dt`   | DATETIME     | End date (null if still active).                                                                         |

---

### 3. `bronze.crm_sales_details`

Stores **sales transaction records**.

| Column         | Data Type    | Description                                    |
| -------------- | ------------ | ---------------------------------------------- |
| `sls_ord_num`  | NVARCHAR(50) | Sales order number.                            |
| `sls_prd_key`  | NVARCHAR(50) | Product key (links to `crm_prd_info.prd_key`). |
| `sls_cust_id`  | INT          | Customer ID (links to `crm_cust_info.cst_id`). |
| `sls_order_dt` | INT          | Order date (YYYYMMDD format).                  |
| `sls_ship_dt`  | INT          | Shipment date.                                 |
| `sls_due_dt`   | INT          | Due date (delivery/payment).                   |
| `sls_sales`    | INT          | Total sales amount.                            |
| `sls_quantity` | INT          | Quantity ordered.                              |
| `sls_price`    | INT          | Price per unit (transactional).                |

---

### 4. `bronze.erp_loc_a101`

Stores **location data** (country-level).

| Column  | Data Type    | Description                                                                    |
| ------- | ------------ | ------------------------------------------------------------------------------ |
| `cid`   | NVARCHAR(50) | Location identifier.                                                           |
| `cntry` | NVARCHAR(50) | Country name (may have inconsistencies like `US` vs `USA` vs `United States`). |

---

### 5. `bronze.erp_cust_az12`

ERP **master customer data** (slightly different from CRM).

| Column  | Data Type    | Description               |
| ------- | ------------ | ------------------------- |
| `cid`   | NVARCHAR(50) | Customer ID (ERP system). |
| `bdate` | DATE         | Birth date of customer.   |
| `gen`   | NVARCHAR(50) | Gender (`Male`/`Female`). |

---

### 6. `bronze.erp_px_cat_g1v2`

Stores **product category and classification**.

| Column        | Data Type    | Description                                                                     |
| ------------- | ------------ | ------------------------------------------------------------------------------- |
| `id`          | NVARCHAR(50) | Category/subcategory identifier.                                                |
| `cat`         | NVARCHAR(50) | High-level product category (`Bikes`, `Clothing`, `Accessories`, `Components`). |
| `subcat`      | NVARCHAR(50) | Detailed subcategory (e.g., `Mountain Bikes`, `Helmets`, `Gloves`, `Chains`).   |
| `maintenance` | NVARCHAR(50) | Maintenance flag / notes (active/inactive).                                     |

---

## 🔹 Relationships (Logical)

* **Customers**

  * `crm_cust_info.cst_id` ↔ `crm_sales_details.sls_cust_id`
  * `erp_cust_az12.cid` may be linked to CRM via mapping rules.

* **Products**

  * `crm_prd_info.prd_key` ↔ `crm_sales_details.sls_prd_key`
  * `crm_prd_info.prd_line` categorizes products (`R`, `M`, `T`, `S`).
  * `erp_px_cat_g1v2.cat/subcat` provides more detailed classification.

* **Locations**

  * `erp_loc_a101.cntry` can be linked to customers for geography-based analysis.

---

## 🔹 Usage Notes

* This is the **Bronze Layer (raw zone)** → data is stored as close to the source as possible.
* Minimal transformations are done here; cleansing and enrichment should be performed in the **Silver Layer**.
* Some fields (e.g., `sls_order_dt` as INT, `cntry` inconsistencies) need transformation for analytics.

---
