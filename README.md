# Real-Time Quick Commerce Data Pipeline & Volatility Engine

[![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=Databricks&logoColor=white)](https://databricks.com/)
[![AWS S3](https://img.shields.io/badge/Amazon_S3-569A31?style=for-the-badge&logo=amazon-s3&logoColor=white)](https://aws.amazon.com/s3/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Apache Spark](https://img.shields.io/badge/Apache_Spark-E25A1C?style=for-the-badge&logo=apache-spark&logoColor=white)](https://spark.apache.org/)
[![Delta Lake](https://img.shields.io/badge/Delta_Lake-00AEEF?style=for-the-badge&logo=delta&logoColor=white)](https://delta.io/)

**Watch the full project walkthrough and Time Travel demonstration here:**  
▶️ **[https://youtu.be/KHTv2OkZEnM]**

## Project Overview
This project is an end-to-end data engineering pipeline built on **Databricks** utilizing the **Medallion Architecture (Bronze, Silver, Gold)**. It is designed to process simulated real-time e-commerce data for the Indian quick-commerce market, specifically handling complex real-world data scenarios like late-arriving facts, payment gateway failures and slowly changing dimensions (SCD).

The pipeline continuously ingests 8 days of transactional data from AWS S3, cleanses it, merges state changes, and models it for downstream Business Intelligence.

## Architecture & Tech Stack

![Medallion Architecture Flow](/Workspace/Repos/nandhuvinoth95@gmail.com/ecomlake/architecture/work_flow_architecture.png)

*   **Cloud Storage:** AWS S3 (Landing Zone)
*   **Compute & Orchestration:** Databricks Workflows (Job Clusters)
*   **Data Processing:** PySpark & Databricks SQL
*   **Storage Format:** Delta Lake
*   **Visualization:** Databricks SQL Dashboards

## Key Engineering Achievements

*   **Stateful Streaming Ingestion:** Implemented Databricks **Auto Loader** with checkpointing to incrementally ingest thousands of JSON and CSV files, preventing data duplication without requiring manual tracking.
*   **Change Data Capture (CDC):** Built robust PySpark `MERGE` statements in the Silver layer to handle **SCD Type 1 (Overwrites)** for dynamic pricing and **SCD Type 2 (Historical Tracking)** for customer tier upgrades.
*   **Resilience to Asynchronous Data:** Designed the pipeline to gracefully handle missing daily batches (e.g., delayed shipment logs) and seamlessly process late-arriving facts without breaking downstream Gold aggregations.
*   **Data Governance & Time Travel:** Utilized Delta Lake's native transaction logs to perform "Time Travel" queries, recovering previous states of order statuses, and implemented a Data Quality dashboard to track nulls and malformed records.

---

## Business Intelligence & Dashboards

### 1. The Cash Flow Engine (GMV vs. NMV)
*Tracks the discrepancy between Gross Merchandise Value and Net Merchandise Value caused by payment gateway failures across an 8-day continuous trend.*
Cash Flow Dashboard
*(![The Cash Flow Engine](/Workspace/Repos/nandhuvinoth95@gmail.com/ecomlake/Dashboard/The Cash Flow Engine.png))*

### 2. Operational SLAs & Seller Performance
*Visualizes shipment delivery SLAs, return trends, and isolates top-performing sellers across varying tiers.*
Seller Performance
*(![Seller Performance](/Workspace/Repos/nandhuvinoth95@gmail.com/ecomlake/Dashboard/Seller Performance.png))*

### 3. Customer Lifetime Value (CLV) & Tier Segmentation
*Analyzes purchasing behavior to calculate Customer Lifetime Value (CLV), segmenting users into loyalty tiers (Bronze, Silver, Gold, Platinum) and identifying the highest-value customer cohorts to drive targeted retention strategies*
Customer Value
*(![Customer Lifetime Value](/Workspace/Repos/nandhuvinoth95@gmail.com/ecomlake/Dashboard/Customer Value.png))*

### 4. Inventory Health & Stockout Risk Map
*Visualizes real-time stock levels across multiple warehouses, calculating the stockout risk percentage by comparing current available inventory against dynamic reorder thresholds to prevent lost sales.*
Stock Risk Map
*(![Stock Risk Map](/Workspace/Repos/nandhuvinoth95@gmail.com/ecomlake/Dashboard/Stock Risk Map.png))*

---

## 📂 Repository Structure

<pre>
ecomlake/
├── README.md                                 # Main project documentation
│
├── architecture/                             # System & Workflow Diagrams
│   ├── pipeline_diagram.png                  # High-level data architecture
│   └── work_flow.png                         # Databricks Job DAG execution graph
│
├── Dashboard/                                # BI & Visualization Exports
│   ├── Customer Value.png  
│   ├── The Cash Flow.png     
│   ├── Stockout Risk Map.png           
│   └── Performance Matrix.png           
│
├── setup/
│   └── create_delete_test_databases.py       # Environment & catalog initialization
│
├── scripts/                                  # Raw Data Generators
│   ├── raw_data_generation_batch_1.py        # Initial load simulation
│   ├── raw_data_generation_batch_2.py        # CDC / Updates simulation
│   └── raw_data_generation_batch_3.py        # Late-arriving facts simulation
│
├── bronze/                                   # Auto Loader Ingestion Layer
│   ├── bronze_customers.py             
│   ├── bronze_inventory.py
│   ├── bronze_order_items.py
│   ├── bronze_orders.py
│   ├── bronze_payments.py
│   ├── bronze_products.py
│   ├── bronze_returns.py
│   ├── bronze_sellers.py
│   └── bronze_shipments.py
│
├── silver/                                   # Cleansing & CDC Merge Layer
│   ├── silver_customers_scd.py               # SCD Type 2 logic
│   ├── silver_inventory.py
│   ├── silver_order_items.py
│   ├── silver_orders_cdc.py                  # CDC & SCD Type 1 logic
│   ├── silver_payments.py
│   ├── silver_products.py
│   ├── silver_returns.py
│   ├── silver_sellers_scd.py
│   └── silver_shipments.py
│
├── gold/                                     # Business Aggregation Layer
│   ├── gold_customer_lifetime_value.py 
│   ├── gold_daily_sales_summary.py    
│   ├── gold_delivery_sla_report.py
│   ├── gold_inventory_health_&_stockout_risk.py
│   ├── gold_product_returns_trend.py
│   └── gold_seller_performance.py
│
├── data_quality/
│   └── data_quality.py                       # Pipeline health & null checks
│
├── quick_stats/
│   └── verify_queries.sql                    # Ad-hoc validation & testing queries
│
└── time_travel/
    └── time_travel_demo.py                   # Delta Lake versioning demonstration
    </pre>