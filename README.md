
# Metadata-Driven Manufacturing Lakehouse

---
<img width="1400" height="720" alt="image" src="https://github.com/user-attachments/assets/786d79ba-95fc-43a1-b67e-2d3d7e1a3580" />

---

### An End-to-End Azure Data Engineering Platform
**Azure Data Factory | ADLS Gen2 | Azure Databricks | PySpark | Delta Lake**


## Project Overview

Modern manufacturing organizations generate large volumes of data across production lines, enterprise resource planning, quality management, inventory operations and dealer networks. However, this information is often distributed across disconnected operational systems.

This project demonstrates the design and implementation of a **metadata-driven Azure Data Engineering platform** for a simulated automotive manufacturing company.

The platform integrates data from four operational systems — MES, ERP, QMS and DMS — into a centralized Azure Data Lakehouse.

Azure Data Factory manages metadata-driven ingestion and pipeline orchestration, while Azure Databricks processes and transforms data using PySpark and Delta Lake.

The solution follows the **Medallion Architecture (Bronze, Silver and Gold)** to support reliable data processing, business-level analytics and downstream Power BI reporting.

The primary objective is to build a scalable, maintainable and reusable data platform rather than a collection of independent ETL pipelines.

---

## 1. Business Context & Data Sources

The simulated automotive manufacturing environment consists of four operational domains.

| Source System | Business Domain | Data Description |
|---|---|---|
| MES | Manufacturing Execution System | Production orders, machine operations and shop-floor activities |
| ERP | Enterprise Resource Planning | Materials, suppliers, purchase orders, inventory and goods receipts |
| QMS | Quality Management System | Quality inspections, defects and inspection results |
| DMS | Dealer Management System | Dealer information and vehicle inventory |

**Source environment:** 4 operational databases containing 20 source tables.

The objective is to consolidate these datasets into a unified analytical platform while preserving source-level traceability and applying consistent transformation rules.

---

## 2. Solution Architecture

The platform combines metadata-driven ingestion using Azure Data Factory with a Medallion Lakehouse architecture implemented using Azure Databricks.

### End-to-End Data Flow

<img width="796" height="268" alt="End to End Architecture" src="https://github.com/user-attachments/assets/1f31214a-e16e-4f2e-94d6-67b31bb3de83" />

### Technology Stack

| Technology | Role in the Platform |
|---|---|
| SQL Server | Operational source databases |
| Azure Data Factory | Metadata-driven orchestration and incremental ingestion |
| ADLS Gen2 | Centralized cloud data lake storage |
| Azure Databricks | Distributed data processing and transformation |
| PySpark | Data cleansing, transformation and business logic |
| Delta Lake | ACID transactions and reliable table storage |
| Unity Catalog | Data governance and table management |
| Power BI | Business intelligence and reporting |

---

## 3. Medallion Architecture

### Bronze Layer — Raw Data Ingestion

**Objective:** Ingest and preserve operational source data.

- Extract data from MES, ERP, QMS and DMS databases.
- Use Azure Data Factory for metadata-driven ingestion.
- Implement watermark-based incremental data extraction.
- Store ingested datasets in ADLS Gen2.
- Preserve source-level data traceability.

**Processing:** SQL Server → Azure Data Factory → ADLS Gen2.

### Silver Layer — Data Cleansing & Standardization

**Objective:** Convert raw source datasets into reliable and standardized Delta tables.

- Read Bronze datasets using PySpark.
- Apply schema validation and standardization.
- Handle duplicate and inconsistent records.
- Implement data quality validation.
- Apply domain-specific business rules.
- Create curated Delta tables.

**Domain-level processing:**

| Notebook | Responsibility |
|---|---|
| Silver_MES | Manufacturing domain transformations |
| Silver_ERP | Enterprise resource and inventory transformations |
| Silver_QMS | Quality domain transformations |
| Silver_DMS | Dealer and vehicle inventory transformations |

### Gold Layer — Business-Level Analytics

**Objective:** Build analytical datasets that support business decision-making.

