# Azure Data Factory Orchestration for Cafe Company Project

## Table of Contents

1. [Project Description](#project-description)
2. [Technical Components](#technical-components)
3. [Pipeline Architecture](#pipeline-architecture)
   - [1- PL_Extract_Data](#pl_extract_data)
   - [2- PL_Trans_Load_Fact_Table](#pl_trans_load_fact_table)
   - [3- PL_Load_Dim_Tables](#pl_load_dim_tables)
   - [4- Type of trigger](#type-of-trigger)
4. [Monitoring and notifications](#monitoring-and-notifications)
5. [Results](#results)

## Project Description <a name="project-description"></a>

<p align="justify">This project addresses the problem that a company may face when managing data distributed across multiple subsidiaries. In this project, the company has adopted a decentralized data management structure, where each subsidiary stores its data locally in SharePoint (simulated in this project using GitHub), while the headquarters consolidates this data into a Data Lake for global analysis.</p>

<p align="justify">This data orchestration project with Azure Data Factory <b>aims to automate the extraction, transformation, and loading (ETL) of sales data</b> from subsidiaries through scheduled and monitored pipelines.</p>

## Technical Components <a name="technical-components"></a>

 - **SharePoint/GitHub:** Data source for subsidiaries' sales records.
 - **Azure Data Lake:** Centralized storage for transformed data.
 - **Azure Data Factory (ADF):** Data integration and orchestration platform.
 - **ETL Pipelines:** Data transformation and loading processes.
 - **Scheduled Triggers:** Automated pipeline execution.
 - **Monitoring and Notifications:** Alerts for success or failure events.

## Pipeline Architecture <a name="pipeline-architecture"></a>

<p align="justify">A <b>modular pipeline structure</b> was chosen to improve reusability, maintainability, and scalability. By breaking processes into smaller modules, updates and reuse can be done without affecting the main workflow. It also allows for more efficient error management, optimizes performance by enabling parallel execution, and enhances the understanding of the workflow. This modular approach also <b>simplifies version control</b> and deployment of specific changes without disrupting the overall system.</p>

The architecture is based on the following steps:

<img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Production.png" alt="image" width="500" height="auto">

### **1- PL_Extract_Data:** <a name="pl_extract_data"></a>

<p align="justify">Extracting sales data for January from the subsidiaries' SharePoint repositories (simulated in GitHub), by using a <b>dynamic copy parameter to extract the path URL and file destination</b>b> within a forEach activity that reads the corresponding url address and loads the data through a json file placed in the Lookup Activity.</p>

<img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data.png" alt="image" width="500" height="auto">

#### **Steps:**
  - **Creation a Dynamic Copy Activity:**
    
     **1- Creation of source connection:**
    
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/Dynamic_Copy_Activity_source_inside.png" alt="image" width="500" height="auto">
     
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/Dynamic_Copy_Activity_source.png" alt="image" width="500" height="auto">

     **2- Creation of sink connection:**
    
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/Dynamic_Copy_Activity_sink_inside.png" alt="image" width="500" height="auto">
     
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/Dynamic_Copy_Activity_sink.png" alt="image" width="500" height="auto">

  - **Creation of LookUp Activity by using json parameter:** [(Format of JSON)](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Codes/Dynamic_Pipeline.json)
    
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/Parameter_of_LookUp_Activity.png" alt="image" width="500" height="auto">
     
  - **Creation of forEach Activity and put inside the Dynamic Copy:** (Extract the values from the LookUp Activity which uses the json script)
    
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Extract_Data_Description/ForEach_Activity.png" alt="image" width="500" height="auto">

#### PL_Extract_Data results:

   - **fact-data Folder:**

     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/Storage_fact-data.png" alt="image" width="200" height="auto">

### **2- PL_Trans_Load_Fact_Table:**<a name="pl_trans_load_fact_table"></a>

Distributing the transformed data to two destinations:
 - **Data Science Team:** The data undergoes aggregation and transformation through an ETL process.
 - **Headquarters:** Additional transformations are performed to convert the data into KPIs for quick reporting.

<img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Trans_Load_Fact_Table.png" alt="image" width="250" height="auto">

#### **Steps:**
  - **Creation a Data flow Activity:**
    
     **1- Creation of source connection:** It is used a with path to extract all csv files:

    <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Trans_Load_Fact_Table/PL_Trans_Load_Fact_Table_inside_source.png" alt="image" width="500" height="auto">

     **2- Transform the dataset for each purpose:**

    <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Trans_Load_Fact_Table/PL_Trans_Load_Fact_Table_inside.png" alt="image" width="800" height="auto">
     
     **3- Creation of sink connection:** It is used a single partition with the purpose to maintain just one file

    <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Trans_Load_Fact_Table/PL_Trans_Load_Fact_Table_inside_sink.png" alt="image" width="500" height="auto">

### **3- PL_Load_Dim_Tables:** <a name="pl_load_dim_tables"></a>

Extracting data from dimensional tables to the Data Science folder:

<img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables.png" alt="image" width="500" height="auto">

#### **Steps:**
  - **Creation of Get Metadata Activity:** For this step, a Get Metadata activity has been used to extract all the files from the dimensional folder. Get Metadata retrieves the file names.
    
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables/PL_Load_Dim_Tables_getMetadata.png" alt="image" width="500" height="auto">
     
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables/PL_Load_Dim_Tables_getMetadata_inside.png" alt="image" width="500" height="auto">
     
     **Get Metadata Output:**
     
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables/PL_Load_Dim_Tables_getMetadata_output.png" alt="image" width="500" height="auto">
     
  - **Creation of forEach Activity and put inside the Dynamic Copy:** (Extract the ChildItems values from the Get Matadata Activity)
    
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables/PL_Load_Dim_Tables_forEach.png" alt="image" width="500" height="auto"> 
  
  - **Creation a Dynamic Copy Activity:**
    
     **1- Creation of source connection:**
    
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables/PL_Load_Dim_Tables_CopyActivity_source_inside.png" alt="image" width="500" height="auto">
     
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables/PL_Load_Dim_Tables_CopyActivity_source.png" alt="image" width="500" height="auto">

     **2- Creation of sink connection:**
    
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables/PL_Load_Dim_Tables_CopyActivity_sink_inside.png" alt="image" width="500" height="auto">
     
     <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables/PL_Load_Dim_Tables_CopyActivity_sink_inside.png" alt="image" width="500" height="auto">

### 4- Type of trigger: <a name="type-of-trigger"></a>

<p align="justify">Since the subsidiaries upload their data to their respective repositories on GitHub on a specific day, but the <b>exact time of upload is unknown</b>, it was decided to use a <b>scheduled trigger</b> to ensure that the pipelines run at regular intervals every hour throughout that day.</p>

The workflow would be as follows:

- The subsidiaries upload the data to their respective GitHub repositories on a specific day, but there is no exact schedule for this.
- To ensure that the data is extracted as soon as it is uploaded, a scheduled trigger is set up to run the data extraction pipeline every hour during that day.
- In this way, even though the exact time when the data will be available cannot be predicted, the pipeline will continuously check, and when the data becomes available, the extraction, transformation, and loading (ETL) process will be initiated.

<p align="justify">This approach also has the advantage of <b>ensuring there are no significant delays in processing</b>, as the pipeline will automatically execute at regular intervals, without the need for manual intervention.</p>

This is the scheduled trigger design: 

<img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/Trigger.png" alt="image" width="500" height="auto">

## Monitoring and Notifications: <a name="monitoring-and-notifications"></a>

In this case, it has been decided to create an email notification only in the event of a failure in any of the created pipelines. These are the characteristics of the notification:

<img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/Notifications/Alert_logic_Failure.png" alt="image" width="500" height="auto">

- **Characteristics:**

  <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/Notifications/Alert_logic_Failure_charateristics.png" alt="image" width="500" height="auto"> 
  <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/Notifications/Alert_logic_Failure_charateristics_2.png" alt="image" width="500" height="auto">

- **Type of notification:**
  
  <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/Notifications/Alert_logic_Failure_notification.png" alt="image" width="500" height="auto">
  <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/Notifications/Alert_logic_Failure_notification_inside.png" alt="image" width="500" height="auto">

## Results: <a name="results"></a>

- [**Data science Folder**](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/tree/main/Data/Pipeline_Results/data-science-models)
  
<img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/Storage_data-science-models.png" alt="image" width="200" height="auto">

- [**Reporting Folder**](https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/tree/main/Data/Pipeline_Results/hq-data-monthly-report)

<img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/Storage_hq-data-monthly-report.png" alt="image" width="250" height="auto">
