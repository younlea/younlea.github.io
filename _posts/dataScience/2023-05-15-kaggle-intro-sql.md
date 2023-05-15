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
## Order By
![image](https://github.com/younlea/younlea.github.io/assets/1435846/f54a60a0-9e91-4467-87e0-1dad38e971c5)

## dates
![image](https://github.com/younlea/younlea.github.io/assets/1435846/5e11a108-73e0-45f6-bbf8-49abfdb8d5e1)

## EXTRACT
![image](https://github.com/younlea/younlea.github.io/assets/1435846/4ebf562c-a9b0-400c-aab4-e8d0785e75b8)

### sample
```python
# Query to find out the number of accidents for each day of the week
query = """
        SELECT COUNT(consecutive_number) AS num_accidents, 
               EXTRACT(DAYOFWEEK FROM timestamp_of_crash) AS day_of_week
        FROM `bigquery-public-data.nhtsa_traffic_fatalities.accident_2015`
        GROUP BY day_of_week
        ORDER BY num_accidents DESC
        """
# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 1 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**9)
query_job = client.query(query, job_config=safe_config)

# API request - run the query, and convert the results to a pandas DataFrame
accidents_by_day = query_job.to_dataframe()

# Print the DataFrame
accidents_by_day        
        
```

## 실습
```python
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()

# Construct a reference to the "world_bank_intl_education" dataset
dataset_ref = client.dataset("world_bank_intl_education", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)

# Construct a reference to the "international_education" table
table_ref = dataset_ref.table("international_education")

# API request - fetch the table
table = client.get_table(table_ref)

# Preview the first five lines of the "international_education" table
client.list_rows(table, max_results=5).to_dataframe()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/371ce7ae-40a1-4116-b0a0-b11cca8eda25)
![image](https://github.com/younlea/younlea.github.io/assets/1435846/c050d6eb-f283-4951-bb75-f8e09571ecc4)

```python
country_spend_pct_query = """
                          SELECT country_name, AVG(value) AS avg_ed_spending_pct
                          FROM `bigquery-public-data.world_bank_intl_education.international_education`
                          WHERE indicator_code = 'SE.XPD.TOTL.GD.ZS' and year >= 2010 and year <= 2017
                          GROUP BY country_name
                          ORDER BY avg_ed_spending_pct DESC
                          """

# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 1 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
country_spend_pct_query_job = client.query(country_spend_pct_query, job_config=safe_config)

# API request - run the query, and return a pandas DataFrame
country_spending_results = country_spend_pct_query_job.to_dataframe()

# View top few rows of results
print(country_spending_results.head())
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/cd3fe1b2-19df-4ddd-b5a6-8de963e297e0)

![image](https://github.com/younlea/younlea.github.io/assets/1435846/bf1f1b02-4ecb-4d5b-b958-bba35126c436)
```python
code_count_query = """
                   SELECT indicator_code, indicator_name, COUNT(1) AS num_rows
                   FROM `bigquery-public-data.world_bank_intl_education.international_education`
                   WHERE year = 2016
                   GROUP BY indicator_name, indicator_code
                   HAVING COUNT(1) >= 175
                   ORDER BY COUNT(1) DESC
                   """

# Set up the query
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
code_count_query_job = client.query(code_count_query, job_config=safe_config)

# API request - run the query, and return a pandas DataFrame
code_count_results = code_count_query_job.to_dataframe()

# View top few rows of results
print(code_count_results.head())
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/75fb49bc-9b00-4a66-9cec-148c2e75247f)

# 5. As & With
> Organize your query for better readability. This becomes especially important for complex queries.   
## AS
![image](https://github.com/younlea/younlea.github.io/assets/1435846/4a664f5b-8ba2-4c9f-8733-92cc4812fd39)

## WITH... AS
![image](https://github.com/younlea/younlea.github.io/assets/1435846/172a1448-250b-46ee-9923-5d32a7c2a125)

## example
```python
# Query to select the number of transactions per date, sorted by date
query_with_CTE = """ 
                 WITH time AS 
                 (
                     SELECT DATE(block_timestamp) AS trans_date
                     FROM `bigquery-public-data.crypto_bitcoin.transactions`
                 )
                 SELECT COUNT(1) AS transactions,
                        trans_date
                 FROM time
                 GROUP BY trans_date
                 ORDER BY trans_date
                 """

# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 10 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
query_job = client.query(query_with_CTE, job_config=safe_config)

# API request - run the query, and convert the results to a pandas DataFrame
transactions_by_date = query_job.to_dataframe()

# Print the first five rows
transactions_by_date.head()
```

## 실습
```python
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()

# Construct a reference to the "chicago_taxi_trips" dataset
dataset_ref = client.dataset("chicago_taxi_trips", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)
```

```python
# List all the tables in the dataset
tables = list(client.list_tables(dataset))

# Print names of all tables in the dataset (there is only one!)
for table in tables:  
    print(table.table_id)

table_name = 'taxi_trips'
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/26d5b9a9-1708-4308-afdc-d72155fdc04a)

```python
# Check your answer (Run this code cell to receive credit!)
# Construct a reference to the "taxi_trips" table
table_ref = dataset_ref.table("taxi_trips")

# API request - fetch the table
table = client.get_table(table_ref)

# Preview the first five lines of the "taxi_trips" table
client.list_rows(table, max_results=5).to_dataframe()
q_2.solution()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/a543674a-70d8-463f-a683-47ab8bc110e4)
```python
rides_per_year_query = """
                       SELECT EXTRACT(YEAR FROM trip_start_timestamp) AS year, 
                              COUNT(1) AS num_trips
                       FROM `bigquery-public-data.chicago_taxi_trips.taxi_trips`
                       GROUP BY year
                       ORDER BY year
                       """

# Set up the query (cancel the query if it would use too much of 
# your quota)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
rides_per_year_query_job = client.query(rides_per_year_query, job_config=safe_config)

# API request - run the query, and return a pandas DataFrame
rides_per_year_result = rides_per_year_query_job.to_dataframe()

# View results
print(rides_per_year_result)
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/5358b85c-bc70-48a3-bdc3-e9d28dd9f8b6)
```python
rides_per_month_query = """
                        SELECT EXTRACT(MONTH FROM trip_start_timestamp) AS month, 
                               COUNT(1) AS num_trips
                        FROM `bigquery-public-data.chicago_taxi_trips.taxi_trips`
                        WHERE EXTRACT(YEAR FROM trip_start_timestamp) = 2016
                        GROUP BY month
                        ORDER BY month
                        """

# Set up the query (cancel the query if it would use too much of 
# your quota)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
rides_per_month_query_job = client.query(rides_per_month_query, job_config=safe_config)

# API request - run the query, and return a pandas DataFrame
rides_per_month_result = rides_per_month_query_job.to_dataframe()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/e0356c62-a3d5-469b-bfed-e1d2bec5bda2)
```python
speeds_query = """
               WITH RelevantRides AS
               (
                   SELECT EXTRACT(HOUR FROM trip_start_timestamp) AS hour_of_day, 
                          trip_miles, 
                          trip_seconds
                   FROM `bigquery-public-data.chicago_taxi_trips.taxi_trips`
                   WHERE trip_start_timestamp > '2016-01-01' AND 
                         trip_start_timestamp < '2016-04-01' AND 
                         trip_seconds > 0 AND 
                         trip_miles > 0
               )
               SELECT hour_of_day, 
                      COUNT(1) AS num_trips, 
                      3600 * SUM(trip_miles) / SUM(trip_seconds) AS avg_mph
               FROM RelevantRides
               GROUP BY hour_of_day
               ORDER BY hour_of_day
               """

# Set up the query (cancel the query if it would use too much of 
# your quota)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
speeds_query_job = client.query(speeds_query, job_config=safe_config)

# API request - run the query, and return a pandas DataFrame
speeds_result = speeds_query_job.to_dataframe()
```

# 6. Joining Data
> Combine data sources. Critical for almost all real-world data problems   

## Example
![image](https://github.com/younlea/younlea.github.io/assets/1435846/607d3856-5f97-41c1-bf8f-96124b9f0435)
![image](https://github.com/younlea/younlea.github.io/assets/1435846/13c6f140-67f9-46e1-b468-aea4ce91364f)

### sample code
```python
# Query to determine the number of files per license, sorted by number of files
query = """
        SELECT L.license, COUNT(1) AS number_of_files
        FROM `bigquery-public-data.github_repos.sample_files` AS sf
        INNER JOIN `bigquery-public-data.github_repos.licenses` AS L 
            ON sf.repo_name = L.repo_name
        GROUP BY L.license
        ORDER BY number_of_files DESC
        """

# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 10 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
query_job = client.query(query, job_config=safe_config)

# API request - run the query, and convert the results to a pandas DataFrame
file_count_by_license = query_job.to_dataframe()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/21bb09c2-ed95-4e99-9557-4cdc366abad9)

