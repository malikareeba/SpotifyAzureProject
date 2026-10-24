# 🎵 Spotify Azure Data Engineering Project

### End-to-End Data Engineering Pipeline | Azure Data Factory | Azure Databricks | PySpark | Delta Lake

![Azure](https://img.shields.io/badge/Microsoft_Azure-0089D6?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Azure Data Factory](https://img.shields.io/badge/Azure_Data_Factory-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Databricks](https://img.shields.io/badge/Azure_Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-FDEE21?style=for-the-badge&logo=apachespark&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

## 📌 Project Overview

The **Spotify Azure Data Engineering Project** is an end-to-end data engineering solution built to demonstrate how Spotify streaming data can be ingested, processed, transformed, and prepared for analytics using Microsoft Azure services.

The project follows the **Medallion Architecture**—Bronze, Silver, and Gold layers. Azure Data Factory orchestrates data ingestion, while Azure Databricks and PySpark are used for data cleaning, transformation, and incremental processing. The Gold layer uses dimensional modelling, with four dimension tables and one fact table for users, artists, tracks, and streaming activity.

The project brings together practical data engineering concepts including incremental data loading, metadata-driven workflows, backfilling, parameterisation, looping, Delta Lake, Slowly Changing Dimensions (SCDs), and data quality checks.

## 🎯 Project Objectives

- Build an end-to-end data pipeline using Azure data engineering services.
- Ingest Spotify data from five source tables.
- Implement initial and incremental ingestion workflows using Azure Data Factory.
- Develop reusable and parameterised pipelines.
- Implement looping and backfilling functionality.
- Apply the Bronze, Silver, and Gold Medallion Architecture.
- Clean, deduplicate, and transform data using PySpark.
- Use Delta Lake for storage and incremental processing.
- Design a Gold-layer star schema with fact and dimension tables.
- Apply data quality checks and SCD concepts.
- Explore Databricks capabilities for streaming, governance, and deployment.
- Manage project code and configurations using GitHub.

## 🏗️ Architecture

```text
                 Spotify Source Data
                          |
                          v
                 Azure SQL Database
                          |
                          v
                  Azure Data Factory
                          |
             +------------+------------+
             |                         |
             v                         v
        Initial Load             Incremental Load
             |                         |
             +------------+------------+
                          |
                          v
                   BRONZE LAYER
                  Raw Ingested Data
                          |
                          v
                  AZURE DATABRICKS
                     PySpark
                          |
                          v
                   SILVER LAYER
             Cleaned and Transformed Data
             Deduplication and Validation
                          |
                          v
                    GOLD LAYER
               Dimensional Data Model
                          |
               +----------+----------+
               |                     |
               v                     v
         Dimension Tables        Fact Table
          4 Dimensions       Streaming Activity
               |                     |
               +----------+----------+
                          |
                          v
                 Analytics-Ready Data
```

*This diagram represents the logical data flow. Exact storage paths, connections, and deployment configurations depend on the Azure environment.*

## 🖼️ Project Screenshots

The screenshots below document the Azure resources, Azure Data Factory workflows, Logic Apps integration, and Databricks data model used during the project.

### 1. Azure Resources

This screenshot shows the Azure resources configured for the project, including Azure SQL Database, Azure Data Factory, Azure Databricks, storage, and Logic Apps.

![Azure resources dashboard](images/azure-resources-dashboard.jpg)

### 2. Bronze Layer and Incremental Ingestion

The Azure Data Factory pipeline uses a lookup to retrieve the last CDC value, sets the current value, copies source data to the lake, and checks whether incremental data is available. Depending on the condition, the workflow updates the CDC value or performs the configured cleanup step.

![Azure Data Factory incremental ingestion overview](images/adf-incremental-ingestion-overview.jpg)

### 3. Incremental Loop Pipeline

The loop workflow coordinates ingestion activities and invokes alerting logic after the loop execution path. The exact outcome depends on the activity dependency conditions configured in the pipeline.

![Azure Data Factory incremental loop pipeline](images/adf-incremental-loop-pipeline.jpg)

### 4. Pipeline Activities and Run Results

The pipeline includes lookup, copy, conditional processing, CDC update, and alert activities. The captured run output shows successful activity statuses for the displayed run.

![Azure Data Factory loop activities](images/adf-incremental-loop-activities.jpg)

![Azure Data Factory pipeline run output](images/adf-pipeline-run-success.jpg)

### 5. Logic Apps Email Notification

The Logic App uses an HTTP request trigger and a Gmail **Send email (V2)** action. The request body schema includes the pipeline name and pipeline run ID, enabling a caller to provide run details for an email notification.

![Logic Apps HTTP trigger and email workflow](images/logic-app-http-email-workflow.jpg)

### 6. Databricks Silver and Gold Data Model

The Databricks workspace screenshot shows data engineering notebook code and the catalog/table browser, including Silver-layer dimension and fact tables. The displayed notebook contains date-dimension and streaming fact processing.

![Databricks Silver and Gold tables](images/databricks-silver-gold-tables.jpg)

> **🟩 If you rename an image:** update the corresponding Markdown image path in this README to match the new filename. For example, if `images/azure-resources-dashboard.jpg` is renamed, change the path in `![Azure resources dashboard](images/azure-resources-dashboard.jpg)` too.

## 🥉 1. Bronze Layer — Data Ingestion

The Bronze layer ingests raw Spotify source data using Azure Data Factory. Its purpose is to establish a repeatable ingestion process and retain source data for downstream processing.

### Key implementations

- Initial data ingestion from five source tables.
- Incremental ingestion for subsequent processing.
- SQL-based source ingestion scripts.
- Azure Data Factory pipeline orchestration.
- Parameterised pipeline execution.
- Reusable pipeline logic using looping.
- Backfilling support for historical data processing.

### Ingestion details

| Component | Details |
|---|---|
| Source tables | 5 |
| Initial-load SQL `INSERT` statements | 2,865 |
| Incremental-load SQL `INSERT` statements | 470 |
| Orchestration | Azure Data Factory |
| Source database | Azure SQL Database |

The initial-load scripts support first-time ingestion, while the incremental-load scripts support subsequent runs. Incremental processing can reduce the need for repeated full reloads when the configured logic identifies only new or changed records.

## 🥈 2. Silver Layer — Data Cleaning and Transformation

The Silver layer uses Azure Databricks and PySpark to transform raw ingested data into cleaner, more consistent datasets.

### Key implementations

- Data cleaning and standardisation.
- Data type handling.
- Removal of unnecessary columns.
- Deduplication.
- PySpark-based transformations.
- Incremental data processing.
- Delta Lake integration.
- Data quality checks.

**Technologies:** Azure Databricks, PySpark, Python, and Delta Lake.

The project also explores Auto Loader and Spark Structured Streaming for incremental file ingestion and streaming-oriented data processing.

## 🥇 3. Gold Layer — Dimensional Data Modelling

The Gold layer organises processed data into business-oriented datasets. A star schema was designed with **four dimension tables and one fact table**.

| Table | Purpose |
|---|---|
| `dim_users` | Descriptive information about users |
| `dim_artists` | Descriptive information about artists |
| `dim_tracks` | Descriptive information about tracks |
| `dim_date` | Date-related attributes for time-based analysis |
| `fact_streaming` | Streaming activity and associated dimension keys |

*These names are illustrative. Replace them with the actual table names used in your implementation if they differ.*

### Key concepts

- Star schema design.
- Fact and dimension tables.
- Dimensional data modelling.
- Slowly Changing Dimensions (SCDs).
- Incremental data loading.
- Data quality validation.
- Preparation of analytics-ready datasets.

The model can support analysis across users, artists, tracks, and dates, depending on the fields available in the source data.

## ⚙️ Azure Data Factory Workflows

Azure Data Factory orchestrates ingestion and coordinates data movement through the pipeline.

### Initial load

The initial-load workflow processes the source data for first-time ingestion.

### Incremental load

The incremental-load workflow processes subsequent changes according to the configured source queries and change-tracking logic.

### Metadata-driven processing

Metadata-driven pipeline concepts make ingestion logic reusable across multiple configured source tables, reducing duplicated hardcoded workflows.

### Parameterisation and looping

Parameters and looping allow the same ingestion logic to run for different configured tables or inputs.

### Backfilling

Backfilling supports historical data processing or reprocessing according to the pipeline configuration.

### Logic Apps integration

Logic Apps integration with Azure Data Factory was explored to understand how Azure services can work together in automated workflows. The captured Logic App uses an HTTP request trigger and an email action.

## 🔄 Incremental Data Processing

The general workflow is:

1. Configure the source and ingestion parameters.
2. Run the initial load to ingest the required source data.
3. Run incremental ingestion for subsequent changes.
4. Process ingested data through the Bronze and Silver layers.
5. Apply transformations and update downstream datasets according to the configured logic.
6. Validate processed data using the implemented quality checks.

The exact incremental logic depends on the source queries, change tracking, and pipeline configuration.

## 🧩 Metadata-Driven PySpark and Jinja2 Workflows

The project incorporates metadata-driven processing concepts using PySpark and Jinja2 to support reusable transformation logic.

Key concepts include:

- Reusable processing templates.
- Parameterised transformations.
- Looping through configured inputs.
- Separation of processing logic from configuration.
- Backfilling support.
- Reduced duplication across workflows.

## 🗄️ Delta Lake and Databricks Capabilities

| Technology | Purpose |
|---|---|
| Delta Lake | Reliable data storage and incremental data operations |
| Auto Loader | Incremental ingestion of files from supported cloud storage |
| Spark Structured Streaming | Stream-processing capabilities |
| Delta Live Tables | Declarative data pipeline development |
| Unity Catalog | Centralised data governance and access management |
| Databricks Asset Bundles | Databricks project configuration and deployment |
| Jinja2 | Template-based processing logic |
| PySpark | Distributed data processing and transformation |

These technologies represent capabilities incorporated into or explored during the project. Not every item should be interpreted as a separately deployed production component.

## 🛠️ Technology Stack

**Cloud and data engineering**
- Microsoft Azure
- Azure Data Factory
- Azure SQL Database
- Azure Databricks
- Delta Lake

**Programming and processing**
- Python
- PySpark
- SQL
- Jinja2

**Data engineering concepts**
- Medallion Architecture
- Incremental data loading
- Metadata-driven pipelines
- Backfilling
- Parameterisation and looping
- Dimensional data modelling
- Slowly Changing Dimensions
- Data quality checks
- Spark Structured Streaming
- Auto Loader

**Governance and deployment**
- Unity Catalog
- Delta Live Tables
- Databricks Asset Bundles
- Git and GitHub

## 📁 Repository Structure

```text
SpotifyAzureProject/
├── dataset/             # Dataset definitions and configuration
├── factory/             # Azure Data Factory factory configuration
├── linkedService/       # Linked service definitions
├── pipeline/            # Pipeline definitions and orchestration
├── source_scripts/      # SQL source and ingestion scripts
├── cdc.json             # CDC-related configuration
├── empty.json           # Supporting JSON configuration
├── loop_input           # Loop input configuration
├── publish_config.json  # Publish-related configuration
├── images/              # Project screenshots used in this README
└── README.md             # Project documentation
```

The repository also includes Silver- and Gold-layer code in the `adf_publish` branch, as described in the project documentation. The exact contents of each folder may evolve over time.

## 🚀 Getting Started

### Prerequisites

- An Azure subscription.
- An Azure Data Factory instance.
- An Azure SQL Database with the required source tables.
- Azure storage configured for data ingestion and processing.
- An Azure Databricks workspace.
- Appropriate Azure permissions and connectivity.
- Basic knowledge of SQL, Python, PySpark, and Azure data services.

### 1. Clone the repository

```bash
git clone https://github.com/malikareeba/SpotifyAzureProject.git
cd SpotifyAzureProject
```

### 2. Configure Azure resources

Set up Azure Data Factory, Azure SQL Database, storage, and Azure Databricks as required by the pipeline.

### 3. Configure linked services

Review `linkedService/` and update connection settings for your environment. Use secure authentication and secret management. Never commit passwords, access keys, or connection strings containing secrets to GitHub.

### 4. Configure datasets and pipelines

Review `dataset/`, `pipeline/`, and `factory/`. Update resource references, table names, parameters, and storage locations as required.

### 5. Run the initial load

Publish the configured Azure Data Factory resources and execute the initial ingestion workflow. Validate the ingested source data.

### 6. Execute incremental ingestion

Run the incremental pipeline according to the configured source queries and change-tracking logic. Verify that the expected records are processed.

### 7. Process the Silver layer

Configure Databricks and execute the relevant PySpark transformations. Validate data types, deduplication, transformation results, and data quality checks.

### 8. Build and validate the Gold layer

Execute the relevant Gold-layer processing logic and validate the fact and dimension tables.

*These are general setup steps. Execution requires the original project configurations, compatible Azure resources, and any notebooks or scripts maintained outside the repository.*

## 📊 Potential Analytical Use Cases

The Gold-layer star schema can support use cases such as:

- Analysing streaming activity over time.
- Exploring streaming trends across artists.
- Comparing activity across tracks.
- Analysing user activity.
- Building reporting datasets based on available streaming attributes.

These are potential use cases; actual results depend on the source data and available fields.

## 🧠 Key Learnings

Through this project, I gained practical experience with:

- Designing an end-to-end cloud data pipeline.
- Orchestrating ingestion using Azure Data Factory.
- Working with Azure SQL Database as a source.
- Building initial and incremental ingestion workflows.
- Implementing parameterisation, looping, and backfilling.
- Applying the Bronze, Silver, and Gold Medallion Architecture.
- Cleaning and transforming data with PySpark.
- Working with Delta Lake and incremental processing.
- Applying dimensional modelling and star schema concepts.
- Understanding Slowly Changing Dimensions.
- Implementing data quality checks.
- Exploring streaming, governance, and deployment capabilities.
- Managing project code using GitHub.

## 🔮 Potential Future Enhancements

- Add automated unit and integration tests.
- Improve pipeline logging, monitoring, and failure notifications.
- Expand data quality rules and reconciliation checks.
- Automate deployments through CI/CD workflows.
- Add data lineage and schema documentation.
- Build dashboards to visualise streaming activity.
- Extend incremental processing and change data capture handling.

## 📚 Learning Journey

This project was a major milestone in my **#100DaysOfDataEngineering** journey.

### Day 60 — Project kick-off
- Created Azure resources.
- Configured Azure SQL Database as a source.
- Built incremental ingestion pipelines using Azure Data Factory.
- Added backfilling functionality.

### Day 61 — Pipeline automation
- Continued implementing backfilling.
- Added looping to make pipeline execution more reusable.
- Integrated Logic Apps with Azure Data Factory.

### Day 63 — Project completion
- Completed the end-to-end Spotify Azure Data Engineering Project.
- Implemented Bronze-layer raw data ingestion.
- Developed Silver-layer transformations using Azure Databricks and PySpark.
- Created the Gold-layer dimensional data model.
- Explored incremental processing, Delta Lake, SCDs, and additional Databricks capabilities.
- Organised and published project code on GitHub.

This journey helped me connect individual data engineering concepts and understand how they work together in a complete pipeline.

## 👩‍💻 Author

**Areeba Malik**  
Aspiring Data Engineer | Computer Science Student | Azure Data Engineering

- **GitHub:** [malikareeba](https://github.com/malikareeba)
- **LinkedIn:** [Areeba Malik](https://www.linkedin.com/in/areeba-malik234/)
- **Project Repository:** [SpotifyAzureProject](https://github.com/malikareeba/SpotifyAzureProject)

## 🔗 Project Links

- [GitHub Repository](https://github.com/malikareeba/SpotifyAzureProject)
- [Azure Data Engineering Learning Tutorial](https://www.youtube.com/watch?v=vJM0wDrTRxI)

## 🏷️ Tags

`Azure` `AzureDataFactory` `AzureDatabricks` `AzureSQLDatabase` `PySpark` `Python` `SQL` `DeltaLake` `MedallionArchitecture` `DataEngineering` `IncrementalLoading` `DimensionalModelling` `DataPipeline` `GitHub` `100DaysOfDataEngineering`

---

⭐ **Thank you for visiting my project!**

This project represents an important step in my data engineering journey, bringing together cloud services, data ingestion, transformation, and dimensional modelling in an end-to-end solution.
