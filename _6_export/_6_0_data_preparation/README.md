## Prepare data to be exported

### Pre-Requisite

* **Create database in Hive**

~~~
[cloudera@quickstart ~]$ hive
Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j.properties
WARNING: Hive CLI is deprecated and migration to Beeline is recommended.

hive> show databases;
OK
default

hive> create database retail_db_sqoop_export;
OK

hive> show databases;
OK
default
retail_db_sqoop_export

hive> use retail_db_sqoop_export;
OK

hive> dfs -ls  /user/hive/warehouse;
Found 1 items
drwxrwxrwx   - cloudera supergroup          0 2018-03-25 05:24 /user/hive/warehouse/retail_db_sqoop_export.db
~~~

* **Import orders data from MySQL to retail_db_sqoop_export database of hive**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--hive-import \
--hive-database retail_db_sqoop_export \
--hive-table orders \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/25 05:52:25 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/25 05:52:25 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/25 05:52:25 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
18/03/25 05:52:25 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
18/03/25 05:52:25 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/25 05:52:25 INFO tool.CodeGenTool: Beginning code generation
18/03/25 05:52:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/25 05:52:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/25 05:52:25 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/62344db6e8644c5e45d0336fd6a01887/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/25 05:52:26 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/62344db6e8644c5e45d0336fd6a01887/orders.jar
18/03/25 05:52:26 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/25 05:52:26 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/25 05:52:26 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/25 05:52:26 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/25 05:52:26 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/25 05:52:26 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/25 05:52:27 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/25 05:52:27 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/25 05:52:27 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/25 05:52:28 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/25 05:52:28 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders`
18/03/25 05:52:28 INFO db.IntegerSplitter: Split size: 34441; Num splits: 2 from: 1 to: 68883
18/03/25 05:52:28 INFO mapreduce.JobSubmitter: number of splits:2
18/03/25 05:52:28 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0061
18/03/25 05:52:29 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0061
18/03/25 05:52:29 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0061/
18/03/25 05:52:29 INFO mapreduce.Job: Running job: job_1514521302404_0061
18/03/25 05:52:35 INFO mapreduce.Job: Job job_1514521302404_0061 running in uber mode : false
18/03/25 05:52:35 INFO mapreduce.Job:  map 0% reduce 0%
18/03/25 05:52:41 INFO mapreduce.Job:  map 100% reduce 0%
18/03/25 05:52:41 INFO mapreduce.Job: Job job_1514521302404_0061 completed successfully
18/03/25 05:52:41 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=303532
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=233
		HDFS: Number of bytes written=2999944
		HDFS: Number of read operations=8
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=4
	Job Counters 
		Launched map tasks=2
		Other local map tasks=2
		Total time spent by all maps in occupied slots (ms)=8296
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=8296
		Total vcore-milliseconds taken by all map tasks=8296
		Total megabyte-milliseconds taken by all map tasks=8495104
	Map-Reduce Framework
		Map input records=68883
		Map output records=68883
		Input split bytes=233
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=101
		CPU time spent (ms)=6240
		Physical memory (bytes) snapshot=570847232
		Virtual memory (bytes) snapshot=3150876672
		Total committed heap usage (bytes)=489684992
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=2999944
18/03/25 05:52:41 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 13.8004 seconds (212.2862 KB/sec)
18/03/25 05:52:41 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
18/03/25 05:52:41 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/25 05:52:41 WARN hive.TableDefWriter: Column order_date had to be cast to a less precise type in Hive
18/03/25 05:52:41 INFO hive.HiveImport: Loading uploaded data into Hive

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 1.252 seconds
Loading data to table retail_db_sqoop_export.orders
Table retail_db_sqoop_export.orders stats: [numFiles=2, totalSize=2999944]
OK
Time taken: 0.589 seconds
~~~

* **Import order_items data from MySQL to retail_db_sqoop_export database of hive**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--hive-import \
--hive-database retail_db_sqoop_export \
--hive-table order_items \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/25 05:56:13 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/25 05:56:13 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/25 05:56:13 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
18/03/25 05:56:13 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
18/03/25 05:56:13 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/25 05:56:13 INFO tool.CodeGenTool: Beginning code generation
18/03/25 05:56:13 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/25 05:56:13 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/25 05:56:13 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/dbbc5174ac864303f9fca301b1c4b958/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/25 05:56:14 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/dbbc5174ac864303f9fca301b1c4b958/order_items.jar
18/03/25 05:56:14 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/25 05:56:14 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/25 05:56:14 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/25 05:56:14 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/25 05:56:14 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/25 05:56:14 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/25 05:56:14 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/25 05:56:15 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/25 05:56:15 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/25 05:56:16 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/25 05:56:16 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/25 05:56:16 INFO db.IntegerSplitter: Split size: 86098; Num splits: 2 from: 1 to: 172198
18/03/25 05:56:16 INFO mapreduce.JobSubmitter: number of splits:2
18/03/25 05:56:16 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0062
18/03/25 05:56:16 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0062
18/03/25 05:56:16 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0062/
18/03/25 05:56:16 INFO mapreduce.Job: Running job: job_1514521302404_0062
18/03/25 05:56:21 INFO mapreduce.Job: Job job_1514521302404_0062 running in uber mode : false
18/03/25 05:56:21 INFO mapreduce.Job:  map 0% reduce 0%
18/03/25 05:56:28 INFO mapreduce.Job:  map 50% reduce 0%
18/03/25 05:56:29 INFO mapreduce.Job:  map 100% reduce 0%
18/03/25 05:56:29 INFO mapreduce.Job: Job job_1514521302404_0062 completed successfully
18/03/25 05:56:29 INFO mapreduce.Job: Counters: 31
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=303750
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=254
		HDFS: Number of bytes written=5408880
		HDFS: Number of read operations=8
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=4
	Job Counters 
		Killed map tasks=1
		Launched map tasks=2
		Other local map tasks=2
		Total time spent by all maps in occupied slots (ms)=7406
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=7406
		Total vcore-milliseconds taken by all map tasks=7406
		Total megabyte-milliseconds taken by all map tasks=7583744
	Map-Reduce Framework
		Map input records=172198
		Map output records=172198
		Input split bytes=254
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=92
		CPU time spent (ms)=6480
		Physical memory (bytes) snapshot=585302016
		Virtual memory (bytes) snapshot=3171360768
		Total committed heap usage (bytes)=497025024
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=5408880
18/03/25 05:56:29 INFO mapreduce.ImportJobBase: Transferred 5.1583 MB in 13.6888 seconds (385.8714 KB/sec)
18/03/25 05:56:29 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
18/03/25 05:56:29 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/25 05:56:29 INFO hive.HiveImport: Loading uploaded data into Hive

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 1.263 seconds
Loading data to table retail_db_sqoop_export.order_items
Table retail_db_sqoop_export.order_items stats: [numFiles=2, totalSize=5408880]
OK
Time taken: 0.568 seconds
~~~

* **Verify orders & order_items table in hive**

~~~
[cloudera@quickstart ~]$ hive

Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j.properties
WARNING: Hive CLI is deprecated and migration to Beeline is recommended.
hive> use retail_db_sqoop_export;
OK

hive> show tables;
OK
order_items
orders

hive> select * from orders limit 5;
OK
1	2013-07-25 00:00:00.0	11599	CLOSED
2	2013-07-25 00:00:00.0	256	PENDING_PAYMENT
3	2013-07-25 00:00:00.0	12111	COMPLETE
4	2013-07-25 00:00:00.0	8827	CLOSED
5	2013-07-25 00:00:00.0	11318	COMPLETE

hive> select * from order_items limit 5;
OK
1	1	957	1	299.98	299.98
2	2	1073	1	199.99	199.99
3	2	502	5	250.0	50.0
4	2	403	1	129.99	129.99
5	4	897	2	49.98	24.99
~~~

* **Create orders_daily_revenue table in hive using CTAS**

~~~
hive> SET hive.auto.convert.join=false;
~~~

~~~
hive> create table orders_daily_revenue as
select o.order_date, sum(oi.order_item_subtotal) revenue
from orders o join order_items oi on (o.order_id = oi.order_item_order_id)
where o.order_date like '2013-07%'
group by o.order_date;

Query ID = cloudera_20180325064646_cf079785-14fa-481c-99b6-89f7ffe7e402
Total jobs = 2
Launching Job 1 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514521302404_0067, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514521302404_0067/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514521302404_0067
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
2018-03-25 06:46:47,635 Stage-1 map = 0%,  reduce = 0%
2018-03-25 06:46:53,932 Stage-1 map = 50%,  reduce = 0%, Cumulative CPU 2.21 sec
2018-03-25 06:46:54,990 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 5.38 sec
2018-03-25 06:47:01,207 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 7.75 sec
MapReduce Total cumulative CPU time: 7 seconds 750 msec
Ended Job = job_1514521302404_0067
Launching Job 2 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514521302404_0068, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514521302404_0068/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514521302404_0068
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2018-03-25 06:47:06,722 Stage-2 map = 0%,  reduce = 0%
2018-03-25 06:47:12,931 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 0.99 sec
2018-03-25 06:47:18,146 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 2.21 sec
MapReduce Total cumulative CPU time: 2 seconds 210 msec
Ended Job = job_1514521302404_0068
Moving data to: hdfs://quickstart.cloudera:8020/user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue
Table retail_db_sqoop_export.orders_daily_revenue stats: [numFiles=1, numRows=7, totalSize=284, rawDataSize=277]
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 7.75 sec   HDFS Read: 8423765 HDFS Write: 425 SUCCESS
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 2.21 sec   HDFS Read: 5266 HDFS Write: 384 SUCCESS
Total MapReduce CPU Time Spent: 9 seconds 960 msec
OK
Time taken: 37.548 seconds
~~~

* **Verify orders_daily_revenue table in hive**

~~~
hive> select * from orders_daily_revenue;
OK
2013-07-25 00:00:00.0	68153.82999999997
2013-07-26 00:00:00.0	136520.1700000003
2013-07-27 00:00:00.0	101074.34000000014
2013-07-28 00:00:00.0	87123.08000000013
2013-07-29 00:00:00.0	137287.09000000032
2013-07-30 00:00:00.0	102745.62000000013
2013-07-31 00:00:00.0	131878.06000000006
~~~

* **Verify orders_daily_revenue data on HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue
-rwxrwxrwx   1 cloudera supergroup        284 2018-03-25 06:47 /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue/000000_0

[cloudera@quickstart ~]$ hadoop fs -cat /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue/000000_0;
2013-07-25 00:00:00.068153.82999999997
2013-07-26 00:00:00.0136520.1700000003
2013-07-27 00:00:00.0101074.34000000014
2013-07-28 00:00:00.087123.08000000013
2013-07-29 00:00:00.0137287.09000000032
2013-07-30 00:00:00.0102745.62000000013
2013-07-31 00:00:00.0131878.06000000006
~~~
