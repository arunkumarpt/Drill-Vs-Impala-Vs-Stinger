# Drill-Vs-Impala-Vs-Stinger

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


