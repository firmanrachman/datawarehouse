# This section to explain python script step by step

## Initialing and Checking Data

````
import pandas as pd
import sqlite3

df_customer = pd.read_csv('E:\Perqara\data\customer_details.csv')
df_sales_data = pd.read_csv('E:\Perqara\data\E-commerece sales data 2024.csv',)
df_product = pd.read_csv('E:\Perqara\data\product_details.csv')

# print(df_customer)
# print(df_sales_data)
# print(df_product)

# df_customer.head()
# df_sales_data.head()
# df_product.head()

# df_customer.info()
# df_sales_data.info()
# df_product.info()

# df_customer.dtypes
# df_sales_data.dtypes
# df_product.dtypes

# print (df_customer.isnull().sum())
# print (df_sales_data.isnull().sum())
# print (df_product.isnull().sum())
````
## Cleaning data

````
# -- cleansing data, remove row empty, drop column unused, etc --------------------------

clean_df_customer = df_customer
clean_df_sales_data = df_sales_data
clean_df_product = df_product

# print(clean_df_customer)
# print(clean_df_sales_data)
# print(clean_df_product)

## Remove Row data if id is null

clean_df_customer.dropna(subset=['Customer ID'], inplace=True)
clean_df_sales_data.dropna(subset=['user id'], inplace=True)
clean_df_product.dropna(subset=['Uniqe Id'], inplace=True)

````

## Remove duplicate for column ID and keep last data
````
clean_df_customer.drop_duplicates(subset=['Customer ID'], keep='last')
clean_df_sales_data.drop_duplicates(subset=['user id'], keep='last')
clean_df_product.drop_duplicates(subset=['Uniqe Id'], keep='last')
````

## Remove unused column
````
clean_df_customer = clean_df_customer.loc[:, ~clean_df_customer.columns.str.contains('^Unnamed')]
clean_df_sales_data = clean_df_sales_data.loc[:, ~clean_df_sales_data.columns.str.contains('^Unnamed')]
clean_df_product = clean_df_product.loc[:, ~clean_df_product.columns.str.contains('^Unnamed')]
````

## Connect to databases SQL Lite as datawarehouse
````
sqliteConnection = sqlite3.connect('E:\Perqara\sales_data.db')
cursor = sqliteConnection.cursor
````

## Insert into table staging in datawarehouse

````
# table staging used as a temporary  table before insert to the datamart

clean_df_customer.to_sql(name='customer_staging', con=sqliteConnection, if_exists='replace', index=False)
clean_df_sales_data.to_sql(name='sales_staging', con=sqliteConnection, if_exists='replace', index=False)
clean_df_product.to_sql(name='product_staging', con=sqliteConnection, if_exists='replace', index=False)
````

## insert into data mart from table staging with insert or replace methods
````
# INSERT OR REPLACE would insert if the row does not exist, or replace the values if exist based on primary key
# insert customer data
sql_insert_customer =""" INSERT OR REPLACE into customer_dim (Customer_ID,
Age,
Gender,
Item_Purchased,
Category,
Purchase_Amount_USD,
Location,
Size,
Color,
Season,
Review_Rating,
Subscription_Status,
Shipping_Type,
Discount_Applied,
Promo_Code_Used,
Previous_Purchases,
Payment_Method,
Frequency_of_Purchases)
select "Customer ID",
"Age",
"Gender",
"Item Purchased",
"Category",
"Purchase Amount (USD)",
"Location",
"Size",
"Color",
"Season",
"Review Rating",
"Subscription Status",
"Shipping Type",
"Discount Applied",
"Promo Code Used",
"Previous Purchases",
"Payment Method",
"Frequency of Purchases" from customer_staging; """
cursor.execute(sql_insert_customer)

# insert product data
sql_insert_product =""" INSERT OR REPLACE into product_dim (Uniqe_Id,
Product_Name,
category,
Upc_Ean_Code,
Selling_Price,
Model_Number,
About_Product,
Product_Specification,
Technical_Details,
Shipping_Weight,
Product_Dimensions,
Image,
Variants,
Product_Url,
Is_Amazon_Seller
)
select "Uniqe Id",
"Product Name",
"category",
"Upc Ean Code",
"Selling Price",
"Model Number",
"About Product",
"Product Specification",
"Technical Details",
"Shipping Weight",
"Product Dimensions",
"Image",
"Variants",
"Product Url",
"Is Amazon Seller"
 from product_staging; """
cursor.execute(sql_insert_product)

# insert sales data
sql_insert_sales=""" INSERT OR REPLACE into sales_fact
(user_id,product_id,interaction_type,time_stamp,datekey)
select  "user id","product id",
"Interaction type",
"time stamp", 
substr("time stamp",7,4)||substr("time stamp",4,2) || substr("time stamp",1,2)as datekey 
from sales_staging; """
cursor.execute(sql_insert_sales)

sqliteConnection.commit()
cursor.close()
sqliteConnection.close()

````
