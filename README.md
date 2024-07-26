# Kore Assignment
Wen Cui 

Objective:
- 
Develop an SSIS-based ETL process that migrates data from a CSV file to a production SQL Server table, via a staging table.

Task Description:
- 
1. Data Extraction and Type Standardization to Staging Table:

Extract: Use an SSIS package to extract data from a CSV file. Assume the source data in the CSV is in string format and may
contain various errors.

Transform: Load the data into a staging table, converting string values to appropriate SQL data types (int, string, float, date, etc.).
This step ensures all data is correctly formatted for further processing.

2. Data Cleaning in Staging Table:

Clean: After data is loaded into the staging table with correct data types, perform data cleaning operations including:

Removing duplicates: Identify and remove duplicate records based on specific criteria or key columns.
Handling error records: Isolate records that do not meet data quality standards (e.g., null values in non-nullable fields, incorrect
formats) for review or correction.

3. Incremental Load to Production Table:
   
Load: Ensure the production table includes an ID column for unique record identification. Create a data flow from the staging table to
the production table, differentiating new from existing records based on ID. Insert new records and update existing records.

Code Description:
- 
* data.csv: Records loaded into the SQL server
* create_db: SQL scripts loaded into SSMS to create the initial database and load data
* ZIP: contains the data flow tasks and control flow tasks for SSIS packaging

Design Choices:
- 
The overall control flow graph is shown below, we then describe each step in more detail. 
<img width="381" alt="Screen Shot 2024-07-26 at 11 19 14 AM" src="https://github.com/user-attachments/assets/571c5c01-c6a2-463e-b900-fd2a53fbfc9d">

* Data Extraction and Load to Staging Table: this is implemented using the first Data flow task, which contains loading from CSV, data type conversion, filtering out invalid input (NULL values), removing duplicated IDs (using SORT) and output to a staging table. Additionally, the error output is written to a CSV file. Details are shown in the figure below.
<img width="420" alt="Screen Shot 2024-07-26 at 11 18 12 AM" src="https://github.com/user-attachments/assets/63cf2084-1bc2-4d7d-83bc-2dd4eaf924d2">

* Data Cleaning: this is implemented as an execution SQL task in the Control Flow graph, which loads data into the staging table.
* Increment Load: this is implemented as the second data flow task, which contains, loading from the staging table, check and split records. Then, the insertion of new records and update of existing records are performed, as shown in the figure below
<img width="559" alt="Screen Shot 2024-07-26 at 11 17 25 AM" src="https://github.com/user-attachments/assets/eab85e5a-12bf-4192-8b08-95651a25afb6">

Challenges and improvements:
-
I used Azure server for SQL server instantiation. It works in general but the implementation of Visual Studio requires AAD registration to deploy the databases, which I did not realize earlier. 

Adding AAD registration is a costly operation, therefore, I had to draw out the design for now. If time allowed, other servers could be explored such as usage of OCDB driver connecting SQL databases. 
