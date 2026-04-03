# NYC Taxi Data Analytics: End-to-End Medallion Pipeline
### Azure Databricks | Spark 4.1.0 | Delta Lake | ADLS Gen2

## 📌 Project Overview
This project demonstrates a production-grade **Medallion Architecture** (Bronze-Silver-Gold) data pipeline. I ingested 50,000 records of raw NYC Taxi trip data into **Azure Data Lake Storage (Gen2)**, performed complex transformations using **PySpark**, and delivered an aggregated "Gold" table ready for business executive reporting.



---

## 🏗️ Technical Architecture
* **Cloud Provider:** Azure
* **Data Lake:** Azure Data Lake Storage (ADLS Gen2)
* **Orchestration & Compute:** Azure Databricks (Single Node Cluster)
* **Runtime:** Spark 4.1.0, Scala 2.13
* **Table Format:** Delta Lake (for ACID transactions and Time Travel)
* **Security:** OAuth 2.0 via Azure Service Principals (Entra ID)

---

## ⚡ Pipeline Stages

### 🟢 01. Bronze Layer (Raw Ingestion)
* **Source:** Public Databricks NYC Taxi Delta dataset.
* **Process:** Authenticated via Service Principal to securely move data into a private storage container.
* **Key Achievement:** Implemented idempotent logic (cleanup before write) to ensure pipeline reliability.

### ⚪ 02. Silver Layer (Transformation & Quality)
* **Schema Audit:** Verified and standardized column names to a consistent `snake_case` schema.
* **Data Quality:** Filtered out system anomalies (records with 0 passengers or 0 trip distance).
* **Feature Engineering:** Calculated `trip_duration_minutes` by deriving the delta between pickup and dropoff timestamps.

### 🟡 03. Gold Layer (Business Intelligence)
* **Aggregation:** Grouped refined data by `vendor_id` to calculate high-level KPIs.
* **KPIs:** Total Rides, Average Trip Distance, Average Tip Amount, and Total Revenue.
* **Outcome:** A high-performance summary table optimized for Power BI/Tableau visualization.

---

## 🚀 How to Deploy
1. **Infrastructure:** Create an Azure Storage Account (ADLS Gen2) with `raw-data` and `transformed-data` containers.
2. **Security:** Register an App in Entra ID and grant `Storage Blob Data Contributor` permissions to the storage account.
3. **Configuration:** Update the `client_id`, `tenant_id`, and `client_secret` in the notebook setup cells (use Databricks Secrets for production).
4. **Execution:** Run the notebooks in sequence: `01_Bronze_Ingestion` -> `02_Silver_Transformation` -> `03_Gold_Analysis`.

---

## 📊 Key Business Insight
The pipeline successfully identified revenue variations between vendors, revealing that while trip counts may be similar, tip percentages vary significantly based on trip duration—providing a data-driven foundation for vendor performance reviews.

---

## 🛡️ Security Note
All sensitive credentials (Client IDs/Secrets) have been **redacted** in this public repository following security best practices. In a production environment, these are managed via **Databricks Secret Scopes**.
