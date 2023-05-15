---
title: "kaggle intro sql "
excerpt_separator: "<!--more-->"
categories:
  - dataScience
tags:
  - kaggle
  - sql

toc : true
toc_sticky : true
---

# 1. Getting Started With SQL and BigQuery
> Learn the workflow for handling big datasets with BigQuery and SQL    
```python
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()

# Construct a reference to the "hacker_news" dataset
dataset_ref = client.dataset("hacker_news", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)

# List all the tables in the "hacker_news" dataset
tables = list(client.list_tables(dataset))

# Print names of all tables in the dataset (there are four!)
for table in tables:  
    print(table.table_id)
    
# comments
# full
# full_201510
# stories    

# Construct a reference to the "full" table
table_ref = dataset_ref.table("full")

# API request - fetch the table
table = client.get_table(table_ref)
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/eeb6cc93-a817-4573-9f2b-ddce058005d6)

```python
# Print information on all the columns in the "full" table in the "hacker_news" dataset
table.schema
```

```
[SchemaField('title', 'STRING', 'NULLABLE', 'Story title', (), None),
 SchemaField('url', 'STRING', 'NULLABLE', 'Story url', (), None),
 SchemaField('text', 'STRING', 'NULLABLE', 'Story or comment text', (), None),
 SchemaField('dead', 'BOOLEAN', 'NULLABLE', 'Is dead?', (), None),
 SchemaField('by', 'STRING', 'NULLABLE', "The username of the item's author.", (), None),
 SchemaField('score', 'INTEGER', 'NULLABLE', 'Story score', (), None),
 SchemaField('time', 'INTEGER', 'NULLABLE', 'Unix time', (), None),
 SchemaField('timestamp', 'TIMESTAMP', 'NULLABLE', 'Timestamp for the unix time', (), None),
 SchemaField('type', 'STRING', 'NULLABLE', 'Type of details (comment, comment_ranking, poll, story, job, pollopt)', (), None),
 SchemaField('id', 'INTEGER', 'NULLABLE', "The item's unique id.", (), None),
 SchemaField('parent', 'INTEGER', 'NULLABLE', 'Parent comment ID', (), None),
 SchemaField('descendants', 'INTEGER', 'NULLABLE', 'Number of story or poll descendants', (), None),
 SchemaField('ranking', 'INTEGER', 'NULLABLE', 'Comment ranking', (), None),
 SchemaField('deleted', 'BOOLEAN', 'NULLABLE', 'Is deleted?', (), None)]
```

```python
# Preview the first five lines of the "full" table
client.list_rows(table, max_results=5).to_dataframe()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/5e5067ad-1db7-466b-be45-a7504dab3b5e)

```python
# Preview the first five entries in the "by" column of the "full" table
client.list_rows(table, selected_fields=table.schema[:1], max_results=5).to_dataframe()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/abab9623-4722-42c6-bae7-0d2e534590de)

>> Each Kaggle user can scan 5TB every 30 days for free. Once you hit that limit, you'll have to wait for it to reset.   

## 실습
```python
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()

# Construct a reference to the "chicago_crime" dataset
dataset_ref = client.dataset("chicago_crime", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)

# List all the tables in the "chicago_crime" dataset
tables = list(client.list_tables(dataset))

# Print number of tables in the dataset
print(len(tables))
```

```python
# Construct to the "crime" table
table_ref = dataset_ref.table("crime")

# API request - fetch the table
table = client.get_table(table_ref)

# Print information on all the columns in the "crime" table in the "chicago_crime" dataset
print(table.schema)
```
# 2. Select, From & Where
> The foundational compontents for all SQL queries   
## SELECT, FROM
![image](https://github.com/younlea/younlea.github.io/assets/1435846/81f59e5e-ae45-4838-b3fb-70740748274e)
## WHERE
![image](https://github.com/younlea/younlea.github.io/assets/1435846/a3fa7529-39ec-4686-8761-a01bec48f51f)

```python
# Query to select all the items from the "city" column where the "country" column is 'US'
query = """
        SELECT city
        FROM `bigquery-public-data.openaq.global_air_quality`
        WHERE country = 'US'
        """
# Create a "Client" object
client = bigquery.Client()

# Set up the query
query_job = client.query(query)

