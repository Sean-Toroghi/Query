# Query platforms: BigQuery

Summary, notes, and code repository.


---

## Overview

BigQuery is a 
- serverless: no local overhead
- scalable: manage big data
- multi-cloud: secure and efficient
- data warehouse: 
service system. It is the public implementation of Dremel, an internal technogoy of Google. Dremol is a distributed system developed by Google to run interactive, ad-hoc queries over massive datasets.

### Datasets in BigQuery

There are three major forms of datasets in BigQuery: tables, views, and models.

Table structure comes in two formats: Row-wise and columair formats. Google has its own proprietary columnar sotrage format called _Capacitor_ format. It uses several compressing (encoding) techniques, and supports for nested and repeated fields[^1]. Also BigQuery supposts data partitioning and clustering, which not only reduces the costs, but also optimizes the performance for filtering and aggregating data.

### Data engineering and machine learning
BigQuery communicates with Spark, including `pyspark`.
Example:
```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("BigQuery with Spark").config("spark.jars.package").getOrCreate()

df = spark.read.format("bigiquery").option("table", "dataset.table_1").load()

```
BigQuery could also perform data quality check, including:
- count verification
- value range check
- null value check
- duplicate record check
- outlier check
- referential integrity check

---

## Data in Bigquery 

__Table in BigQuery__

BigQuery has three types of table:  
- native
- external
- views

__Data ingestion in BigQuery__

Two major types of data that can be used in BigQuery are batch data and stream data.
1. __Batch data ingestion__

  BigQuery supports multiple data formats, including CSV, JSON, Parquet, ORC, Avor, and Iceberg. It also supports data transfer from multiple sources, including google cloud, Oracle, Salesforce, google merchant center, and ServiceNow. BigQuery also supports federated query from sources such as cloud SQL and cloud Spanner.
  
  
  Example: load data. Steps for loading a data is as following:
  - Under the project, select "create a dataset"
  - Under the dataset, choose "create table"

2.  __Stream data__
  Some of the challenges w.r.t. stream data are:
  - late arrival, caused by window aggregation, state management, or data watermarks,
  - muliple time stamps, due to different event time, ingestion time, or processing time
  - how processing: exactly once, or at least once


---
## EDA with BigQuery

__Check NULL in features__
```sql
SELECT
  COUNTIF (feature_1 IS NULL) AS feature_1_null_counts
FROM
  'dataset_name.table_name'
```

__Check for duplicate record__
```sql
SELECT
  feature_1,
  COUNT(*) AS frequency
FROM
  'dataset_name.table_name'
GROUP BY
  feature_1
HAVING
  COUNT(*) > 1;
```

__Check for error in values (for this example we assume feature_1 cannot take a negative value)__
```sql
SELECT
  feature_1
FROM
  'dataset_name.table_name'
WHERE
  feature_1 < 0;
```

__Check for outliers__
```sql
SELECT
  feature_1
FROM
  'dataset_name.table_name'
WHERE
  feature_1 >= 1000000;
```
__Check for number of values for each category in feature_1 (similar to `value_count()` function)__
```sql
SELECT
  feature_1,
  COUNT(*) AS count_per_category
FROM
  'dataset_name.table_name'
GROUP BY
  feature_1
ORDER BY
  feature_1  
```

__Check for integrity (here we check if all values for feature_1 are available in both table_1 and table_2__
```sql
SELECT
  t1.feature_1,
  t2_feature_1
FROM
  'dataset_name.table_name_1' t1
LEFT JOIN
  'dataset_name.table_name_2' t1
ON
  t1.feature_1 = t2_feature_1
WHERE
  t2.feature_1 IS NULL
```

__Check if value is valid w/ regular expression (checking if all the values for feature_1 consists of two capital letters)__
```sql
SELECT
  feature_1
FROM
  'dataset_name.table_name'
WHERE
  LENGTH(feature_1) != 2 OR NOT REGEXP_CONTAINS(feature_1, r'^[A-Z]{2}'); -- check if all values in feature_1 are exactly 2 uppercase letters
```



---
## ML with BigQuery

Google implemented `bigframes`, which provides a platform similar to Pandas in its BigQuery product.

EXample - Perform the following tasks:
- Load data via bigquery pandas
- retrieve table schema, using google bigquery
- Generate statistic descriptives for numeric features
```python
import bigframes.pandas as bpd
from goolge.cloud import bigquery

# define table_id and load data
def load_data(project_id, dataset_name, table_name):
  '''
  Return google bigframe dataframe, given project id, dataset name, and table name:
    load_data(project_id, dataset_name, table_name)
  '''
  table_id = f"{project_id}.{dataset_name}.{table_name}"
  # load data into bigframe
  try:
    data_df = bpd.read_gbq(table_id)
    return data_df
  except Exception as e:
    print(f"Error in processing the {table_id}: {e}")
    return None
```

Example: retrieve table schema, using google bigquery
```python
import bigframes.pandas as bpd
from goolge.cloud import bigquery
def get_schema(project_id, dataset_name, table_name):
  table_id = pd
```


---
### Example - classification model

### Example - regression model

### Example: tune series forecsting model

---

__References__
[^1]: [Inside Capacitor, BigQuery’s next-generation columnar storage format, by Pasumansky - 2016](https://cloud.google.com/blog/products/bigquery/inside-capacitor-bigquerys-next-generation-columnar-storage-format). 


