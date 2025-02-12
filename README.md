# Azure-Data-Factory-Data-Orchestration-Project

## Project Description

Our organization has adopted a decentralized data management structure for its subsidiaries. Each subsidiary stores its data locally in SharePoint (simulated in this project using GitHub), while the headquarters consolidates these data into a Data Lake for global analysis.

This Azure Data Factory data orchestration project aims to automate the extraction, transformation, and loading (ETL) of sales data from subsidiaries through scheduled and monitored pipelines.

## Pipeline Architecture

The architecture is based on the following steps:

1- **PL_Extract_Data:**: Extracting sales data for January from the subsidiaries' SharePoint repositories (simulated in GitHub), by using a dynamic copy parameter within a forEach activity that reads the corresponding url address and loads the data through a json file placed in the Lookup Activity.

![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data.png)

 - Steps:
   - Creation a Dynamic Copy Activity:
     Creation of source:
     ![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/Dynamic_Copy_Activity_source_inside.png)
     ![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/Dynamic_Copy_Activity_source.png)

     Creation of sink:
     ![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/Dynamic_Copy_Activity_sink_inside.png)
     ![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/Dynamic_Copy_Activity_sink.png)

   - Creation of LookUp Activity by using json parameter:
     [Parameters JSON](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Codes/Dynamic_Pipeline.json)
     ![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/Parameter_of_LookUp_Activity.png)
     
   - Creation of forEach Activity and put inside the Dynamic Copy:
     ![image](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/ForEach_Activity.png)

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
