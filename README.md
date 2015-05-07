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

####Join Query : Hortonworks HDP
```
set hive.execution.engine=tez;

select a.buildingid, b.buildingmgr, max(a.targettemp)
from hvac a join building b
on a.buildingid = b.buildingid
group by a.buildingid, b.buildingmgr;
```

####Join Query : MapR-Sandbox-For-Apache-Drill

The .csv files directly loaded to hdfs.

```
Select a.columns[6], b.columns[1]  , MAX(a.columns[2]) 
from dfs.`/mapr/demo.mapr.com/data/HVAC.csv` a join  
dfs.`/mapr/demo.mapr.com/data/building.csv` b on a.columns[6] = b.columns[0]
group by a.columns[6], b.columns[1]

```
####Join Query : cloudera-quickstart-vm-5.4.0-0

```
CREATE EXTERNAL TABLE hvac
(
   Date1 string,
   Time1 string,
   TargetTemp int,
   ActualTemp int,
   System int,
   SystemAge int,
   BuildingID int
  
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
LOCATION '/user/cloudera/hvac/';

CREATE EXTERNAL TABLE building
(
 BuildingID int,
 BuildingMgr string,
 BuildingAge int,
 HVACproduct string,
 Country string
)

ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
LOCATION '/user/cloudera/building/';



select a.BuildingID, b.BuildingMgr, max(a.TargetTemp)
from hvac a join building b
on a.BuildingID = b.BuildingID
group by a.BuildingID, b.BuildingMgr;

```

Processor :  Intel core i5 2.6Ghz

| Item        | Drill           | Impala  | Stinger.next|
| ------------- |:-------------:| -----:|  ----------:|
| Environment      | MapR-Sandbox-For-Apache-Drill-0.8.0-4.1.0| cloudera-quickstart-vm-5.4.0-0| Sandbox HDP 2.2.4    |
|   Cores  | 2  | 2 |  2  |
|   Memory  | 8GB  | 8GB |  8GB  |
|    Join: Time in seconds  | Less than a second |  3-5 seconds  |70-90 seconds |
|   Same Query again : Time in seconds | Less than a second   | 3-5 seconds |30-35 seconds|  




