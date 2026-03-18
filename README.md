# Eternals-Retail-Grocery-chain

🛒 FreshMart Retail Data Pipeline & Analytics
Project Overview
FreshMart is a robust End-to-End Data Engineering pipeline built on Snowflake. It ingests raw retail data (Sales, Inventory, Products, Stores, and Promotions), cleans it using advanced Slowly Changing Dimension (SCD-2) logic, and transforms it into a Star Schema for real-time Business Intelligence and KPI tracking.

🏗️ Architecture Layers
The project follows a modular Medallion-style architecture:

RAW Layer: Landing zone for CSV files via Internal Stages.

CLEANSING Layer: The "Engine Room." Handles deduplication, data type casting, and SCD-TYPE 2 history tracking for Products and Stores.

AUDIT Layer: Integrated logging for ingestion metrics and Anomaly Detection (Price drops, stockouts, and duplicate row logging).

ANALYTICS Layer: A Star Schema (Fact & Dimension tables) optimized for BI tools and fast KPI calculation.

🛠️ Key Features
1. Intelligent SCD-2 Processing
Unlike simple overwrites, this pipeline tracks history. If a product price changes (e.g., from 306 to 305) or a store type is updated, the system:

Automatically marks the old record as Inactive.

Inserts the new record as Active.

Ensures the Fact Table always links to the price that was valid at the time of sale.

2. Pre-emptive Audit & Anomaly Detection
The pipeline doesn't just delete bad data—it logs it.

Duplicate Detection: All duplicate rows found in RAW files are logged to AUDIT.ANOMALY_LOGS before being filtered.

Business Rules: Detects "Fake Promos" (discounts higher than official rates) and "Stockout Anomalies" (sales occurring when inventory is 0).

3. Optimized Star Schema
We implemented a Star Schema to separate high-volume transactions (FACT_SALES) from descriptive attributes (DIM_PRODUCTS, DIM_STORES), ensuring sub-second query performance for management dashboards.

📊 Retail KPIs Tracked
The system provides instant insights into:

Stockout Rate: Percentage of product-store combinations with zero availability.

Inventory Turnover: How quickly stock is being sold and replaced.

Revenue per Store (RPS): Performance benchmarking across locations.

Category Contribution: Sales distribution across Snacks, Grocery, Electronics, etc.

🚀 Technical Stack
Cloud Data Warehouse: Snowflake

Language: SQL (Snowflake Scripting)

Automation: Stored Procedures (SP_RUN_MANUAL_CLEANING, SP_INGEST_RAW_DATA)

Data Modeling: Star Schema (Fact/Dimensions), SCD Type 2

Tools: Internal Stages, File Formats, CTEs, Window Functions (QUALIFY ROW_NUMBER)
