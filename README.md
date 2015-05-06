# Real time ad-hoc querying on Big Data 

Here is an comparative study on real time ad hoc SQL like querying on Big data on scale. Below are the products from 'big'  big data players such as Cloudera, Hortonworks, MapR.

As the hadoop clusters are going large and large, traditional batch processing is inadequate for data analysis. Demand for SQL like ad hoc quering on big data sitting in large hadoop clusters is increasing all the time. 
Hive served up to an extend for analysing big data but it relay on traditional map-reduce approach. Set up time for map reduce jobs are costly so a faster approach is in demand. 

There are different solutions are in market right now. I found below products very promising and worth a try. 


##Drill Vs Impala Vs Stinger



| Item        | Drill           | Impala  | Stinger.next|
| ------------- |:-------------:| -----:|  ----------:|
| supported by      | MapR | Cloudera | Hortonworks    |
| Inspired by     | Google Dremel     |  Google Dremel |  Hive   |
|    purpose   |avoid ETL to an extend, low latency query |avoid ETL to an extend, low latency query   |avoid ETL to an extend , low latency query   | 
| Map Reduce      | No | No | on Tez   |
| Sopports SQL      | Yes | Yes  | Yes   |
| Works with      | hadoop and beyond | hadoop |  hadoop  |
|     BI connectivity  | BI/SQL tools such as Tableau, MicroStrategy, Pentaho and Jaspersoft using JDBC/ODBC drivers | JDBC/ODBC connectivity for most of the tools | JDBC/ODBC connectivity for most of the tools   |
|  Schema     | Self Describing Schema, No need for SerDe or attach structure | Schema is required | Schema is required    |
|   Connectivity with Hive    | Plays Well with Hive  | Plays Well with Hive (can use the hive metastore for Impala metadata) | Build for  Hive   |
|   UDFs and UADFs    |Java API to build custom functions (UDFs and UDAFs)  | Supports SQL built-in functions by writing user-defined functions (UDFs) in C++ or Java. |  supports all hive features  |
|   distributed execution environment    | Drill bits and Zookeeper | The Impala Daemon and Impala Statestore (health check, running in one node), Impala Catalog Service provides metadata changes (running in one node)   | LLAP, a process for multi-threaded execution, will work with Apache Tez to achieve sub-second response times   |
| user interfaces   | Drill shell (SQLLine),Drill Web UI, ODBC, JDBC, C++ API  | The impala-shell interactive command interpreter, The Apache Hue web-based user interface, JDBC, ODBC  | Current Hive UIs   |
|   Architecture    | Distributed, no master-slave, any node can get the query | Distributed, You can submit a query to the Impala daemon running on any node, and that node serves as the coordinator node for that query | Uses Hive with the help of ORC, Tez and a new query execution plans   |
|   Joins on different storage layers    | Yes, can join tables from HBASE, JSON, HIVE in a single query |  |    |
|   Works well with    | JSON, can directly query on JSON with out  creating structure  | Parquet file format | ORC   |
|   Support for S3 (AWS)  | Yes  | Yes | Natively no    |
|   Support for spark  | Yes  | No |  Yes  |

## A comparison study on performance on same data set. 

### 1. Small & Simple Data set : building.csv 20 rows and 5 coums. HVAC.csv : 8000 rows and 7 columns.

Join Query : 

select a.buildingid, b.buildingmgr, max(a.targettemp-a.actualtemp)
from hvac a join building b
on a.buildingid = b.buildingid
group by a.buildingid, b.buildingmgr;


| Item        | Drill           | Impala  | Stinger.next|
| ------------- |:-------------:| -----:|  ----------:|
| Environment      | MapR-Sandbox-For-Apache-Drill-0.8.0-4.1.0| Cloudera QuickStart VM for CDH 5.3| Sandbox HDP 2.2.4    |
|   Cores  | 2  | 2 |  2  |
|   Memory  | 8GB  | 8GB |  8GB  |
|     | Join: Time in seconds  | No |  Yes  |Time taken: 31.243 seconds, Fetched: 20 row(s) |
|   Same Query again : Time in seconds | Yes  | No |Time taken: 10.788 seconds, Fetched: 20 row(s)  |



Impala is an in-memory SQL-on-Hadoop project that supports interactive use cases. Its development was and is spearheaded by Cloudera. (Proponents claim that Spark's ability to persist data to disk gives it a distinct advantage over Impala, which has no provision for spilling over to disk if it runs out of physical memory.)





















MapR Drill Basics
 - Easy SQL query on HDFS
 - Can query semi structed data directly without ETL
 - Schema on Fly
 - Query on json and parque using std SQL
 - JDBC for tableau, Microstrategy, SAS
 - ODBC for Drill Explorer
 - Drill Explorer for analyze metadata for unstructed data
 - Drill Explorer will automatically create sql queries for the selected table/cloumn
How Drill identify the schema
 - Schema on fly
 - Select * from hive.orders  hive -> storage system type, orders -> table
 - No need to connect the data base before query
 - Drill explorer will connect to hive metastore and fetch the metadata and show to the user. User can explore
   the metadata before writitng the actual queries. (No coding required)
 - Query on HIVE, HBASE and JSON
 - Create view and save it in Drill Explorer
Key features
 - Extensibility
 - Full Structured Query Language
 - Nested data
 - unknown schemas
 - Not only for hadoop

Sandbox

```
arun:~ arun$ ssh mapr@localhost -p 2222
The authenticity of host '[localhost]:2222 ([127.0.0.1]:2222)' can't be established.
RSA key fingerprint is ce:e9:73:20:5c:f8:e5:50:c6:e2:d0:11:89:29:d0:ad.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[localhost]:2222' (RSA) to the list of known hosts.
Password: 
Welcome to your Mapr Demo virtual machine.
[mapr@maprdemo ~]$ sqlline
Drill log directory: /opt/mapr/drill/drill-0.8.0/logs
```

Things to remember

tableau -> Drill -> json (remove the etl process - no need for schema)

Drill on spark

#Impala
Impala, the open source MPP analytic database for Apache Hadoop, is now firmly entrenched in the Big Data mainstream



