About Project 

- This project to create a datamart which data comes from https://www.kaggle.com/datasets/datascientist97/e-commerece-sales-data-2024.
- ETL process using python script and sqlite as data storage for datamart.
- Automate jobs using task scheduler in windows system by running batch file that calls python script or put the python/bat script in apache airflow
- Datamart results in sqlite databases are used to build dashboards with Power BI

ETL Process
- Data collecting from E-commerce Sales Data 2024 dataset on Kaggle. consist of customer_details.csv,E-commerece sales data 2024.csv, product_details.csv
- Data extraction using python script. This stage is a process
    - Checking data
    - Cleaning data
    - Remove duplicate
    - Remove unused column
    - Transform data
- Load data into sqlite databases from pandas dataframes, the steps are
    - Create table for datamart purpose (staging,dimension and fact table).
    - Insert into table staging for temporary data
    - Load data from table staging to datamart (dimension and fact table). 
        
Databases Structure (Please check sql file attachment or documentation file for detail table structure)
- Customer_Staging
- Product_Staging
- Sales_Staging
- Custome_dim
- Product_dim
- Sales_fact
- Date_dim
  
Datawarehouse design using star schema model (check Pbix file to view design schema or documentation file). Example results can be reviewed in the Power BI report

Attachment file
- data.zip (source data)
- script_databases_sqlite.zip (sql file script)
- exctract_data.zip (python and batch file script)
- sales_data_perqara_report.zip (Power BI report)
- Documentation Project (docx file)
- sales_data.zip (ready-made sqlite database)

Installation instruction
- Extract source data (data.zip)
- Create sqlite database with the name sales_data.db and then execute sql file to generate table (script_databases_sqlite.zip) or use a ready-made sqlite database (sales_data.zip).
- Extract python and batch file script then modified code for adjust data source location and sqlite databases (exctract_data.zip)
- Modified connection in power BI report to access your datamart and install odbc diver for sqlite if needed (sales_data_perqara_report.zip).
- Modified batch file to adjus script python location
- Testing batch file to run program
- Create task schedule in windows system to run program autamatically (call batch file) or put the python/bat script in apache airflow.

Thanks
