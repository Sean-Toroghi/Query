# Query platforms: BigQuery

Summary, notes, and code repository.


---

## Overview

__Datasets__

There are three major forms of datasets in BigQuery: tables, views, and models.

__Tables__

Table structure comes in two formats: Row-wise and columair formats. Google has its own proprietary columnar sotrage format called _Capacitor_ format. It uses several compressing (encoding) techniques, and supports for nested and repeated fields[^1]. 



    
__References:__

[^1]: [Inside Capacitor, BigQuery’s next-generation columnar storage format, by Pasumansky - 2016](https://cloud.google.com/blog/products/bigquery/inside-capacitor-bigquerys-next-generation-columnar-storage-format). 
