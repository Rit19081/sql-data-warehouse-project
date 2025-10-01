# Data Warehouse and Analytics Project

Welcome to the **Data Warehouse and Analytics Project** repository! 🚀  
This project demonstrates a comprehensive data warehousing and analytics solution, from building a data warehouse to generating actionable insights. Designed as a portfolio project, it highlights industry best practices in data engineering and analytics.

---
## 🏗️ Data Architecture

The data architecture for this project follows Medallion Architecture **Bronze**, **Silver**, and **Gold** layers:
![Data Architecture](docs/data_architecture.png)

1. **Bronze Layer**: Stores raw data as-is from the source systems. Data is ingested from CSV Files into SQL Server Database.
2. **Silver Layer**: This layer includes data cleansing, standardization, and normalization processes to prepare data for analysis.
3. **Gold Layer**: Houses business-ready data modeled into a star schema required for reporting and analytics.

---
## 📖 Project Overview

This project involves:

1. **Data Architecture**: Designing a Modern Data Warehouse Using Medallion Architecture **Bronze**, **Silver**, and **Gold** layers.
2. **ETL Pipelines**: Extracting, transforming, and loading data from source systems into the warehouse.
3. **Data Modeling**: Developing fact and dimension tables optimized for analytical queries.
4. **Analytics & Reporting**: Creating SQL-based reports and dashboards for actionable insights.

🎯 This repository is an excellent resource for professionals and students looking to showcase expertise in:
- SQL Development
- Data Architect
- Data Engineering  
- ETL Pipeline Developer  
- Data Modeling  
- Data Analytics  

---

## 🛠️ Important Links & Tools:

