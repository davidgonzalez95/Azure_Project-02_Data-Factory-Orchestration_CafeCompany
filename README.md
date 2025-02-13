# Azure-Data-Factory-Data-Orchestration-Project

## Project Description

Our organization has adopted a decentralized data management structure for its subsidiaries. Each subsidiary stores its data locally in SharePoint (simulated in this project using GitHub), while the headquarters consolidates these data into a Data Lake for global analysis.

This Azure Data Factory data orchestration project aims to automate the extraction, transformation, and loading (ETL) of sales data from subsidiaries through scheduled and monitored pipelines.

## Pipeline Architecture

The architecture is based on the following steps:

### **1- PL_Extract_Data:** 
Extracting sales data for January from the subsidiaries' SharePoint repositories (simulated in GitHub), by using a **dynamic copy parameter to extract the path URL and file destination** within a forEach activity that reads the corresponding url address and loads the data through a json file placed in the Lookup Activity.

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

### **2- PL_Trans_Load_Fact_Table:** 
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
     
     **3- Creation of sink connection:** It is used a single partition with the purpose to maintain just one file**

    <img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Trans_Load_Fact_Table/PL_Trans_Load_Fact_Table_inside_sink.png" alt="image" width="500" height="auto">

### **3- PL_Load_Dim_Tables:Extracting data from dimensional tables for the Data Science team:**

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

### 4- PL_Production: Executing the pipelines automatically via a scheduled trigger.

<img src="https://github.com/davidgonzalez95/Azure-Data-Factory-Data-Orchestration-Project/blob/main/Pictures/PL_Load_Dim_Tables.png" alt="image" width="500" height="auto">

### 5- Monitoring pipeline execution by sending success or failure notifications.

## Technical Components

 - **Azure Data Factory (ADF):** Data integration and orchestration platform.
 - **SharePoint/GitHub:** Data source for subsidiaries' sales records.
 - **Azure Data Lake:** Centralized storage for transformed data.
 - **ETL Pipelines:** Data transformation and loading processes.
 - **Scheduled Triggers:** Automated pipeline execution.
 - **Monitoring and Notifications:** Alerts for success or failure events.
