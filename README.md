# DATACONCEPTS


# Data Lake vs Data Warehouse

Understanding the difference between a **Data Lake** and a **Data Warehouse** is fundamental for Data Engineering.

## Data Lake vs Data Warehouse

| Concept        | Data Lake                                   | Data Warehouse               |
| -------------- | ------------------------------------------- | ---------------------------- |
| **Data**       | Raw + Processed                             | Usually Cleaned + Structured |
| **Schema**     | Schema-on-Read                              | Schema-on-Write              |
| **Size**       | Very large / massive                        | Large                        |
| **Data Types** | Structured + Semi-structured + Unstructured | Mainly Structured            |
| **Main Usage** | Data Engineering, Big Data, ML              | BI, Reporting, Analytics     |
| **Examples**   | S3, ADLS, HDFS                              | Snowflake, Teradata          |

---

## What is a Data Lake?

A **Data Lake** is a storage system designed to store large amounts of data in its original or raw form.

It can contain:

* Structured data
* Semi-structured data
* Unstructured data
* Logs
* JSON
* CSV
* Parquet
* Images
* Text
* Machine Learning datasets

The data does not necessarily need to be transformed before being stored.

### Example

```text
Data Sources
     ↓
  Data Lake
     ↓
Raw Data
     ↓
Processing
     ↓
Clean / Transformed Data
```

Common Data Lake technologies include:

* Amazon S3
* Azure Data Lake Storage (ADLS)
* HDFS

---

## What is a Data Warehouse?

A **Data Warehouse** is designed to store structured, cleaned, and organized data for analytics and reporting.

It is commonly used by:

* Business Intelligence (BI)
* Data Analysts
* Reporting systems
* Dashboards
* Business analytics

### Example

```text
Data Sources
     ↓
     ETL
     ↓
Cleaned Data
     ↓
Data Warehouse
     ↓
BI / Reporting
```

Examples:

* Snowflake
* Teradata
* Amazon Redshift
* Google BigQuery

---

# Where does Spark come in?

This is where **Apache Spark** becomes important for Data Engineering.

Spark is **not a Data Lake or Data Warehouse**.

Spark is a **distributed data processing engine**.

It can read data from different sources, process large datasets, transform them, join them, clean them, and write the results back to storage systems.

```text
Data Sources
     ↓
  Data Lake
     ↓
   Apache Spark
     ↓
Transform / Clean / Join
     ↓
Processed Data
     ↓
Data Warehouse / Analytics
```

---

# Spark in a Cloudera Environment

In a **Cloudera** environment, you may encounter a flow such as:

```text
HDFS
  ↓
 Hive
  ↓
 Spark
  ↓
 Impala
```

Each component has a different role.

### HDFS

**HDFS (Hadoop Distributed File System)** is a distributed storage system.

```text
HDFS
→ Stores large amounts of data
```

### Hive

**Apache Hive** provides SQL-based access to data stored in systems such as HDFS.

```text
Hive
→ SQL
→ Tables
→ Metadata
→ Data Warehousing
```

Example:

```sql
SELECT *
FROM database.table;
```

### Spark

**Apache Spark** is used for large-scale data processing.

```text
Spark
→ Read
→ Transform
→ Clean
→ Join
→ Aggregate
→ Write
```

Spark can process data using:

* Spark SQL
* PySpark
* Scala
* Java

### Impala

**Impala** is a distributed SQL query engine commonly used in Cloudera environments for fast interactive SQL queries.

```text
Impala
→ Fast SQL queries
→ Analytics
→ Data exploration
```

---

# Example Data Engineering Pipeline

A typical pipeline can look like this:

```text
        Data Sources
             ↓
    ┌──────────────────┐
    │   Data Lake      │
    │  HDFS / S3 / ADLS│
    └──────────────────┘
             ↓
          Spark
             ↓
    ┌──────────────────┐
    │ Transform        │
    │ Clean            │
    │ Join             │
    │ Aggregate        │
    └──────────────────┘
             ↓
      Processed Data
             ↓
    ┌──────────────────┐
    │ Data Warehouse   │
    └──────────────────┘
             ↓
       BI / Analytics
```