Everything is for Free!
- **[Datasets](datasets/):** Access to the project dataset (csv files).
- **[SQL Server Express](https://www.microsoft.com/en-us/sql-server/sql-server-downloads):** Lightweight server for hosting your SQL database.
- **[SQL Server Management Studio (SSMS)](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16):** GUI for managing and interacting with databases.
- **[Git Repository](https://github.com/):** Set up a GitHub account and repository to manage, version, and collaborate on your code efficiently.
- **[DrawIO](https://www.drawio.com/):** Design data architecture, models, flows, and diagrams.
- **[Notion](https://www.notion.com/templates/sql-data-warehouse-project):** Get the Project Template from Notion
- **[Notion Project Steps](https://thankful-pangolin-2ca.notion.site/SQL-Data-Warehouse-Project-16ed041640ef80489667cfe2f380b269?pvs=4):** Access to All Project Phases and Tasks.

---

## 🚀 Project Requirements

### Building the Data Warehouse (Data Engineering)

#### Objective
Develop a modern data warehouse using SQL Server to consolidate sales data, enabling analytical reporting and informed decision-making.

#### Specifications
- **Data Sources**: Import data from two source systems (ERP and CRM) provided as CSV files.
- **Data Quality**: Cleanse and resolve data quality issues prior to analysis.
- **Integration**: Combine both sources into a single, user-friendly data model designed for analytical queries.
- **Scope**: Focus on the latest dataset only; historization of data is not required.
- **Documentation**: Provide clear documentation of the data model to support both business stakeholders and analytics teams.

---

### BI: Analytics & Reporting (Data Analysis)

#### Objective
Develop SQL-based analytics to deliver detailed insights into:
- **Customer Behavior**
- **Product Performance**
- **Sales Trends**

These insights empower stakeholders with key business metrics, enabling strategic decision-making.  

For more details, refer to [docs/requirements.md](docs/requirements.md).

## 📂 Repository Structure
```
data-warehouse-project/
│
├── datasets/                           # Raw datasets used for the project (ERP and CRM data)
│
├── docs/                               # Project documentation and architecture details
│   ├── etl.drawio                      # Draw.io file shows all different techniquies and methods of ETL
│   ├── data_architecture.drawio        # Draw.io file shows the project's architecture
│   ├── data_catalog.md                 # Catalog of datasets, including field descriptions and metadata
│   ├── data_flow.drawio                # Draw.io file for the data flow diagram
│   ├── data_models.drawio              # Draw.io file for data models (star schema)
│   ├── naming-conventions.md           # Consistent naming guidelines for tables, columns, and files
│
├── scripts/                            # SQL scripts for ETL and transformations
│   ├── bronze/                         # Scripts for extracting and loading raw data
│   ├── silver/                         # Scripts for cleaning and transforming data
│   ├── gold/                           # Scripts for creating analytical models
│
├── tests/                              # Test scripts and quality files
│
├── README.md                           # Project overview and instructions
├── LICENSE                             # License information for the repository
├── .gitignore                          # Files and directories to be ignored by Git
└── requirements.txt                    # Dependencies and requirements for the project
```
---

## 💼 What is **CRM**?

**CRM** stands for **Customer Relationship Management**.

* It's a system used to manage a company’s interactions with **current and potential customers**.
* CRM systems help in **sales, marketing, support**, and tracking customer behaviors.
* The **CRM tables** in your project are most likely populated from a CRM software (like Salesforce, Zoho, HubSpot, etc.).

### 🧾 Example CRM Tables in Your Case:

| Table               | Meaning                                          |
| ------------------- | ------------------------------------------------ |
| `crm_cust_info`     | Customer details like name, contact, email, etc. |
| `crm_prd_info`      | Product catalog visible to customers             |
| `crm_sales_details` | Sales transactions involving customers           |

---

# 📘 CSV Source File Documentation

This dataset contains three main CSVs that capture **customer information**, **product catalog**, and **sales transactions**.
Below is the documentation for each file and its column headings.

---

## 1. `crm_cust_info` — Customer Information

Contains details of customers including demographic attributes and creation date.

| Column Name            | Description                                                        | Example      |
| ---------------------- | ------------------------------------------------------------------ | ------------ |
| **cst_id**             | Unique numeric identifier for the customer (surrogate key)         | `11000`      |
| **cst_key**            | Business key / alternate key used in external systems              | `AW00011000` |
| **cst_firstname**      | Customer’s first name                                              | `Jon`        |
| **cst_lastname**       | Customer’s last name                                               | `Yang`       |
| **cst_marital_status** | Marital status of the customer (`M` = Married, `S` = Single, etc.) | `M`          |
| **cst_gndr**           | Gender of the customer (`M` = Male, `F` = Female)                  | `M`          |
| **cst_create_date**    | Date when the customer record was created                          | `06-10-2025` |

---

## 2. `crm_prd_info` — Product Catalog

Represents product details visible to customers, including product line classification.

| Column Name      | Description                                                                                                                                                        | Example                     |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------- |
| **prd_id**       | Unique numeric identifier for the product (surrogate key)                                                                                                          | `210`                       |
| **prd_key**      | Business key representing product code                                                                                                                             | `CO-RF-FR-R92B-58`          |
| **prd_nm**       | Product name / description                                                                                                                                         | `HL Road Frame - Black- 58` |
| **prd_cost**     | Standard cost or unit price of the product                                                                                                                         | `12`                        |
| **prd_line**     | Product Line category (high-level classification): <br>• `R` = Road <br>• `M` = Mountain <br>• `T` = Touring <br>• `S` = Standard (accessories, helmets, clothing) | `R`                         |
| **prd_start_dt** | Date when the product became available                                                                                                                             | `01-07-2003`                |
| **prd_end_dt**   | Date when the product was discontinued (blank if still active)                                                                                                     | `28-12-2007`                |

---

## 3. `crm_sales_details` — Sales Transactions

Captures sales orders including products purchased, customer IDs, dates, and financial details.

| Column Name      | Description                                                                                   | Example      |
| ---------------- | --------------------------------------------------------------------------------------------- | ------------ |
| **sls_ord_num**  | Sales order number (unique identifier for transaction)                                        | `SO43697`    |
| **sls_prd_key**  | Product key (links to `prd_key` in `crm_prd_info`)                                            | `BK-R93R-62` |
| **sls_cust_id**  | Customer ID (links to `cst_id` in `crm_cust_info`)                                            | `21768`      |
| **sls_order_dt** | Date when the order was placed                                                                | `20101229`   |
| **sls_ship_dt**  | Date when the order was shipped                                                               | `20110105`   |
| **sls_due_dt**   | Due date for payment or delivery                                                              | `20110110`   |
| **sls_sales**    | Total sales amount for the transaction                                                        | `3578`       |
| **sls_quantity** | Quantity of items ordered                                                                     | `1`          |
| **sls_price**    | Unit price at which the product was sold (may differ from `prd_cost` due to markup/discounts) | `3578`       |

---

## 🏭 What is **ERP**?

**ERP** stands for **Enterprise Resource Planning**.

* It’s a system used to **manage business operations** like **inventory, procurement, HR, accounting, logistics**, etc.
* ERP systems integrate internal data across departments.
* Think of systems like SAP, Oracle ERP, Microsoft Dynamics, etc.

### 🗂️ Example ERP Tables in Your Case:

| Table             | Meaning                                                   |
| ----------------- | --------------------------------------------------------- |
| `erp_loc_a101`    | Possibly physical location or warehouse/store details     |
| `erp_cust_az12`   | Master customer data from ERP side (may differ from CRM!) |
| `erp_px_cat_g1v2` | Product categories, pricing, or inventory classifications |

---

# 📘 CSV Source File Documentation (Extended — ERP Files)

---

## 4. `erp_loc_a101` — Location Information

Represents physical locations such as countries where customers, warehouses, or stores exist.

| Column Name | Description                                                                 | Example         |
| ----------- | --------------------------------------------------------------------------- | --------------- |
| **CID**     | Location identifier / code (primary key or foreign key in related datasets) | `NASAW00011000` |
| **CNTRY**   | Country name (geographic location)                                          | `United States` |

**Notes on `CNTRY` values:**

* Valid country names include: *Australia, US, Canada, DE, United Kingdom, France, USA, Germany, United States*.
* Some entries contain **duplicates or inconsistent naming** (e.g., `US` vs `USA` vs `United States`, `DE` vs `Germany`).
* `0` may represent a missing or invalid entry.
  👉 In practice, this should be standardized for reporting/analysis.

---

## 5. `erp_cust_az12` — ERP Customer Master Data

Master customer records maintained in ERP, which may differ from CRM (`crm_cust_info`).

| Column Name | Description                                                           | Example         |
| ----------- | --------------------------------------------------------------------- | --------------- |
| **CID**     | Unique customer identifier in ERP system                              | `NASAW00011000` |
| **BDATE**   | Birth date of customer                                                | `06-10-1971`    |
| **GEN**     | Gender of customer (`Male`, `Female`, or other values depending data) | `Male`          |

---

## 6. `erp_px_cat_g1v2` — Product Categories and Classifications

Represents product categories, subcategories, and possibly maintenance flags for catalog and pricing.

| Column Name     | Description                                                                 | Example          |
| --------------- | --------------------------------------------------------------------------- | ---------------- |
| **ID**          | Unique identifier for category/subcategory entry                            | `101`            |
| **CAT**         | High-level product category                                                 | `Bikes`          |
| **SUBCAT**      | Detailed subcategory within the category                                    | `Mountain Bikes` |
| **MAINTENANCE** | Field reserved for maintenance flag or notes (may indicate active/inactive) | *blank / value*  |

**Valid values in `CAT`:**

* Accessories
* Bikes
* Clothing
* Components

**Valid values in `SUBCAT`:**

* **Accessories:** Bike Racks, Bike Stands, Bottles and Cages, Cleaners, Fenders, Helmets, Hydration Packs, Lights, Locks, Panniers, Pumps, Tires and Tubes
* **Bikes:** Mountain Bikes, Road Bikes, Touring Bikes
* **Clothing:** Bib-Shorts, Caps, Gloves, Jerseys, Shorts, Socks, Tights, Vests
* **Components:** Bottom Brackets, Brakes, Chains, Cranksets, Derailleurs, Forks, Handlebars, Headsets, Mountain Frames, Pedals, Road Frames, Saddles, Touring Frames, Wheels

---

✅ Now your CRM + ERP files have **uniform documentation**.

* CRM deals with **customers, products, sales** (customer-facing).
* ERP deals with **locations, customer master, and product categories** (back-office).

## 🎯 CRM vs ERP in Simple Terms:

| Feature  | CRM                             | ERP                                  |
| -------- | ------------------------------- | ------------------------------------ |
| Focus    | Customers                       | Business operations                  |
| Users    | Sales, Marketing, Support teams | Supply chain, Finance, HR, Logistics |
| Examples | Contact mgmt, customer sales    | Inventory, payroll, procurement      |

---
