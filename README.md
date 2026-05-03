# ADF E-commerce Analytics Pipeline

## 📌 Overview

This project demonstrates an end-to-end data ingestion pipeline built using Azure Data Factory. The pipeline extracts e-commerce datasets from a GitHub-hosted source and loads them into Azure Data Lake Storage Gen2 for further analytics.

The datasets included are:

* Customers
* Sales
* Sales Orders

---

## 🏗️ Architecture

The solution uses the following components:

* **Azure Data Factory** for orchestration and data movement
* **HTTP source** (GitHub raw files) for input data
* **Azure Data Lake Storage Gen2 (ADLS Gen2)** as the destination

Each dataset is ingested using a dedicated Copy Activity pipeline.

---

## 🔄 Pipeline Workflow

1. Data is fetched from GitHub using HTTP connector
2. Data is read as delimited text (CSV-like format)
3. Column mappings are applied using TabularTranslator
4. Data is written into ADLS Gen2 in structured folders

---

## 📂 Data Organization in ADLS

Data is stored in the following structure:

customers/
    customers.txt

sales/
    sales.txt

sales_orders/
    sales_orders.txt

---

## ⚙️ Configuration Details

### Source

* Type: Delimited Text
* Connector: HTTP
* Format: CSV

### Sink

* Type: Delimited Text
* Storage: ADLS Gen2
* File format: `.txt`
* Copy behavior: PreserveHierarchy

---

## ⚠️ Known Issue & Fix

### Issue

Duplicate files and folders were created in ADLS, such as:

* `customers` (0 KB file)
* `customers/` (folder with data)

### Root Cause

The sink dataset did not define a **fileName**, and the pipeline used `FlattenHierarchy`. This caused Azure Data Factory to create both:

* a placeholder blob (file)
* and a directory for actual output

### Fix

* Added explicit `fileName` in dataset:

  * `customers.txt`, `sales.txt`, `sales_orders.txt`
* Changed copy behavior to `PreserveHierarchy`
* Removed 0 KB duplicate blobs from storage

---

## 🚀 How to Run

1. Deploy linked services, datasets, and pipelines in Azure Data Factory
2. Update the ADLS Gen2 connection (linked service)
3. Trigger pipelines manually or via schedule
4. Verify output in ADLS Gen2 container

---

## 📌 Future Enhancements

* Parameterize pipelines for dynamic dataset handling
* Add data validation and error handling
* Integrate with Azure Synapse for analytics
* Implement incremental data loading

---

## 👤 Author

Pooja
