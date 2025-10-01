
---

# 📘 Stored Procedure Documentation — `bronze.load_bronze`

## 🔹 Purpose

The stored procedure **`bronze.load_bronze`** is responsible for loading raw data from **external CSV files** into the **Bronze Layer** (`bronze` schema) of the data warehouse.

The **Bronze Layer** acts as the **raw ingestion layer** — storing data as close to the source format as possible, with minimal transformations.

This procedure ensures that all bronze tables are refreshed with the latest data from CSV source files.

---

## 🔹 Actions Performed

1. **Start Batch Timer** – Tracks total load duration.
2. **Truncate Bronze Tables** – Removes old data before reload (avoids duplicates).
3. **Bulk Insert from CSV Files** – Loads fresh data into Bronze tables using `BULK INSERT`.
4. **Log Progress** – Uses `PRINT` statements to output progress and time taken.
5. **Error Handling** – Captures any errors and prints relevant details.

---

## 🔹 Step-by-Step Explanation

### 1. **Initialization**

```sql
DECLARE @start_time DATETIME, @end_time DATETIME, @batch_start_time DATETIME, @batch_end_time DATETIME;
SET @batch_start_time = GETDATE();
```

* Declares variables for measuring **load duration** per table and for the **entire batch**.

---

### 2. **Loading CRM Tables**

For each CRM table:

* **Truncate old data** → `TRUNCATE TABLE bronze.crm_cust_info`
* **Bulk load new data from CSV** →

  ```sql
  BULK INSERT bronze.crm_cust_info
  FROM 'D:\SQL Server\Project\sql-data-warehouse-project\datasets\source_crm\cust_info.csv'
  WITH (
      FIRSTROW = 2,          -- Skip header row
      FIELDTERMINATOR = ',', -- CSV delimiter
      TABLOCK                -- Lock table for fast load
  );
  ```
* **Measure load time** → Using `DATEDIFF(SECOND, @start_time, @end_time)`.

This process repeats for:

* `crm_cust_info`
* `crm_prd_info`
* `crm_sales_details`

---

### 3. **Loading ERP Tables**

Same workflow as CRM tables, but loads data from **ERP CSVs**:

* `erp_loc_a101`
* `erp_cust_az12`
* `erp_px_cat_g1v2`

---

### 4. **Completion Logs**

```sql
SET @batch_end_time = GETDATE();
PRINT 'Loading Bronze Layer is Completed';
PRINT 'Total Load Duration: ' + CAST(DATEDIFF(SECOND, @batch_start_time, @batch_end_time) AS NVARCHAR) + ' seconds';
```

* Logs the total load duration for the full batch.

---

### 5. **Error Handling**

```sql
BEGIN CATCH
    PRINT 'ERROR OCCURED DURING LOADING BRONZE LAYER'
    PRINT 'Error Message' + ERROR_MESSAGE();
    PRINT 'Error Number' + CAST (ERROR_NUMBER() AS NVARCHAR);
    PRINT 'Error State' + CAST (ERROR_STATE() AS NVARCHAR);
END CATCH
```

* If an error occurs (e.g., missing CSV file, file lock, wrong delimiter), the procedure **prints error details**.
* This makes troubleshooting easier.

---

## 🔹 Parameters

* **None**
  This procedure does not accept any input parameters.

---

## 🔹 Usage

Run the procedure with:

```sql
EXEC bronze.load_bronze;
```

This will:

* Truncate all **bronze tables**.
* Reload them from their respective CSV source files.
* Print log messages with load durations.

---

## 🔹 Example Output (Console)

```
================================================
Loading Bronze Layer
================================================
------------------------------------------------
Loading CRM Tables
------------------------------------------------
>> Truncating Table: bronze.crm_cust_info
>> Inserting Data Into: bronze.crm_cust_info
>> Load Duration: 3 seconds
>> -------------
>> Truncating Table: bronze.crm_prd_info
>> Inserting Data Into: bronze.crm_prd_info
>> Load Duration: 2 seconds
...
==========================================
Loading Bronze Layer is Completed
   - Total Load Duration: 15 seconds
==========================================
```

---

## 🔹 Key Points

* **`TRUNCATE`** is used instead of `DELETE` → faster and resets identity values.
* **`BULK INSERT`** is optimized for large file loads, but requires the file path to be accessible to SQL Server.
* **`FIRSTROW = 2`** skips CSV headers.
* **`FIELDTERMINATOR = ','`** ensures proper parsing of comma-separated values.
* **`TABLOCK`** minimizes locking overhead during load.
* **Error handling** ensures failures are logged but do not silently fail.

---

✅ With this doc, anyone checking your GitHub can quickly understand **what the stored procedure does, how it works, and how to run it**.
