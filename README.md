#  Spotify Azure Data Engineering Project

An end-to-end data engineering project built using **Azure Data Factory, Azure SQL Database, Azure Data Lake Storage, Azure Databricks, PySpark, and Delta Lake**. This project demonstrates how to build automated data pipelines for Spotify streaming data using the Medallion Architecture, incremental ingestion, data transformation, and dimensional data modelling.

## 📌 Project Overview

The goal of this project was to design and implement a complete data engineering pipeline that ingests Spotify data, processes it through Bronze, Silver, and Gold layers, and prepares analytics-ready datasets.

The project brings together the data engineering concepts I learned during my **#100DaysOfDataEngineering** journey and applies them in an end-to-end cloud project.

###  Project Objectives

- Build an end-to-end data ingestion pipeline using Azure Data Factory.
- Use Azure SQL Database as a source for Spotify data.
- Implement initial and incremental data loading.
- Develop reusable, parameterized pipelines with looping and backfilling capabilities.
- Store raw and processed data in Azure Data Lake Storage.
- Transform data using Azure Databricks and PySpark.
- Implement Delta Lake processing and data quality checks.
- Design a dimensional model with fact and dimension tables.
- Configure automated email notifications for pipeline failures.
- Explore advanced data engineering concepts, including Structured Streaming, Auto Loader, Unity Catalog, Delta Live Tables, and Databricks Asset Bundles.

##  Architecture

The project follows the **Medallion Architecture**, organizing data into three layers.

```text
             Spotify Source Data
                      |
                      v
             Azure SQL Database
                      |
                      v
             Azure Data Factory
       Initial & Incremental Ingestion
       Parameterization, Looping,
          Backfilling and CDC
                      |
                      v
          Azure Data Lake Storage
                      |
                      v
              BRONZE LAYER
              Raw Data Storage
                      |
                      v
             AZURE DATABRICKS
                 PySpark
                      |
                      v
              SILVER LAYER
       Cleaning, Deduplication,
       Data Types and Transformation
                      |
                      v
               GOLD LAYER
        Dimensional Data Modelling
          Fact and Dimension Tables
                      |
                      v
            Analytics-Ready Data

       Azure Logic Apps + Gmail
       Pipeline Failure Notifications
```

*Note: The diagram represents the project's logical architecture. The exact implementation of individual advanced features may vary.*

## 🥉 Bronze Layer — Data Ingestion

The Bronze layer is responsible for ingesting and storing raw data from the source system.

### Key Features

- Azure Data Factory pipelines for data ingestion.
- Azure SQL Database as the source.
- Azure Data Lake Storage as the data lake destination.
- Initial loading to ingest the source data.
- Incremental loading to process new or changed records.
- Parameterized pipelines to support reusable ingestion workflows.
- Looping to process multiple datasets.
- Backfilling to support historical data processing.
- Change Data Capture (CDC) concepts for tracking data changes.

The project includes two primary ingestion pipelines:

- `incremental_ingestion`
- `incremental_loop`

These pipelines demonstrate how ingestion workflows can be made more dynamic and reusable instead of building separate pipelines for every dataset.

## 🥈 Silver Layer — Data Transformation

The Silver layer processes the raw data using **Azure Databricks and PySpark**.

### Key Transformations

- Data cleaning and standardization.
- Data type handling.
- Removal of unnecessary columns.
- Duplicate record removal.
- Data transformation and validation.
- Incremental processing using Delta Lake.
- Preparation of structured data for analytical modelling.

Delta Lake supports reliable data storage and processing, while PySpark provides scalable transformation capabilities.

## 🥇 Gold Layer — Analytics and Dimensional Modelling

The Gold layer contains business-ready datasets designed for reporting and analytical use cases.

The project uses dimensional data modelling to organize Spotify streaming data into fact and dimension tables.

### Dimensional Model

The planned analytical model consists of four dimension tables and one fact table.

| Table | Purpose |
|---|---|
| `DimUser` | Stores user-related attributes. |
| `DimArtist` | Stores artist-related attributes. |
| `DimTrack` | Stores track-related attributes. |
| `DimDate` | Stores date-related attributes for time-based analysis. |
| `FactStream` | Stores streaming activity and measurable streaming events. |

The fact table connects to the dimension tables through keys, allowing analytical queries across users, artists, tracks, and dates.

### Concepts Applied

- Star schema design.
- Fact and dimension tables.
- Surrogate and foreign keys where applicable.
- Slowly Changing Dimensions (SCD).
- Incremental data processing.
- Data quality validation.

