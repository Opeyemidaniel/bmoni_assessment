# BMONI Assessment Data Architecture

This repository contains the data pipeline architecture for **BMONI Assessment**, which ingests data from **Amazon S3**, transforms it in **Amazon Redshift** using **dbt**, and visualizes it in **Power BI**.  
All credentials are securely managed using **AWS Secrets Manager**, and the entire project (Python + dbt) is **version-controlled via GitHub**.

---

##  Architecture Overview

---

## Components and Workflow

### **Amazon S3 – Data Source Layer**
- Stores **raw or semi-structured** data (CSV, JSON, Parquet, etc.).
- Acts as the **data lake** for source ingestion.
- Source for ETL jobs that load data into Redshift.

---

### **Python (ETL Ingestion Layer)**
- Extracts data from **Amazon S3** and loads it into the **staging schema** in Redshift.
- Uses:
  - `boto3` for S3 and Secrets Manager access.
  - `redshift-data` or `psycopg2` for database interaction.
- Retrieves credentials securely from **AWS Secrets Manager**.
- Code is version-controlled and CI/CD-enabled via **GitHub**.

---

### **Amazon Redshift – Data Warehouse Layer**
- Serves as the **central data repository**.
- Contains two main schemas:
  - **Staging:** Holds raw ingested data.
  - **Production:** Holds cleaned and modeled data from dbt transformations.

---

### **dbt (Data Build Tool) – Transformation Layer**
- Performs **SQL-based data modeling and transformation**.
- Reads data from **staging**, applies models, and writes to **production**.
- Handles:
  - Data testing (`dbt test`)
  - Documentation (`dbt docs`)
  - Incremental model builds
- Fully version-controlled in **GitHub**.
- CI/CD automation via **GitHub Actions** or **dbt Cloud**.

---

### **GitHub – Version Control & CI/CD**
- Stores and manages all Python ETL and dbt project code.
- Supports collaborative development (branches, pull requests).
- CI/CD pipelines automate:
  - Testing and validation (unit and integration tests)
  - Deployment of dbt models to Redshift
- Integrations:
  - **GitHub Actions** for workflow automation.
  - **AWS CodeBuild** or **dbt Cloud** for deployments.

---

###  **AWS Secrets Manager – Security Layer**
- Manages and rotates sensitive credentials:
  - Redshift usernames and passwords
  - S3 access tokens
- Accessed programmatically by both **Python scripts** and **dbt**.
- Eliminates the need to hardcode secrets in code or environment files.

---

### **Power BI – Visualization Layer**
- Connects directly to **Amazon Redshift (production schema)**.
- Provides:
  - Executive dashboards
  - Operational metrics
  - Ad-hoc analysis
- Enables **data-driven decision-making** using the transformed data.

---

## **End-to-End Data Flow Summary**

| Step | Process | Tool/Technology |
|------|----------|----------------|
| 1 | Raw data collected and stored | **Amazon S3** |
| 2 | Python ETL script loads data into staging | **Python + Redshift Data API** |
| 3 | Transform and model data | **dbt** |
| 4 | Version control and CI/CD automation | **GitHub** |
| 5 | Secure credentials | **AWS Secrets Manager** |
| 6 | Visualize curated data | **Power BI** |

---

## **Deployment and Automation**

1. **Developers** push Python/dbt changes to GitHub.  
2. **GitHub Actions** triggers CI/CD workflows:
   - Run `dbt run` and `dbt test` on staging.
   - Deploy approved changes to production.
3. **AWS Secrets Manager** provides credentials dynamically during the process.
4. **Power BI** automatically refreshes dashboards with new data.

---

## **Tech Stack Summary**

| Layer | Technology | Purpose |
|-------|-------------|----------|
| Storage | **Amazon S3** | Raw data lake |
| Ingestion | **Python (boto3, psycopg2)** | Data loading to Redshift |
| Warehouse | **Amazon Redshift** | Central data store |
| Transformation | **dbt** | SQL-based modeling |
| Orchestration | **GitHub Actions / dbt Cloud** | CI/CD pipeline |
| Security | **AWS Secrets Manager** | Secret management |
| Analytics | **Power BI** | Visualization and insights |

---