---

# Example with PySpark

For example, Spark can read a dataset from a Data Lake:

```python
df = spark.read.parquet("/data/raw/customers")
```

Then transform the data:

```python
clean_df = (
    df
    .filter(df["status"] == "ACTIVE")
    .select("customer_id", "name", "city")
)
```

Then write the processed data:

```python
clean_df.write.mode("overwrite").parquet(
    "/data/processed/customers"
)
```

The general idea is:

```text
Raw Data
   ↓
Spark
   ↓
Transform
   ↓
Processed Data
```

---

# Data Lake + Spark + Data Warehouse

A more complete architecture can look like:

```text
                 DATA SOURCES
                      │
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
      APIs          Files         Databases
        │             │             │
        └─────────────┼─────────────┘
                      ↓
                 DATA LAKE
              HDFS / S3 / ADLS
                      ↓
                    Spark
                      ↓
          ┌─────────────────────┐
          │ Transform           │
          │ Clean               │
          │ Join                │
          │ Aggregate           │
          └─────────────────────┘
                      ↓
              Processed Data
                      ↓
              DATA WAREHOUSE
                      ↓
              BI / Analytics
                      ↓
             Reports / Dashboards
```

---

# Important Concepts for Data Engineering

If you want to become strong in **Data Engineering**, it is useful to understand these concepts in sequence:

```text
SQL
 ↓
Data Warehouse
 ↓
Data Lake
 ↓
HDFS
 ↓
Hive
 ↓
Spark
 ↓
PySpark
 ↓
ETL / ELT
 ↓
Data Pipelines
 ↓
Data Modeling
 ↓
Data Lineage
 ↓
Data Quality
```

---

# ETL vs ELT

## ETL

**Extract → Transform → Load**

```text
Source
  ↓
Extract
  ↓
Transform
  ↓
Load
  ↓
Data Warehouse
```

## ELT

**Extract → Load → Transform**

```text
Source
  ↓
Extract
  ↓
Load
  ↓
Data Lake / Warehouse
  ↓
Transform
```

Modern cloud data platforms often use ELT because they can store large amounts of raw data and perform transformations later.

---

# Key Takeaways

### Data Lake

> Stores large amounts of raw and processed data.

### Data Warehouse

> Stores structured and prepared data for analytics and reporting.

### Spark

> Processes and transforms large-scale data.

### Hive

> Provides SQL and table/metadata management over big-data storage.

### HDFS

> Distributed storage for large datasets.

### Impala

> Fast distributed SQL querying, especially in Cloudera environments.

---

## The Big Picture

The most important idea is to understand **how the technologies work together**:

```text
              DATA
               ↓
        ┌─────────────┐
        │ Data Sources│
        └─────────────┘
               ↓
        ┌─────────────┐
        │  Data Lake  │
        │ HDFS / S3   │
        └─────────────┘
               ↓
        ┌─────────────┐
        │    Spark    │
        └─────────────┘
               ↓
       Transform / Clean
               ↓
        ┌─────────────┐
        │   Processed │
        │     Data    │
        └─────────────┘
               ↓
       ┌────────────────┐
       │ Data Warehouse │
       └────────────────┘
               ↓
        BI / Analytics
```

**Data Engineering is not about learning one tool. It is about understanding the entire data flow.**

# 📊 Data Engineering & Data Concepts

A practical reference guide to the most important concepts in **Data Engineering, Data Analytics, Data Architecture, Data Governance, BI, and Machine Learning**.

---

## 📚 Table of Contents