## 🔄 Initial and Incremental Loading

An important part of this project is understanding how data pipelines handle both historical and newly available data.

### Initial Load

The initial load ingests the existing source data into the data lake.

The project includes an initial-load SQL script containing approximately **2,865 INSERT statements**.

### Incremental Load

The incremental load processes new or changed data rather than repeatedly reloading the entire source dataset.

The project includes an incremental-load SQL script containing approximately **470 INSERT statements**.

The pipeline uses lookup and SQL activities to retrieve change-tracking information and conditionally process incremental data.

### Backfilling

Backfilling allows historical data to be processed for a selected period or range when required.

### Looping

The looping pipeline uses reusable activities to process multiple datasets or inputs dynamically.

These approaches help improve pipeline maintainability and reduce unnecessary data processing.

## 📧 Pipeline Monitoring and Email Notifications

Azure Logic Apps is integrated with the pipeline workflow to support email notifications.

The Logic App uses an HTTP request trigger and an email action. The workflow can receive pipeline details, including:

- Pipeline name.
- Pipeline run ID.

This provides a way to notify the relevant recipient when a pipeline failure occurs and makes troubleshooting easier.

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Azure Data Factory | Data ingestion and pipeline orchestration. |
| Azure SQL Database | Source database for Spotify data. |
| Azure Data Lake Storage | Storage for raw and processed data. |
| Azure Databricks | Data processing and transformation. |
| PySpark | Distributed data transformation. |
| Python | Utility functions and workflow logic. |
| SQL | Source data loading and database operations. |
| Delta Lake | Reliable data storage and incremental processing. |
| Spark Structured Streaming | Streaming data processing concepts. |
| Auto Loader | Incremental file ingestion in Databricks. |
| Unity Catalog | Data governance and catalog management concepts. |
| Delta Live Tables | Managed data pipeline concepts. |
| Jinja2 | Template-driven, metadata-based processing. |
| Databricks Asset Bundles | Deployment and project configuration concepts. |
| Azure Logic Apps | Workflow automation and email notifications. |
| GitHub | Source control and project documentation. |

## 📂 Repository Structure

The repository contains the following Azure Data Factory configuration files, SQL scripts, and supporting resources.

```text
SpotifyAzureProject/
│
├── dataset/
│   ├── Json_dynamic.json
│   ├── Parquet_dynamic.json
│   └── azure_sql.json
│
├── factory/
│   └── df01azureproject.json
│
├── images/
│   ├── azure-resources-dashboard.jpg
│   ├── adf-incremental-ingestion-overview.jpg
│   ├── adf-incremental-loop-pipeline.jpg
│   ├── adf-incremental-loop-activities.jpg
│   ├── adf-pipeline-run-success.jpg
│   ├── logic-app-http-email-workflow.jpg
│   └── databricks-silver-gold-tables.jpg
│
├── linkedService/
│   ├── azure_sql.json
│   └── datalake.json
│
├── pipeline/
│   ├── incremental_ingestion.json
│   └── incremental_loop.json
│
├── source_scripts/
│   ├── spotify_incremental_load (1).sql
│   └── spotify_initial_load (2).sql
│
├── README.md
├── cdc.json
├── empty.json
├── loop_input
└── publish_config.json
```

*The image filenames above are the recommended names for the screenshots. Ensure the files in your `images/` folder use these exact names, or update the corresponding image links below.*

## 📸 Project Screenshots

The following screenshots document the Azure resources, ingestion workflows, pipeline execution, notification setup, and Databricks work.

## 1. Azure Resources

The Azure environment contains the resources used to build the project, including Azure Data Factory, Azure SQL Database, Azure Databricks, and storage resources.

![Azure resources dashboard](images/dashboard%20all%20resources%20files.jpg)

## 2. Incremental Ingestion Pipeline

The ingestion workflow uses lookup, copy, and conditional activities to process data incrementally.

![Azure Data Factory incremental ingestion overview](images/bronze%201.jpg)

## 3. Incremental Loop Pipeline | Pipeline Activities and CDC Logic

The looping workflow supports reusable processing across multiple inputs. The pipeline uses lookup and variable activities, data copying, conditional processing, and change-tracking logic to control incremental ingestion. The Azure Data Factory monitoring view shows pipeline execution details and activity statuses. Successful runs and failure notifications help monitor and troubleshoot the workflow.

![Azure Data Factory incremental loop pipeline](images/day%202.2.jpg)

![Azure Data Factory incremental loop activities](images/DAY%202.1.jpg)

## 4. Logic Apps Email Notifications

