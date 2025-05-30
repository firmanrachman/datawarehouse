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