## 실습  
```python
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()

# Construct a reference to the "stackoverflow" dataset
dataset_ref = client.dataset("stackoverflow", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/2b47c064-610f-4d23-a945-90cdb8456a65)
```python
# Get a list of available tables 
tables = list(client.list_tables(dataset))
list_of_tables = [table.table_id for table in tables]

# Print your answer
print(list_of_tables)
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/a3220c11-bb24-4dba-a9f1-f0ddcacc8f73)

```python
# Construct a reference to the "posts_answers" table
answers_table_ref = dataset_ref.table("posts_answers")
​
# API request - fetch the table
answers_table = client.get_table(answers_table_ref)
​
# Preview the first five lines of the "posts_answers" table
client.list_rows(answers_table, max_results=5).to_dataframe()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/a9a05dd2-51e8-43ab-9676-8a5cf8a40fcb)

```python
# Construct a reference to the "posts_questions" table
questions_table_ref = dataset_ref.table("posts_questions")

# API request - fetch the table
questions_table = client.get_table(questions_table_ref)

# Preview the first five lines of the "posts_questions" table
client.list_rows(questions_table, max_results=5).to_dataframe()
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/d63a713a-97bb-4bda-a64b-d38cf4a15bf5)

![image](https://github.com/younlea/younlea.github.io/assets/1435846/83a42e3d-5bee-4fdb-922e-2f97cb807e2f)
```python
questions_query = """
                  SELECT id, title, owner_user_id
                  FROM `bigquery-public-data.stackoverflow.posts_questions`
                  WHERE tags LIKE '%bigquery%'
                  """

# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 1 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
questions_query_job = client.query(questions_query, job_config=safe_config)

# API request - run the query, and return a pandas DataFrame
questions_results = questions_query_job.to_dataframe()

# Preview results
print(questions_results.head())
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/07597fe3-7000-4f1b-9115-653ddf04dd4b)
![image](https://github.com/younlea/younlea.github.io/assets/1435846/a3ff1fd2-2a0c-4e7d-8e69-f3e1a4dabbb8)
```python
answers_query = """
                SELECT a.id, a.body, a.owner_user_id
                FROM `bigquery-public-data.stackoverflow.posts_questions` AS q 
                INNER JOIN `bigquery-public-data.stackoverflow.posts_answers` AS a
                    ON q.id = a.parent_id
                WHERE q.tags LIKE '%bigquery%'
                """

# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 27 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=27*10**10)
answers_query_job = client.query(answers_query, job_config=safe_config)

# API request - run the query, and return a pandas DataFrame
answers_results = answers_query_job.to_dataframe()

# Preview results
print(answers_results.head())
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/2c389b6b-1580-46f8-afb1-6e4bb597f1eb)

![image](https://github.com/younlea/younlea.github.io/assets/1435846/5de696dc-f1cf-4992-8114-78702c6d129e)
```python
bigquery_experts_query = """
                         SELECT a.owner_user_id AS user_id, COUNT(1) AS number_of_answers
                         FROM `bigquery-public-data.stackoverflow.posts_questions` AS q
                         INNER JOIN `bigquery-public-data.stackoverflow.posts_answers` AS a
                             ON q.id = a.parent_Id
                         WHERE q.tags LIKE '%bigquery%'
                         GROUP BY a.owner_user_id
                         """

# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 1 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
bigquery_experts_query_job = client.query(bigquery_experts_query, job_config=safe_config)

# API request - run the query, and return a pandas DataFrame
bigquery_experts_results = bigquery_experts_query_job.to_dataframe()

# Preview results
print(bigquery_experts_results.head())
```