The Logic App workflow uses an HTTP request trigger and an email action to support pipeline notifications.

![Azure Logic Apps HTTP and email workflow](images/DAY%202.jpg)

![Azure Logic Apps HTTP and email workflow](images/day%202.3.jpg)

## 5. Databricks Silver and Gold Layers

The Databricks workspace supports data transformation and dimensional modelling, including the date dimension and streaming fact table.

![Azure Databricks Silver and Gold tables](images/day%2062.3.jpg)

![Azure Databricks Silver and Gold tables](images/day%2062.4.jpg)

![Azure Databricks Silver and Gold tables](images/day%2062.5.jpg)

![Azure Databricks Silver and Gold tables](images/DAY%2063.1%20testing.jpg)

![Azure Databricks Silver and Gold tables](images/DAY%2063.2%20testing%20pt2.jpg)

## 🚀 How to Explore the Project

### Prerequisites

To explore or recreate this project, you will need:

- An Azure subscription.
- Azure Data Factory.
- Azure SQL Database.
- Azure Data Lake Storage.
- An Azure Databricks workspace.
- Appropriate permissions to access and configure the services.
- Git and GitHub for version control.

Some services may incur costs depending on usage and configuration.

### Step 1: Clone the Repository

```bash
git clone https://github.com/malikareeba/SpotifyAzureProject.git
cd SpotifyAzureProject
```

### Step 2: Review the Data Factory Configuration

Explore the following folders:

- `factory/`
- `dataset/`
- `linkedService/`
- `pipeline/`

Review the JSON definitions to understand how datasets, connections, and pipeline activities are configured.

### Step 3: Configure Azure Connections

Create or configure the required Azure SQL Database, Azure Data Lake Storage, and Azure Data Factory resources.

Update connection settings and credentials for your own environment. Do not commit passwords, access keys, connection strings containing secrets, or other sensitive information.

### Step 4: Review the SQL Scripts

Open the scripts in `source_scripts/` to understand the initial and incremental source-data loading logic.

Make sure the required source tables and database permissions are available before running the pipelines.

### Step 5: Run the Data Factory Pipelines

Import or recreate the pipeline definitions in your Azure Data Factory environment, configure the required linked services and datasets, and test the workflows.

Start with a controlled initial load, then validate incremental processing and looping.

### Step 6: Explore Databricks Transformations

Use the associated Databricks notebooks or code artifacts from your project to review the Silver transformations, Delta Lake processing, and Gold dimensional model.

**Note:** The repository tree above does not include Databricks notebook files, so these may need to be accessed from your Databricks workspace or added to the repository separately.

## 📚 Key Learnings

Through this project, I developed practical experience with:

- Designing an end-to-end data engineering pipeline on Azure.
- Building initial and incremental ingestion workflows.
- Using Azure Data Factory parameters, variables, loops, and conditional activities.
- Understanding backfilling and change-tracking concepts.
- Integrating Azure SQL Database with Azure Data Lake Storage.
- Transforming data with PySpark and Delta Lake.
- Applying the Bronze, Silver, and Gold Medallion Architecture.
- Designing fact and dimension tables using a star schema.
- Understanding Slowly Changing Dimensions.
- Automating pipeline notifications with Azure Logic Apps.
- Organizing project configuration and code using GitHub.

## 🔮 Future Improvements

Potential enhancements include:

- Implement automated testing for data quality and schema validation.
- Add more detailed logging and centralized monitoring.
- Introduce CI/CD deployment workflows.
- Add Databricks notebooks and transformation code to the repository.
- Improve error handling and retry strategies.
- Add documentation for data lineage and table relationships.
- Develop analytical dashboards using the Gold-layer datasets.

## 👩‍💻 About This Project

This project was completed as part of my **#100DaysOfDataEngineering** learning journey.

It represents an important step in applying data engineering concepts to a practical cloud-based project, from source ingestion and incremental processing to transformation and analytical data modelling.

I am continuing to build my skills in Azure, PySpark, SQL, and modern data engineering practices through hands-on projects.

## 🔗 Project Links

- **GitHub Repository:** [Spotify Azure Data Engineering Project](https://github.com/malikareeba/SpotifyAzureProject)
- **LinkedIn:** [Areeba Malik](https://www.linkedin.com/in/areeba-malik234/)

---

⭐ If you find this project useful, feel free to explore the repository.

**#DataEngineering #Azure #AzureDataFactory #AzureDatabricks #PySpark #SQL #DeltaLake #DataPipeline #MedallionArchitecture #100DaysOfDataEngineering**
