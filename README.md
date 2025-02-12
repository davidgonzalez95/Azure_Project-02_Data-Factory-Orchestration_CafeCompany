# Azure-Data-Factory-Data-Orchestration-Project

## Project Description

Our organization has adopted a decentralized data management structure for its subsidiaries. Each subsidiary stores its data locally in SharePoint (simulated in this project using GitHub), while the headquarters consolidates these data into a Data Lake for global analysis.

This Azure Data Factory data orchestration project aims to automate the extraction, transformation, and loading (ETL) of sales data from subsidiaries through scheduled and monitored pipelines.

## Pipeline Architecture

The primary pipeline is responsible for:

1- Extracting sales data for January from the subsidiaries' SharePoint repositories (simulated in GitHub).

2- Distributing the transformed data to two destinations:
 - Data Science Team: The data undergoes aggregation and transformation through an ETL process.
 - Headquarters: Additional transformations are performed to convert the data into KPIs for quick reporting.

3- Extracting data from dimensional tables for the Data Science team.
4- Executing the pipelines automatically via a scheduled trigger.
5- Monitoring pipeline execution by sending success or failure notifications.

## Technical Components

 - Azure Data Factory (ADF): Data integration and orchestration platform.
 - SharePoint/GitHub: Data source for subsidiaries' sales records.
 - Azure Data Lake: Centralized storage for transformed data.
 - ETL Pipelines: Data transformation and loading processes.
 - Scheduled Triggers: Automated pipeline execution.
 - Monitoring and Notifications: Alerts for success or failure events.
