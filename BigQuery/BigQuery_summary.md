# Query platforms: BigQuery

Summary, notes, and code repository.


---

## Overview

### Datasets

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

## Data ingestion 

Two major types of data that can be used in BigQuery are batch data and stream data.

### Batch data ingestion
BigQuery supports multiple data formats, including CSV, JSON, Parquet, ORC, Avor, and Iceberg. It also supports data transfer from multiple sources, including google cloud, Oracle, Salesforce, google merchant center, and ServiceNow. BigQuery also supports federated query from sources such as cloud SQL and cloud Spanner.


Example: load data. Steps for loading a data is as following:
- Under the project, select "create a dataset"
- Under the dataset, choose "create table"

### Stream data
Some of the challenges w.r.t. stream data are:
- late arrival, caused by window aggregation, state management, or data watermarks,
- muliple time stamps, due to different event time, ingestion time, or processing time
- how processing: exactly once, or at least once


---
## EDA with BigQuery


---
## ML with BigQuery

---
### Example - classification model

### Example - regression model

### Example: tune series forecsting model

---

__References__
[^1]: [Inside Capacitor, BigQuery’s next-generation columnar storage format, by Pasumansky - 2016](https://cloud.google.com/blog/products/bigquery/inside-capacitor-bigquerys-next-generation-columnar-storage-format). 