# API request - run the query, and return a pandas DataFrame
us_cities = query_job.to_dataframe()

# What five cities have the most measurements?
us_cities.city.value_counts().head()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/29cade27-839a-48bc-99ef-ee47f06dcd54)

## 실습
```python
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()

# Construct a reference to the "openaq" dataset
dataset_ref = client.dataset("openaq", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)

# Construct a reference to the "global_air_quality" table
table_ref = dataset_ref.table("global_air_quality")

# API request - fetch the table
table = client.get_table(table_ref)

# Preview the first five lines of the "global_air_quality" table
client.list_rows(table, max_results=5).to_dataframe()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/d9814997-bf9c-4453-8044-113d60be8413)
```python
# Query to select countries with units of "ppm"
first_query = """
        SELECT country
        FROM `bigquery-public-data.openaq.global_air_quality`
        WHERE unit = 'ppm'
        """ # Your code goes here

# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 10 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
first_query_job = client.query(first_query, job_config=safe_config)

# API request - run the query, and return a pandas DataFrame
first_results = first_query_job.to_dataframe()

# View top few rows of results
print(first_results.head())
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/b789dd6a-543b-4700-b5f9-108ee5b52a4d)
```python
# Query to select all columns where pollution levels are exactly 0
zero_pollution_query = """
                       SELECT *
                       FROM `bigquery-public-data.openaq.global_air_quality`
                       WHERE value = 0
                       """

# Set up the query
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
query_job = client.query(zero_pollution_query, job_config=safe_config)

# API request - run the query and return a pandas DataFrame
zero_pollution_results = query_job.to_dataframe() # Your code goes here

print(zero_pollution_results.head())
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/1efd0d56-bfcc-4232-972a-aff6b9b1783b)

# 3. Group By, Having & Count
> Get more interesting insights directly from your SQL queries 

## Count
![image](https://github.com/younlea/younlea.github.io/assets/1435846/e399f160-f994-4e69-8707-9ae760a7c31a)

## GROUP BY
![image](https://github.com/younlea/younlea.github.io/assets/1435846/7f2a5162-d74e-444a-957d-157e98840a1c)

## GROUP BY ... HAVING
![image](https://github.com/younlea/younlea.github.io/assets/1435846/69188cb6-716d-402d-b4e6-0a249c26fa99)

## 실습
```python
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()

# Construct a reference to the "hacker_news" dataset
dataset_ref = client.dataset("hacker_news", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)

# Construct a reference to the "comments" table
table_ref = dataset_ref.table("comments")

# API request - fetch the table
table = client.get_table(table_ref)

# Preview the first five lines of the "comments" table
client.list_rows(table, max_results=5).to_dataframe()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/bed5f384-b9f8-4f91-a21d-b00fefc4ef1f)

![image](https://github.com/younlea/younlea.github.io/assets/1435846/b9cbeae7-2bdd-4835-8ec7-ac52c1886210)
```python
# Query to select prolific commenters and post counts
prolific_commenters_query = """
        SELECT author, COUNT(1) AS NumPosts
        FROM `bigquery-public-data.hacker_news.comments`
        GROUP BY author
        HAVING COUNT(1) > 10000
        """

# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 1 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
query_job = client.query(prolific_commenters_query, job_config=safe_config)

# API request - run the query, and return a pandas DataFrame
prolific_commenters = query_job.to_dataframe()

# View top few rows of results
print(prolific_commenters.head())
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/8873fe15-933a-4c4f-8c40-6cb66df52e5e)

![image](https://github.com/younlea/younlea.github.io/assets/1435846/839b6184-2a20-430e-a93f-c3f1590f358a)
```python
# Query to determine how many posts were deleted
deleted_posts_query = """
                      SELECT COUNT(1) AS num_deleted_posts
                      FROM `bigquery-public-data.hacker_news.comments`
                      WHERE deleted = True
                      """

# Set up the query
query_job = client.query(deleted_posts_query)

# API request - run the query, and return a pandas DataFrame
deleted_posts = query_job.to_dataframe()

# View results
print(deleted_posts)
```

# 4. Order By
> Order your results to focus on the most important data for your use case.    

# 5. As & With
> Organize your query for better readability. This becomes especially important for complex queries.   

# 6. Joining Data
> Combine data sources. Critical for almost all real-world data problems   


