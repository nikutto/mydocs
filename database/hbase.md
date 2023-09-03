# HBase

HBase is key-value type NoSQL.

## Comparison

### Compared to RDBMS

If you think RDBMS doesn't reach enough performance objective, use HBase.

I think RDBMS can operate flexiable and common operations, for example, join tables.
So, it is good to use RDBMS if you don't suffered from performance issues.
Performance of RDBMS is based on query or schema.
But, with my experience on current MySQL (8.x), I think the following is rough measure to give up RDBMS.
- Beyond 10M records for one table
- Need beyond 10k records fetch for some online queries
- Reach 100-1000 QPS for not efficient query


### Compared to Hive

Use HBase if you want to fetch and insert data online.

Hive can handle SQL-like query language (HiveQL). So, you can use SQL to operate it.
But, Hive is column-orient database, and its format is ORC.
Even for simple fetch query like lookup data by id, hive need to search from large data.

### Compared to Redis

Use HBase when data is large enough.
Use Redis when data is not too large and very fast access is required. 
For example, store session variables.

HBase store data not only into memory, and but also into storage.
HBase uses HDFS as storage layer, so it is more scalable and durable.


## Architecture

### Basic feature

ref: https://hbase.apache.org/book.html#arch.overview

- Strongly consistent reads/wirtes: Not evantually consistent
- Use HDFS as storage layer
  - It is possible to run without HDFS. Use standalone mode.  
- Support Thrift/REST API and java client API.
- Need at least 5 instance for DataNodes and NameNode for production usage.

### Requirement

To run HBase server (and maybe Java API too),
you can use Java8 or Java11.
- See https://hbase.apache.org/book.html#basic.prerequisites.

Currently, JDK17 is not supported but there is a plan to support it.
See https://issues.apache.org/jira/browse/HBASE-26038.

### 