| Analytical Domain | Notebook | Business Purpose |
|---|---|---|
| Manufacturing | Gold_Manufacturing | Production performance and manufacturing operations |
| Quality | Gold_Quality | Inspection results, defects and quality performance |
| Inventory | Gold_Inventory | Stock levels, inventory movement and material availability |
| Sales | Gold_Sales | Dealer sales and sales performance |
| Procurement & Supplier | Gold_Procurement_Supplier | Supplier performance and procurement analytics |
| Production Planning | Gold_Production_Planning | Production planning and material requirements |
| Executive Summary | Gold_Executive_Summary | Consolidated business performance indicators |

The resulting Gold datasets are designed for consumption through Power BI.

---

## 4. Metadata-Driven Azure Data Factory Framework

A central design principle of this project is metadata-driven ingestion.

Instead of maintaining separate ingestion pipelines for individual source tables, a reusable pipeline reads configuration information from a metadata repository.

### Pipeline Execution Flow

<img width="1221" height="544" alt="image" src="https://github.com/user-attachments/assets/b70014ff-4ad4-4b05-8f5f-f0738e321a07" />


### Metadata Configuration

| Metadata Attribute | Description |
|---|---|
| SourceSystem | MES, ERP, QMS or DMS |
| SourceSchema | Source database schema |
| SourceTable | Operational source table |
| LoadType | Full or incremental load |
| WatermarkColumn | Column used for incremental extraction |
| TargetFolder | Destination folder in ADLS Gen2 |
| IsActive | Controls whether the table participates in ingestion |

This approach separates pipeline logic from source-specific configuration, making the ingestion framework easier to maintain and extend.

---

## 5. Databricks Processing & Job Orchestration

Azure Databricks manages the transformation stages using domain-oriented notebooks.

| Layer | Number of Notebooks | Processing Scope |
|---|---:|---|
| Bronze | 1 | Bronze ingestion |
| Silver | 4 | MES, ERP, QMS and DMS |
| Gold | 7 | Manufacturing, Quality, Inventory, Sales, Procurement & Supplier, Production Planning and Executive Summary |
| **Total** | **12** | **End-to-end processing** |

Databricks Workflows are designed to orchestrate notebook execution using task dependencies.

The intended execution sequence is:

1. Bronze ingestion completes.
2. Silver domain notebooks execute after Bronze.
3. Core Gold analytical notebooks execute after their required Silver dependencies.
4. Advanced Gold analytical notebooks execute after their upstream analytical dependencies.
5. Executive Summary is generated after its required Gold datasets are available.

Azure Data Factory acts as the external orchestration layer, while Databricks manages notebook-level execution.

---

## 6. Data Engineering Practices

- Metadata-driven ingestion and orchestration
- Watermark-based incremental extraction
- Medallion Lakehouse Architecture
- Distributed processing with PySpark
- Delta Lake table management
- Schema validation and data quality checks
- Deduplication and data standardization
- Incremental data processing
- Domain-based transformation design
- Databricks task dependencies
- Business-oriented analytical data modeling

---

---

## 7. Implementation Status

| Component | Current Status |
|---|---|
| SQL Server source databases | Completed |
| Azure Data Factory ingestion pipeline | Implemented |
| Metadata-driven ingestion | Implemented |
| ADLS Gen2 Bronze ingestion | Implemented |
| Databricks Silver transformations | Implemented |
| Gold analytical datasets | Implemented |
| Databricks Job orchestration | Implemented |
| Power BI dashboards | Planned |

Implementation status will be updated as each component is completed and validated.

---

## 8. Expected Business Outcome

The completed platform aims to provide a unified analytical foundation for automotive manufacturing operations.

By integrating data across manufacturing, enterprise resource planning, quality management and dealer systems, the solution will support:

- Manufacturing and production performance monitoring.
- Inventory and material availability analysis.
- Quality inspection and defect analysis.
- Supplier and procurement performance analysis.
- Production planning insights.
- Consolidated executive-level business reporting.

The architecture is designed to be reusable and extensible, allowing additional source tables and business domains to be onboarded through configuration and modular processing.

---

---

**Project Focus:** Metadata-Driven Data Engineering | Azure Lakehouse | Manufacturing Analytics

**Technology:** Microsoft Azure | Azure Data Factory | ADLS Gen2 | Azure Databricks | PySpark | Delta Lake | SQL Server | Power BI