* [1. Data Management](#1-data-management)
* [2. Data Architecture](#2-data-architecture)
* [3. Data Governance & Quality](#3-data-governance--quality)
* [4. Data Security & Privacy](#4-data-security--privacy)
* [5. Data Analytics](#5-data-analytics)
* [6. Data Engineering](#6-data-engineering)
* [7. Advanced Data Concepts](#7-advanced-data-concepts)
* [8. Business Intelligence](#8-business-intelligence)
* [9. Machine Learning & Data Science](#9-machine-learning--data-science)
* [10. How the Concepts Fit Together](#10-how-the-concepts-fit-together)
* [11. Recommended Learning Path](#11-recommended-learning-path)

---

# 1. Data Management

Data management is the process of collecting, storing, organizing, maintaining, and using data effectively.

| Concept                 | Description                                                | Example                        |
| ----------------------- | ---------------------------------------------------------- | ------------------------------ |
| **Data Collection**     | Gathering data from different sources                      | APIs, databases, CSV files     |
| **Data Integration**    | Combining data from multiple systems                       | CRM + ERP + external API       |
| **Data Transformation** | Converting data into a useful format                       | Converting dates or currencies |
| **Data Cleaning**       | Fixing incorrect, missing, duplicate, or inconsistent data | Removing duplicate customers   |
| **Data Validation**     | Checking whether data follows defined rules                | Email must contain `@`         |
| **Data Storage**        | Storing data for operational or analytical use             | Database, Data Lake            |
| **Data Archiving**      | Moving old data to long-term storage                       | Historical transactions        |
| **Data Retention**      | Defining how long data should be stored                    | Keep records for 10 years      |
| **Data Migration**      | Moving data from one system to another                     | Oracle → PostgreSQL            |

---

# 2. Data Architecture

Data architecture defines how data is collected, processed, stored, and accessed across an organization.

## 🏗️ Data Modeling

**Data Modeling** is the design of data structures, entities, attributes, and relationships.

Example:

```text
Customer
   │
   ├── Customer ID
   ├── Name
   └── Email
        │
        ▼
     Orders
        │
        ├── Order ID
        ├── Customer ID
        └── Order Date
```

### Types of Data Models

* **Conceptual Data Model**
* **Logical Data Model**
* **Physical Data Model**

### Common Modeling Concepts

* Entity
* Attribute
* Relationship
* Primary Key
* Foreign Key
* Cardinality
* Normalization
* Denormalization
* Star Schema
* Snowflake Schema

---

## 🏢 Data Warehouse

A **Data Warehouse** is a centralized system optimized for analytical queries and reporting.

Examples:

* Snowflake
* Google BigQuery
* Amazon Redshift
* Azure Synapse
* Teradata

Typical flow:

```text
Operational Systems
        ↓
     ETL / ELT
        ↓
 Data Warehouse
        ↓
 BI / Analytics
```

---

## 🏞️ Data Lake

A **Data Lake** stores large amounts of raw or semi-structured data.

```text
Applications
     ↓
 Data Lake
     ↓
Raw / Cleaned / Curated Data
```

Common technologies:

* Hadoop
* HDFS
* Amazon S3
* Azure Data Lake Storage
* Google Cloud Storage

---

## 🏠 Data Lakehouse

A **Data Lakehouse** combines characteristics of Data Lakes and Data Warehouses.

```text
             Data Lakehouse
                  │
       ┌──────────┴──────────┐
       │                     │
   Data Lake             Warehouse
   Flexibility           Analytics
```

Examples:

* Databricks
* Delta Lake
* Apache Iceberg
* Apache Hudi

---

# 3. Data Governance & Quality

## 🏛️ Data Governance

**Data Governance** defines policies, standards, responsibilities, and processes for managing organizational data.

It answers questions such as:

* Who owns the data?
* Who can access it?
* What does a field mean?
* How should data be maintained?
* How long should data be stored?

---

## ✅ Data Quality

Data Quality measures whether data is reliable and useful.

Important dimensions:

* **Accuracy**
* **Completeness**
* **Consistency**
* **Validity**
* **Uniqueness**
* **Timeliness**

Example:

```text
Customer ID
-----------

12345      ✅
12346      ✅
12345      ❌ Duplicate
NULL       ❌ Missing
ABC        ❌ Invalid
```

---

## 📖 Data Dictionary

A **Data Dictionary** describes individual fields/columns.

Example:

| Column        | Type      | Description                |
| ------------- | --------- | -------------------------- |
| `customer_id` | INT       | Unique customer identifier |
| `name`        | STRING    | Customer name              |
| `email`       | STRING    | Customer email             |
| `created_at`  | TIMESTAMP | Creation timestamp         |

---

## 🗂️ Data Catalog

A **Data Catalog** is an inventory of datasets available within an organization.

It can contain:

* Tables
* Columns
* Definitions
* Owners
* Metadata
* Data lineage
* Data quality information

---

## 🧬 Data Lineage

**Data Lineage** tracks where data comes from, how it changes, and where it goes.

Example:

```text
Source Database
      ↓
   ETL Job
      ↓
 Data Cleaning
      ↓
 Transformation
      ↓
Data Warehouse
      ↓
 Power BI
      ↓
 Dashboard
```

A lineage question might be:

> Where does this column come from?

For example:

```text
Power BI Column
      ↓
Warehouse View
      ↓
SQL Transformation
      ↓
Source Table
      ↓
Source System
```

---

## 🧑‍💼 Data Ownership

Defines who is responsible for a dataset or data domain.

Example:

```text
Customer Data
     ↓
Data Owner: Customer Department
Data Steward: Data Management Team
```

---

## 🏷️ Data Classification

Categorizing data based on sensitivity or importance.

Example:

```text
Public
Internal
Confidential
Restricted
```

---

# 4. Data Security & Privacy

## 🔐 Data Security

Protecting data against:

* Unauthorized access
* Data loss
* Data modification
* Data destruction
* Cyber attacks

---

## 👤 Access Control

Determines who can access which data.

Common concepts:

* Authentication
* Authorization
* Role-Based Access Control (RBAC)
* Least Privilege

Example:

```text
Analyst
   ↓
Can SELECT data

Engineer
   ↓
Can SELECT + INSERT

Admin
   ↓
Full access
```

---

## 🔒 Encryption

Protecting data by converting it into an unreadable format without the proper key.

Two common types:

* Encryption at rest
* Encryption in transit

---

## 🎭 Data Masking

Hiding sensitive information while preserving its structure.

Example:

```text
Original:
mohammad@example.com

Masked:
m******@example.com
```

---

## 🕵️ Anonymization

Removing or transforming information so that individuals cannot easily be identified.

---

## 📝 Audit Trail

Records activities performed on data.

Example:

```text
User: analyst01
Action: SELECT
Table: customers
Time: 2026-10-06 10:30
```

---

# 5. Data Analytics

## 🔎 Data Exploration

Investigating data to discover:

* Patterns
* Trends
* Relationships
* Outliers
* Anomalies

Typical tools:

* SQL
* Python
* Pandas
* Excel
* Power BI

---

## 🧭 Data Navigation

Finding and moving through datasets, tables, reports, dashboards, or data catalogs to locate the information you need.

Example:

```text
Sales Dashboard
      ↓
Region
      ↓
Country
      ↓
Customer
      ↓
Order
```

---

## 📊 Descriptive Analytics

Answers:

> **What happened?**

Example:

```text
Revenue in 2026 = €5M
```

---

## 🔍 Diagnostic Analytics

Answers:

> **Why did it happen?**

Example:

```text
Revenue decreased
       ↓
Lower sales in Region A
       ↓
Fewer customers
```

---

## 🔮 Predictive Analytics

Answers:

> **What might happen?**

Example:

```text
Historical Sales
       ↓
Machine Learning Model
       ↓
Forecasted Sales
```

---

## 🎯 Prescriptive Analytics

Answers:

> **What should we do?**

Example:

```text
Sales decreasing
       ↓
Model recommends
       ↓
Increase promotion in Region A
```

---

# 6. Data Engineering

Data Engineering focuses on building systems that collect, process, transform, store, and deliver data.

## 🔄 ETL

**Extract → Transform → Load**

```text
Source
  ↓
Extract
  ↓
Transform
  ↓
Load
  ↓
Data Warehouse
```

Example:

```text
CSV
 ↓
Python
 ↓
Clean Data
 ↓
Transform
 ↓
PostgreSQL
```

---

## ⚡ ELT

**Extract → Load → Transform**

```text
Source
  ↓
Extract
  ↓
Load
  ↓
Data Warehouse
  ↓
Transform
```

ELT is common in modern cloud data platforms.

---

## 🔧 Data Pipeline

A **Data Pipeline** automatically moves and processes data between systems.

Example:

```text
API
 ↓
Kafka
 ↓
Spark
 ↓
Data Lake
 ↓
Data Warehouse
 ↓
Power BI
```

---

## 🌊 Batch Processing

Data is processed in groups at scheduled times.

Example:

```text
Every night at 02:00

Database
    ↓
Batch Job
    ↓
Transform
    ↓
Warehouse
```

---

## ⚡ Stream Processing

Data is processed continuously as it arrives.

Example:

```text
IoT Device
    ↓
Kafka
    ↓
Spark Streaming
    ↓
Real-Time Dashboard
```

---

## 🧮 Distributed Computing

Processing large datasets across multiple machines.

Important technologies:

* Apache Spark
* Hadoop
* Hive
* Impala
* Presto / Trino

Example:

```text
Large Dataset
      ↓
 ┌────┼────┐
 ↓    ↓    ↓
Node Node Node
 └────┼────┘
      ↓
   Result
```

---

## 🚀 Apache Spark

Apache Spark is a distributed computing framework used for large-scale data processing.

Common components:

* Spark SQL
* PySpark
* Spark Streaming
* MLlib

Example:

```python
df = spark.read.parquet("data.parquet")

df.filter(df.age > 18) \
  .groupBy("country") \
  .count()
```

---

## 🐝 Apache Hive

Hive provides SQL-like querying capabilities over large datasets in Hadoop ecosystems.

Example:

```sql
SELECT
    comune,
    COUNT(*) AS total
FROM buildings
GROUP BY comune;
```

---

# 7. Advanced Data Concepts

## 📈 Big Data

Big Data is commonly associated with datasets characterized by:

### The 5 Vs

* **Volume** — large amounts of data
* **Velocity** — data generated quickly
* **Variety** — different data formats
* **Veracity** — data reliability
* **Value** — useful business information

---

## 👀 Data Observability

Data Observability monitors the health and reliability of data systems.

It can detect:

* Missing data
* Schema changes
* Pipeline failures
* Unexpected values
* Data freshness problems
* Data volume anomalies

Example:

```text
Pipeline
   ↓
Monitoring
   ↓
 ┌───────────────┐
 │ Freshness     │
 │ Volume        │
 │ Schema        │
 │ Quality       │
 └───────────────┘
```

---

## 🔁 DataOps

**DataOps** applies automation, testing, monitoring, and operational practices to data workflows.

Similar idea:

```text
DevOps → Software

DataOps → Data
```

---

## 🕸️ Data Mesh

**Data Mesh** is a decentralized approach where business domains take ownership of their data products.

Example:

```text
             Data Platform
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
     Sales       Finance      HR
    Domain       Domain      Domain
       ↓           ↓           ↓
  Data Product Data Product Data Product
```

---

## 🏢 Master Data Management (MDM)

MDM manages important shared business entities.

Examples:

* Customers
* Products
* Suppliers
* Locations
* Employees

---

# 8. Business Intelligence

## 📊 Business Intelligence (BI)

BI uses data to support reporting, monitoring, and business decision-making.

Common BI tools:

* Power BI
* Tableau
* Qlik
* Looker

Typical architecture:

```text
Data Sources
     ↓
ETL / ELT
     ↓
Data Warehouse
     ↓
Semantic Model
     ↓
BI Dashboard
```

---

## 📐 KPI

**Key Performance Indicator**

A measurable value used to evaluate business performance.

Examples:

```text
Revenue
Profit Margin
Customer Churn
Orders
Conversion Rate
Average Delivery Time
```

---

## 📊 Dashboard

A visual interface that presents important metrics and KPIs.

Example:

```text
┌─────────────────────────────────────┐
│          SALES DASHBOARD            │
├──────────┬──────────┬───────────────┤
│ Revenue  │ Orders   │ Customers     │
│ €5.2M    │ 125,430  │ 45,230        │
├──────────┴──────────┴───────────────┤
│                                     │
│        Revenue Trend                │
│       📈📈📈📈📈                    │
│                                     │
└─────────────────────────────────────┘
```

---

# 9. Machine Learning & Data Science

## 🤖 Machine Learning

Machine Learning uses algorithms to learn patterns from data.

Basic flow:

```text
Data
 ↓
Cleaning
 ↓
Feature Engineering
 ↓
Training
 ↓
Model
 ↓
Prediction
```

---

## 🧪 Feature Engineering

Creating or transforming variables to improve a Machine Learning model.

Example:

```text
date_of_birth
      ↓
     age
```

or:

```text
timestamp
    ↓
 ┌──────────────┐
 │ hour         │
 │ day_of_week  │
 │ month        │
 └──────────────┘
```

---

## 🧠 Data Science

Data Science combines:

* Statistics
* Programming
* Machine Learning
* Data Analysis
* Domain Knowledge

Typical workflow:

```text
Business Problem
      ↓
Data Collection
      ↓
Data Cleaning
      ↓
Exploration
      ↓
Feature Engineering
      ↓
Modeling
      ↓
Evaluation
      ↓
Deployment
```

---

# 10. How the Concepts Fit Together

A typical modern data platform can look like this:

```text
                  DATA SOURCES
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
       Database       API          Files
          │            │            │
          └────────────┼────────────┘
                       ↓
                DATA INGESTION
                       │
                       ↓
                DATA PIPELINES
                       │
                       ↓
              DATA CLEANING
                       │
                       ↓
             DATA TRANSFORMATION
                       │
                       ↓
              DATA INTEGRATION
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
         DATA LAKE          DATA WAREHOUSE
             │                   │
             └─────────┬─────────┘
                       ↓
                 DATA MODELING
                       │
                       ↓
                  DATA QUALITY
                       │
                       ↓
                 DATA GOVERNANCE
                       │
                       ↓
                  DATA LINEAGE
                       │
                       ↓
                BI / ANALYTICS
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
        DASHBOARDS          ML / AI MODELS
```

---

# 11. Recommended Learning Path

If you are learning **Data Engineering**, a good order is:

### 🟢 Level 1 — Fundamentals

1. Data
2. Databases
3. Tables
4. Rows & Columns
5. Primary Keys
6. Foreign Keys
7. SQL
8. Data Types
9. NULL values
10. CRUD

---

### 🟡 Level 2 — Data Management

11. Data Collection
12. Data Cleaning
13. Data Validation
14. Data Transformation
15. Data Integration
16. Data Migration
17. Data Storage

---

### 🟠 Level 3 — Data Modeling

18. Data Modeling
19. ER Models
20. Normalization
21. Denormalization
22. Star Schema
23. Snowflake Schema
24. Fact Tables
25. Dimension Tables

---

### 🔵 Level 4 — Data Engineering

26. ETL
27. ELT
28. Data Pipelines
29. Batch Processing
30. Stream Processing
31. Apache Spark
32. PySpark
33. Hive
34. Impala
35. Kafka

---

### 🟣 Level 5 — Data Platforms

36. Data Warehouse
37. Data Lake
38. Data Lakehouse
39. Delta Lake
40. Apache Iceberg
41. Cloud Storage
42. Distributed Computing

---

### 🔴 Level 6 — Governance & Quality

43. Data Quality
44. Data Governance
45. Data Lineage
46. Metadata
47. Data Catalog
48. Data Dictionary
49. Data Ownership
50. Data Classification

---

### ⚫ Level 7 — Advanced Concepts

51. Data Observability
52. DataOps
53. Data Mesh
54. Master Data Management
55. Real-Time Analytics
56. Big Data
57. Feature Engineering
58. Machine Learning

---

# 🎯 Core Concepts to Remember

If you are preparing for a **Data Engineer** role, focus especially on these:

```text
SQL
│
├── Joins
├── CTEs
├── Window Functions
├── Aggregations
└── Views

DATA ENGINEERING
│
├── ETL / ELT
├── Data Pipelines
├── Data Transformation
├── Batch Processing
└── Streaming

DATA ARCHITECTURE
│
├── Data Warehouse
├── Data Lake
├── Data Lakehouse
└── Distributed Systems

DATA MODELING
│
├── Fact Tables
├── Dimension Tables
├── Star Schema
└── Snowflake Schema

DATA GOVERNANCE
│
├── Data Quality
├── Data Lineage
├── Metadata
├── Data Catalog
└── Data Ownership

BIG DATA
│
├── Hadoop
├── Hive
├── Impala
├── Spark
└── Kafka

ANALYTICS
│
├── BI
├── Dashboards
├── KPIs
└── Data Visualization
```

---

## 🧠 The Big Picture

The main idea behind Data Engineering can be summarized as:

> **Get the right data → clean it → transform it → store it → model it → validate it → understand its lineage → make it accessible → use it for analytics and decisions.**

```text
SOURCE
  ↓
INGEST
  ↓
CLEAN
  ↓
TRANSFORM
  ↓
INTEGRATE
  ↓
STORE
  ↓
MODEL
  ↓
VALIDATE
  ↓
GOVERN
  ↓
ANALYZE
  ↓
VISUALIZE
  ↓
DECIDE
```

---

## 📌 Quick Reference

| Area                 | Key Concepts                                        |
| -------------------- | --------------------------------------------------- |
| **SQL**              | SELECT, JOIN, GROUP BY, CTE, Window Functions       |
| **Data Management**  | Cleaning, Validation, Integration, Migration        |
| **Data Engineering** | ETL, ELT, Pipelines, Spark, Kafka                   |
| **Architecture**     | Lake, Warehouse, Lakehouse                          |
| **Modeling**         | Fact, Dimension, Star Schema, Snowflake Schema      |
| **Governance**       | Quality, Lineage, Metadata, Catalog                 |
| **Security**         | Access Control, Encryption, Masking                 |
| **Analytics**        | Descriptive, Diagnostic, Predictive, Prescriptive   |
| **BI**               | Power BI, Tableau, KPIs, Dashboards                 |
| **Big Data**         | Hadoop, Hive, Impala, Spark                         |
| **Advanced**         | Data Mesh, DataOps, Observability, MDM              |
| **AI/ML**            | Feature Engineering, Machine Learning, Data Science |

---

⭐ **Goal:** Build a strong foundation in data concepts before moving into advanced Data Engineering, Cloud, Big Data, and Machine Learning.

# Hive / Cloudera SQL — Database, Schema & Table Commands

A quick reference for working with **Hive / Cloudera SQL**, especially when investigating databases, schemas, tables, views, and data lineage.

---

## 1. Show All Databases / Schemas

Show all available databases:

```sql
SHOW DATABASES;
```

Or:

```sql
SHOW SCHEMAS;
```

---

## 2. Show Tables Inside a Schema

To list all tables and views inside a specific schema/database:

```sql
SHOW TABLES IN test_stradario;
```

Example:

```text
test_stradario
├── table_1
├── table_2
├── vw_check_vendibilita_egon
└── ...
```

---

## 3. Describe a Table or View

### Basic information

To see the columns and their data types:

```sql
DESCRIBE test_stradario.vw_check_vendibilita_egon;
```

This usually shows:

* Column name
* Data type
* Comment

---

### Detailed information

Use `DESCRIBE EXTENDED`:

```sql
DESCRIBE EXTENDED test_stradario.vw_check_vendibilita_egon;
```

This can provide additional metadata about the object.

---

### Formatted information

If supported by the Cloudera/Hive environment:

```sql
DESCRIBE FORMATTED test_stradario.vw_check_vendibilita_egon;
```

`FORMATTED` is particularly useful for inspecting metadata such as:

* Table type
* Database/schema
* Location
* Storage format
* Partitions
* Table properties

---

## 4. Show the SQL Used to Create a Table

To see the DDL used to create a table:

```sql
SHOW CREATE TABLE test_stradario.vw_check_vendibilita_egon;
```

This is useful for understanding the table structure and, depending on the object type, how it was created.

---

## 5. Show the SQL Used to Create a View

If the object is a **view**, use:

```sql
SHOW CREATE VIEW test_stradario.vw_check_vendibilita_egon;
```

This is especially useful for **data lineage investigations**.

For example, it can help identify:

```text
Source Table
     ↓
Transformation / JOIN
     ↓
View
     ↓
Final EGON / Coordinate values
```

---

## 6. Create a Schema / Database

In Hive, a schema is essentially a database.

Create a database:

```sql
CREATE DATABASE my_schema;
```

Or:

```sql
CREATE SCHEMA my_schema;
```

To avoid an error if the database already exists:

```sql
CREATE DATABASE IF NOT EXISTS my_schema;
```

---

## 7. Create a Table

Example:

```sql
CREATE TABLE my_schema.test_table (
    egon STRING,
    latitude DOUBLE,
    longitude DOUBLE
);
```

The table contains:

| Column      | Type     |
| ----------- | -------- |
| `egon`      | `STRING` |
| `latitude`  | `DOUBLE` |
| `longitude` | `DOUBLE` |

---

## 8. Query Data

To inspect the actual data:

```sql
SELECT *
FROM test_stradario.vw_check_vendibilita_egon
LIMIT 10;
```

For example, you can inspect:

```sql
SELECT
    egon,
    latitude,
    longitude
FROM test_stradario.vw_check_vendibilita_egon
LIMIT 10;
```

---

# Recommended Investigation Workflow

When investigating an existing Stradario table or view, use the following sequence.

### Step 1 — List databases

```sql
SHOW DATABASES;
```

### Step 2 — Check the Stradario schema

```sql
SHOW TABLES IN test_stradario;
```

### Step 3 — Inspect the object structure

```sql
DESCRIBE FORMATTED test_stradario.vw_check_vendibilita_egon;
```

### Step 4 — Check how the object was created

If it is a table:

```sql
SHOW CREATE TABLE test_stradario.vw_check_vendibilita_egon;
```

If it is a view:

```sql
SHOW CREATE VIEW test_stradario.vw_check_vendibilita_egon;
```

### Step 5 — Inspect the data

```sql
SELECT *
FROM test_stradario.vw_check_vendibilita_egon
LIMIT 10;
```

---

# Data Lineage Investigation

For the current **Stradario / EGON / Coordinate** investigation, the most important command is:

```sql
SHOW CREATE VIEW test_stradario.vw_check_vendibilita_egon;
```

The goal is to identify the source of fields such as:

```text
EGON
Building
Latitude
Longitude
Comune
Civico
```

A typical investigation looks like:

```text
                 Source Tables
                      │
          ┌───────────┴───────────┐
          │                       │
       EGON Source          Coordinate Sources
          │                       │
          └───────────┬───────────┘
                      ↓
              Stradario Logic
                      ↓
             EGON + Coordinates
                      ↓
       vw_check_vendibilita_egon
```

This allows you to understand **which source tables provide each value** and how Stradario selects the final value.

---

# Quick Reference

| Goal                 | Command                                |
| -------------------- | -------------------------------------- |
| List databases       | `SHOW DATABASES;`                      |
| List schemas         | `SHOW SCHEMAS;`                        |
| List tables          | `SHOW TABLES IN schema_name;`          |
| Describe object      | `DESCRIBE schema.table;`               |
| Detailed metadata    | `DESCRIBE EXTENDED schema.table;`      |
| Formatted metadata   | `DESCRIBE FORMATTED schema.table;`     |
| Show table DDL       | `SHOW CREATE TABLE schema.table;`      |
| Show view definition | `SHOW CREATE VIEW schema.view;`        |
| Read sample data     | `SELECT * FROM schema.table LIMIT 10;` |
| Create database      | `CREATE DATABASE schema_name;`         |
| Create table         | `CREATE TABLE schema.table (...);`     |

---

## Example: Stradario Investigation

```sql
-- 1. Check available databases
SHOW DATABASES;

-- 2. List Stradario objects
SHOW TABLES IN test_stradario;

-- 3. Inspect metadata
DESCRIBE FORMATTED test_stradario.vw_check_vendibilita_egon;

-- 4. Check the definition
SHOW CREATE VIEW test_stradario.vw_check_vendibilita_egon;

-- 5. Inspect sample records
SELECT *
FROM test_stradario.vw_check_vendibilita_egon
LIMIT 10;
```

> **Tip:** For understanding the **EGON and coordinate data flow in Stradario**, start with `SHOW CREATE VIEW` (when the object is a view). The definition can reveal the source tables, joins, filters, and transformations used to produce the final values.




