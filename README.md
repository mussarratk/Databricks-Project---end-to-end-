# Databricks-Project---end-to-end

### **Project Title:** End-to-End Data Engineering Pipeline on the Databricks Lakehouse Platform

-----

<details>
  

  
### **CV/Resume Points**

  * Designed and implemented a scalable, multi-layered data architecture (Bronze, Silver, Gold) on Databricks to process real-time streaming data from diverse sources, including CSV, JSON, and a Kafka multiplex stream.
  * Engineered a robust ingestion framework using **Structured Streaming** and **Auto Loader (`cloudFiles`)** to handle incremental data loads, ensuring data idempotency and fault tolerance with checkpointing.
  * Developed sophisticated Change Data Capture (CDC) logic in the Silver layer to manage Slowly Changing Dimensions (SCD) Type 1 for user profiles, employing window functions to handle late and out-of-order data effectively.
  * Optimized data pipelines for performance by implementing **`foreachBatch`** for efficient upsert operations, utilizing **`withWatermark`** and **`dropDuplicates`** for stateful stream processing, and leveraging static-stream joins.
  * Automated the end-to-end deployment of the data pipeline as a Databricks multi-task job using the **Databricks REST API**, ensuring consistent and repeatable deployments across environments.
    
</details>

-----


## **End-to-End Data Engineering Project on Databricks**

### **Project Overview**

This project demonstrates the design and implementation of a modern, medallion-style data architecture on the Databricks Lakehouse Platform. The pipeline ingests and processes streaming data from multiple sources, transforming it through a series of stages to create a high-quality, reliable, and analytics-ready dataset. The core objective is to build a robust, scalable, and maintainable data pipeline that can handle real-time data ingestion and complex data transformations, including Change Data Capture (CDC).

The project is structured into three main layers: Bronze, Silver, and Gold, representing raw, validated, and aggregated data, respectively.

### **Contents & Folder Structure**

The project is organized to promote modularity and reusability, with each Databricks notebook serving a specific purpose in the data pipeline.

```
/
├── Deploy.py              (Python script for automated job deployment)
├── 01-config.py           (Project-wide configuration variables)
├── 02-setup.py            (Notebook for creating the database and tables)
├── 03-history-loader.py   (Notebook for loading historical data and performing initial validations)
├── 04-bronze.py           (Notebook for the Bronze layer ingestion logic)
├── 05-silver.py           (Notebook for the Silver layer transformation logic)
├── 06-gold.py             (Notebook for the Gold layer aggregation logic)
├── 07-run.py              (Main entry point notebook to orchestrate the pipeline run)
├── 08-batch-test.py       (Notebook for testing the pipeline in batch mode)
└── 09-stream-test.py      (Notebook for testing the pipeline in streaming mode, including job creation)
```

### **Data Sources**

The project processes data from several simulated streaming sources, including:

  * **`registered_users` (CSV):** New user registrations.
  * **`gym_logins` (CSV):** User check-ins and check-outs at gym locations.
  * **`kafka_multiplex` (JSON):** A multiplexed Kafka stream containing three distinct topics:
      * **`user_info`:** Change Data Capture (CDC) events for user profile updates.
      * **`workout`:** Start and stop events for user workouts.
      * **`bpm`:** Heart rate data from wearable devices.

### **Pipeline Architecture and Process Flow**

The data flows through a medallion architecture, ensuring data quality and usability at each stage.

#### **1. Bronze Layer (Raw Data Ingestion)**

The Bronze layer is the raw ingestion stage. It reads data directly from the landing zone and stores it in Delta tables without any major transformations. This layer acts as a single source of truth for all raw data.

  * **Technology:** Structured Streaming with Auto Loader (`cloudFiles`) is used to incrementally and efficiently ingest new files as they arrive.
  * **Logic:** The primary logic here is to append all incoming data to the respective Delta tables, preserving the raw state. The `outputMode("append")` is used for this purpose.

#### **2. Silver Layer (Validated & Enriched Data)**

The Silver layer cleanses, refines, and enriches the raw Bronze data. This stage resolves duplicates, applies schemas, and performs necessary joins to create a more reliable and complete dataset for downstream analysis.

  * **Technology:** Structured Streaming with `foreachBatch` is used to apply more complex, batch-oriented logic like `MERGE INTO` operations.
  * **Logic:**
      * **`upsert_user_profile`:** Handles Change Data Capture (CDC) for user profile updates. It uses a custom **`CDCUpserter`** class to apply window functions and rank records by a timestamp, ensuring only the latest update for each user is processed.

#### **3. Gold Layer (Aggregated & Business-Ready Data)**

The Gold layer contains the final, aggregated data, optimized for business intelligence, reporting, and machine learning. This data is highly curated and ready for consumption.

  * **Technology:** Structured Streaming with `foreachBatch` is used to create final aggregate tables.
  * **Logic:**
      * **`upsert_workout_bpm_summary`:** Groups heart rate data by `user_id`, `workout_id`, and `session_id` to calculate summary statistics like `min_bpm`, `avg_bpm`, and `max_bpm`. This data is then joined with user demographics to create a business-ready table for analytics.

-----

### **Deployment**

The project deployment is automated using the Databricks REST API. The `Deploy.py` script serves as a single source of truth for the job definition, making the deployment process consistent and repeatable.

#### **Process**

1.  The `Deploy.py` script is executed with the Databricks workspace URL and a personal access token as arguments.
2.  It constructs a **JSON payload** that defines a Databricks multi-task job named `"ingest-process-pipeline"`.
3.  This job is configured with a series of dependent tasks:
      * **`ingest-data`:** This task, which points to a notebook (`/exercise/data_ingestion`), is responsible for running the initial data ingestion.
      * **`process-data-2014` and `process-data-2015`:** These tasks are configured to run after `ingest-data` is complete. They reference a data analysis notebook and pass specific parameters (`year: 2014` and `year: 2015`) to process data for different years.
4.  The script then sends a **POST request** to the `api/2.1/jobs/create` endpoint of the Databricks REST API, passing the job definition.
5.  Upon a successful request, a new multi-task job is created in the Databricks workspace, ready to be triggered manually or via a schedule. This process streamlines the CI/CD pipeline and ensures that the production environment is always in sync with the code repository.
