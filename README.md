# Azure-Data-Factory-Data-Orchestration-Project

## Project Description

Our organization has adopted a decentralized data management structure for its subsidiaries. Each subsidiary stores its data locally in SharePoint (simulated in this project using GitHub), while the headquarters consolidates these data into a Data Lake for global analysis.

This Azure Data Factory data orchestration project aims to automate the extraction, transformation, and loading (ETL) of sales data from subsidiaries through scheduled and monitored pipelines.

## Pipeline Architecture

The primary pipeline is responsible for:

1- Extracting sales data for January from the subsidiaries' SharePoint repositories (simulated in GitHub).

![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data.png)

2- Distributing the transformed data to two destinations:
 - **Data Science Team:** The data undergoes aggregation and transformation through an ETL process.
 - **Headquarters:** Additional transformations are performed to convert the data into KPIs for quick reporting.

![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Trans_Load_Fact_Table.png)
![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Trans_Load_Fact_Table_inside.png)

3- Extracting data from dimensional tables for the Data Science team.

![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables.png)

4- Executing the pipelines automatically via a scheduled trigger.

![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Production.png)

5- Monitoring pipeline execution by sending success or failure notifications.

## Technical Components

 - **Azure Data Factory (ADF):** Data integration and orchestration platform.
 - **SharePoint/GitHub:** Data source for subsidiaries' sales records.
 - **Azure Data Lake:** Centralized storage for transformed data.
 - **ETL Pipelines:** Data transformation and loading processes.
 - **Scheduled Triggers:** Automated pipeline execution.
 - **Monitoring and Notifications:** Alerts for success or failure events.
