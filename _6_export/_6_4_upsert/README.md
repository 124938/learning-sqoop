## Examples - Export data (Using UPSERT)

### Pre-Requisite

* **Create orders_daily_revenue table under MySQL**

~~~
mysql> drop table if exists orders_daily_revenue;
Query OK, 0 rows affected (0.03 sec)

mysql> create table orders_daily_revenue(
 order_date varchar(100) primary key,
 revenue float 
);
Query OK, 0 rows affected (0.00 sec)

mysql> describe orders_daily_revenue;
+------------+--------------+------+-----+---------+-------+
| Field      | Type         | Null | Key | Default | Extra |
+------------+--------------+------+-----+---------+-------+
| order_date | varchar(100) | YES  |     | NULL    |       |
| revenue    | float        | YES  |     | NULL    |       |
+------------+--------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
~~~

### Need of UPSERT

* **Export orders_daily_revenue table from HDFS to RDBMS - First Time**

~~~
[cloudera@quickstart ~]$ sqoop-export \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--export-dir /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue \
--input-fields-terminated-by '\001' \
--table orders_daily_revenue \
--num-mappers 1

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/04/07 09:37:29 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/04/07 09:37:29 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/04/07 09:37:29 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/04/07 09:37:29 INFO tool.CodeGenTool: Beginning code generation
18/04/07 09:37:30 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 09:37:30 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 09:37:30 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/0f062123e358affc7c5f2250a1b0b3c6/orders_daily_revenue.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/04/07 09:37:31 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/0f062123e358affc7c5f2250a1b0b3c6/orders_daily_revenue.jar
18/04/07 09:37:31 INFO mapreduce.ExportJobBase: Beginning export of orders_daily_revenue
18/04/07 09:37:31 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/04/07 09:37:31 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/04/07 09:37:32 INFO Configuration.deprecation: mapred.reduce.tasks.speculative.execution is deprecated. Instead, use mapreduce.reduce.speculative
18/04/07 09:37:32 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 09:37:32 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/04/07 09:37:32 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/04/07 09:37:32 INFO input.FileInputFormat: Total input paths to process : 1
18/04/07 09:37:32 INFO input.FileInputFormat: Total input paths to process : 1
18/04/07 09:37:33 INFO mapreduce.JobSubmitter: number of splits:1
18/04/07 09:37:33 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 09:37:33 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0073
18/04/07 09:37:33 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0073
18/04/07 09:37:33 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0073/
18/04/07 09:37:33 INFO mapreduce.Job: Running job: job_1514521302404_0073
18/04/07 09:37:38 INFO mapreduce.Job: Job job_1514521302404_0073 running in uber mode : false
18/04/07 09:37:38 INFO mapreduce.Job:  map 0% reduce 0%
18/04/07 09:37:43 INFO mapreduce.Job:  map 100% reduce 0%
18/04/07 09:37:43 INFO mapreduce.Job: Job job_1514521302404_0073 completed successfully
18/04/07 09:37:43 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=151378
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=475
		HDFS: Number of bytes written=0
		HDFS: Number of read operations=4
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=0
	Job Counters 
		Launched map tasks=1
		Data-local map tasks=1
		Total time spent by all maps in occupied slots (ms)=2135
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=2135
		Total vcore-milliseconds taken by all map tasks=2135
		Total megabyte-milliseconds taken by all map tasks=2186240
	Map-Reduce Framework
		Map input records=7
		Map output records=7
		Input split bytes=188
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=21
		CPU time spent (ms)=610
		Physical memory (bytes) snapshot=206651392
		Virtual memory (bytes) snapshot=1575493632
		Total committed heap usage (bytes)=172490752
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
18/04/07 09:37:43 INFO mapreduce.ExportJobBase: Transferred 475 bytes in 11.5998 seconds (40.9491 bytes/sec)
18/04/07 09:37:43 INFO mapreduce.ExportJobBase: Exported 7 records.
~~~

* **Verify orders_daily_revenue table under MySQL**

~~~
mysql> select * from orders_daily_revenue;
+-----------------------+---------+
| order_date            | revenue |
+-----------------------+---------+
| 2013-07-25 00:00:00.0 | 68153.8 |
| 2013-07-26 00:00:00.0 |  136520 |
| 2013-07-27 00:00:00.0 |  101074 |
| 2013-07-28 00:00:00.0 | 87123.1 |
| 2013-07-29 00:00:00.0 |  137287 |
| 2013-07-30 00:00:00.0 |  102746 |
| 2013-07-31 00:00:00.0 |  131878 |
+-----------------------+---------+
7 rows in set (0.00 sec)
~~~

* **Export orders_daily_revenue table from HDFS to RDBMS - Second Time**

~~~
[cloudera@quickstart ~]$ sqoop-export \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--export-dir /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue \
--input-fields-terminated-by '\001' \
--table orders_daily_revenue \
--num-mappers 1

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/04/07 09:41:28 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/04/07 09:41:28 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/04/07 09:41:28 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/04/07 09:41:28 INFO tool.CodeGenTool: Beginning code generation
18/04/07 09:41:29 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 09:41:29 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 09:41:29 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/4997809ed5212e366566cd28463e92af/orders_daily_revenue.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/04/07 09:41:30 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/4997809ed5212e366566cd28463e92af/orders_daily_revenue.jar
18/04/07 09:41:30 INFO mapreduce.ExportJobBase: Beginning export of orders_daily_revenue
18/04/07 09:41:30 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/04/07 09:41:30 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/04/07 09:41:31 INFO Configuration.deprecation: mapred.reduce.tasks.speculative.execution is deprecated. Instead, use mapreduce.reduce.speculative
18/04/07 09:41:31 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 09:41:31 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/04/07 09:41:31 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/04/07 09:41:31 INFO input.FileInputFormat: Total input paths to process : 1
18/04/07 09:41:31 INFO input.FileInputFormat: Total input paths to process : 1
18/04/07 09:41:32 INFO mapreduce.JobSubmitter: number of splits:1
18/04/07 09:41:32 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 09:41:32 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0074
18/04/07 09:41:32 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0074
18/04/07 09:41:32 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0074/
18/04/07 09:41:32 INFO mapreduce.Job: Running job: job_1514521302404_0074
18/04/07 09:41:38 INFO mapreduce.Job: Job job_1514521302404_0074 running in uber mode : false
18/04/07 09:41:38 INFO mapreduce.Job:  map 0% reduce 0%
18/04/07 09:41:42 INFO mapreduce.Job:  map 100% reduce 0%
18/04/07 09:41:42 INFO mapreduce.Job: Job job_1514521302404_0074 failed with state FAILED due to: Task failed task_1514521302404_0074_m_000000
Job failed as tasks failed. failedMaps:1 failedReduces:0

18/04/07 09:41:42 INFO mapreduce.Job: Counters: 8
	Job Counters 
		Failed map tasks=1
		Launched map tasks=1
		Data-local map tasks=1
		Total time spent by all maps in occupied slots (ms)=2065
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=2065
		Total vcore-milliseconds taken by all map tasks=2065
		Total megabyte-milliseconds taken by all map tasks=2114560
18/04/07 09:41:42 WARN mapreduce.Counters: Group FileSystemCounters is deprecated. Use org.apache.hadoop.mapreduce.FileSystemCounter instead
18/04/07 09:41:42 INFO mapreduce.ExportJobBase: Transferred 0 bytes in 11.5655 seconds (0 bytes/sec)
18/04/07 09:41:42 WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
18/04/07 09:41:42 INFO mapreduce.ExportJobBase: Exported 0 records.
18/04/07 09:41:42 ERROR tool.ExportTool: Error during export: 
Export job failed!
	at org.apache.sqoop.mapreduce.ExportJobBase.runExport(ExportJobBase.java:439)
	at org.apache.sqoop.manager.SqlManager.exportTable(SqlManager.java:931)
	at org.apache.sqoop.tool.ExportTool.exportTable(ExportTool.java:80)
	at org.apache.sqoop.tool.ExportTool.run(ExportTool.java:99)
	at org.apache.sqoop.Sqoop.run(Sqoop.java:147)
	at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:70)
	at org.apache.sqoop.Sqoop.runSqoop(Sqoop.java:183)
	at org.apache.sqoop.Sqoop.runTool(Sqoop.java:234)
	at org.apache.sqoop.Sqoop.runTool(Sqoop.java:243)
	at org.apache.sqoop.Sqoop.main(Sqoop.java:252)
~~~

### Usage of --update-key & --update-mode

* **Important Notes:**
  * --update-key is used to update existing data
  * --update-mode supports following values:
    * updateonly (Default)
    * allowinsert

* **Insert additional data into hive table orders_daily_revenue**

~~~
hive> SET hive.auto.convert.join=false;
~~~

~~~
hive> insert into table orders_daily_revenue
select o.order_date, sum(oi.order_item_subtotal) revenue
from orders o join order_items oi on (o.order_id = oi.order_item_order_id)
where o.order_date like '2013-08%'
group by o.order_date;

Query ID = cloudera_20180407100202_43e59907-e063-45f1-8723-54ea81ac43fb
Total jobs = 2
Launching Job 1 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514521302404_0075, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514521302404_0075/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514521302404_0075
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
2018-04-07 10:02:13,475 Stage-1 map = 0%,  reduce = 0%
2018-04-07 10:02:20,836 Stage-1 map = 50%,  reduce = 0%, Cumulative CPU 3.24 sec
2018-04-07 10:02:21,895 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 6.42 sec
2018-04-07 10:02:27,074 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 8.86 sec
MapReduce Total cumulative CPU time: 8 seconds 860 msec
Ended Job = job_1514521302404_0075
Launching Job 2 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514521302404_0076, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514521302404_0076/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514521302404_0076
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2018-04-07 10:02:32,671 Stage-2 map = 0%,  reduce = 0%
2018-04-07 10:02:36,837 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 0.72 sec
2018-04-07 10:02:43,126 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 2.0 sec
MapReduce Total cumulative CPU time: 2 seconds 0 msec
Ended Job = job_1514521302404_0076
Loading data to table retail_db_sqoop_export.orders_daily_revenue
Table retail_db_sqoop_export.orders_daily_revenue stats: [numFiles=2, numRows=38, totalSize=1532, rawDataSize=1494]
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 8.86 sec   HDFS Read: 8423765 HDFS Write: 1553 SUCCESS
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 2.0 sec   HDFS Read: 7047 HDFS Write: 1349 SUCCESS
Total MapReduce CPU Time Spent: 10 seconds 860 msec
OK
Time taken: 36.358 seconds
~~~    

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
2013-08-01 00:00:00.0	129001.62000000029
2013-08-02 00:00:00.0	109347.00000000013
2013-08-03 00:00:00.0	95266.89000000022
2013-08-04 00:00:00.0	90930.98000000019
2013-08-05 00:00:00.0	75882.31000000008
2013-08-06 00:00:00.0	120573.66000000034
2013-08-07 00:00:00.0	103351.3900000002
2013-08-08 00:00:00.0	76501.66000000006
2013-08-09 00:00:00.0	62316.47000000007
2013-08-10 00:00:00.0	129574.8000000002
2013-08-11 00:00:00.0	71149.59000000014
2013-08-12 00:00:00.0	121883.33000000034
2013-08-13 00:00:00.0	39874.520000000004
2013-08-14 00:00:00.0	103939.26000000021
2013-08-15 00:00:00.0	103030.53000000019
2013-08-16 00:00:00.0	69264.50000000003
2013-08-17 00:00:00.0	127361.93000000015
2013-08-18 00:00:00.0	106788.6500000001
2013-08-19 00:00:00.0	50348.27999999999
2013-08-20 00:00:00.0	87983.11000000018
2013-08-21 00:00:00.0	64860.810000000005
2013-08-22 00:00:00.0	94574.71000000027
2013-08-23 00:00:00.0	99616.1700000002
2013-08-24 00:00:00.0	128883.8200000003
2013-08-25 00:00:00.0	98521.56000000014
2013-08-26 00:00:00.0	88114.22000000015
2013-08-27 00:00:00.0	91634.92000000013
2013-08-28 00:00:00.0	55189.22000000001
2013-08-29 00:00:00.0	99960.57000000025
2013-08-30 00:00:00.0	57008.50000000003
2013-08-31 00:00:00.0	75923.72000000009
Time taken: 0.079 seconds, Fetched: 38 row(s)
~~~

* **Update existing data of orders_daily_revenue table under MySQL**

~~~
mysql> select * from orders_daily_revenue;
+-----------------------+---------+
| order_date            | revenue |
+-----------------------+---------+
| 2013-07-25 00:00:00.0 | 68153.8 |
| 2013-07-26 00:00:00.0 |  136520 |
| 2013-07-27 00:00:00.0 |  101074 |
| 2013-07-28 00:00:00.0 | 87123.1 |
| 2013-07-29 00:00:00.0 |  137287 |
| 2013-07-30 00:00:00.0 |  102746 |
| 2013-07-31 00:00:00.0 |  131878 |
+-----------------------+---------+
7 rows in set (0.00 sec)

mysql> update orders_daily_revenue set revenue = 0.0;
Query OK, 7 rows affected (0.01 sec)
Rows matched: 7  Changed: 7  Warnings: 0

mysql> select * from orders_daily_revenue;
+-----------------------+---------+
| order_date            | revenue |
+-----------------------+---------+
| 2013-07-25 00:00:00.0 |       0 |
| 2013-07-26 00:00:00.0 |       0 |
| 2013-07-27 00:00:00.0 |       0 |
| 2013-07-28 00:00:00.0 |       0 |
| 2013-07-29 00:00:00.0 |       0 |
| 2013-07-30 00:00:00.0 |       0 |
| 2013-07-31 00:00:00.0 |       0 |
+-----------------------+---------+
7 rows in set (0.00 sec)
~~~

* **(1) : Export orders_daily_revenue table from HDFS to RDBMS (Using default update mode i.e. updateonly)**

~~~
[cloudera@quickstart ~]$ sqoop-export \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--export-dir /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue \
--input-fields-terminated-by '\001' \
--table orders_daily_revenue \
--update-key order_date \
--num-mappers 1

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/04/07 10:09:45 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/04/07 10:09:45 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/04/07 10:09:45 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/04/07 10:09:46 INFO tool.CodeGenTool: Beginning code generation
18/04/07 10:09:46 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 10:09:46 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 10:09:46 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/bf602833df6ac758280d685fcdf9077c/orders_daily_revenue.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/04/07 10:09:47 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/bf602833df6ac758280d685fcdf9077c/orders_daily_revenue.jar
18/04/07 10:09:47 INFO mapreduce.ExportJobBase: Beginning export of orders_daily_revenue
18/04/07 10:09:47 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/04/07 10:09:47 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/04/07 10:09:48 INFO Configuration.deprecation: mapred.reduce.tasks.speculative.execution is deprecated. Instead, use mapreduce.reduce.speculative
18/04/07 10:09:48 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 10:09:48 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/04/07 10:09:48 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/04/07 10:09:49 INFO input.FileInputFormat: Total input paths to process : 2
18/04/07 10:09:49 INFO input.FileInputFormat: Total input paths to process : 2
18/04/07 10:09:50 INFO mapreduce.JobSubmitter: number of splits:1
18/04/07 10:09:50 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 10:09:50 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0077
18/04/07 10:09:50 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0077
18/04/07 10:09:50 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0077/
18/04/07 10:09:50 INFO mapreduce.Job: Running job: job_1514521302404_0077
18/04/07 10:09:55 INFO mapreduce.Job: Job job_1514521302404_0077 running in uber mode : false
18/04/07 10:09:55 INFO mapreduce.Job:  map 0% reduce 0%
18/04/07 10:10:00 INFO mapreduce.Job:  map 100% reduce 0%
18/04/07 10:10:00 INFO mapreduce.Job: Job job_1514521302404_0077 completed successfully
18/04/07 10:10:00 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=151659
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=1857
		HDFS: Number of bytes written=0
		HDFS: Number of read operations=7
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=0
	Job Counters 
		Launched map tasks=1
		Data-local map tasks=1
		Total time spent by all maps in occupied slots (ms)=2164
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=2164
		Total vcore-milliseconds taken by all map tasks=2164
		Total megabyte-milliseconds taken by all map tasks=2215936
	Map-Reduce Framework
		Map input records=38
		Map output records=38
		Input split bytes=319
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=15
		CPU time spent (ms)=570
		Physical memory (bytes) snapshot=210366464
		Virtual memory (bytes) snapshot=1574699008
		Total committed heap usage (bytes)=244842496
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
18/04/07 10:10:00 INFO mapreduce.ExportJobBase: Transferred 1.8135 KB in 12.4602 seconds (149.0349 bytes/sec)
18/04/07 10:10:00 INFO mapreduce.ExportJobBase: Exported 38 records.
~~~

* **Verify orders_daily_revenue table under MySQL**

~~~
mysql> select * from orders_daily_revenue;
+-----------------------+---------+
| order_date            | revenue |
+-----------------------+---------+
| 2013-07-25 00:00:00.0 | 68153.8 |
| 2013-07-26 00:00:00.0 |  136520 |
| 2013-07-27 00:00:00.0 |  101074 |
| 2013-07-28 00:00:00.0 | 87123.1 |
| 2013-07-29 00:00:00.0 |  137287 |
| 2013-07-30 00:00:00.0 |  102746 |
| 2013-07-31 00:00:00.0 |  131878 |
+-----------------------+---------+
7 rows in set (0.00 sec)
~~~

* **Update existing data of orders_daily_revenue table under MySQL**

~~~
mysql> select * from orders_daily_revenue;
+-----------------------+---------+
| order_date            | revenue |
+-----------------------+---------+
| 2013-07-25 00:00:00.0 | 68153.8 |
| 2013-07-26 00:00:00.0 |  136520 |
| 2013-07-27 00:00:00.0 |  101074 |
| 2013-07-28 00:00:00.0 | 87123.1 |
| 2013-07-29 00:00:00.0 |  137287 |
| 2013-07-30 00:00:00.0 |  102746 |
| 2013-07-31 00:00:00.0 |  131878 |
+-----------------------+---------+
7 rows in set (0.00 sec)

mysql> update orders_daily_revenue set revenue = 0.0;
Query OK, 7 rows affected (0.01 sec)
Rows matched: 7  Changed: 7  Warnings: 0

mysql> select * from orders_daily_revenue;
+-----------------------+---------+
| order_date            | revenue |
+-----------------------+---------+
| 2013-07-25 00:00:00.0 |       0 |
| 2013-07-26 00:00:00.0 |       0 |
| 2013-07-27 00:00:00.0 |       0 |
| 2013-07-28 00:00:00.0 |       0 |
| 2013-07-29 00:00:00.0 |       0 |
| 2013-07-30 00:00:00.0 |       0 |
| 2013-07-31 00:00:00.0 |       0 |
+-----------------------+---------+
7 rows in set (0.00 sec)
~~~

* **(2) : Export orders_daily_revenue table from HDFS to RDBMS (Using allowinsert update mode)**

~~~
[cloudera@quickstart ~]$ sqoop-export \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--export-dir /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue \
--input-fields-terminated-by '\001' \
--table orders_daily_revenue \
--update-key order_date \
--update-mode allowinsert \
--num-mappers 1

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/04/07 10:14:51 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/04/07 10:14:51 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/04/07 10:14:51 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/04/07 10:14:51 INFO tool.CodeGenTool: Beginning code generation
18/04/07 10:14:52 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 10:14:52 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 10:14:52 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/8057378196f83af7c6efbfcec85ee8e2/orders_daily_revenue.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/04/07 10:14:53 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/8057378196f83af7c6efbfcec85ee8e2/orders_daily_revenue.jar
18/04/07 10:14:53 WARN manager.MySQLManager: MySQL Connector upsert functionality is using INSERT ON
18/04/07 10:14:53 WARN manager.MySQLManager: DUPLICATE KEY UPDATE clause that relies on table's unique key.
18/04/07 10:14:53 WARN manager.MySQLManager: Insert/update distinction is therefore independent on column
18/04/07 10:14:53 WARN manager.MySQLManager: names specified in --update-key parameter. Please see MySQL
18/04/07 10:14:53 WARN manager.MySQLManager: documentation for additional limitations.
18/04/07 10:14:53 INFO mapreduce.ExportJobBase: Beginning export of orders_daily_revenue
18/04/07 10:14:53 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/04/07 10:14:53 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/04/07 10:14:54 INFO Configuration.deprecation: mapred.reduce.tasks.speculative.execution is deprecated. Instead, use mapreduce.reduce.speculative
18/04/07 10:14:54 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 10:14:54 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/04/07 10:14:54 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/04/07 10:14:54 INFO input.FileInputFormat: Total input paths to process : 2
18/04/07 10:14:54 INFO input.FileInputFormat: Total input paths to process : 2
18/04/07 10:14:55 INFO mapreduce.JobSubmitter: number of splits:1
18/04/07 10:14:55 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 10:14:55 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0078
18/04/07 10:14:55 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0078
18/04/07 10:14:55 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0078/
18/04/07 10:14:55 INFO mapreduce.Job: Running job: job_1514521302404_0078
18/04/07 10:15:00 INFO mapreduce.Job: Job job_1514521302404_0078 running in uber mode : false
18/04/07 10:15:00 INFO mapreduce.Job:  map 0% reduce 0%
18/04/07 10:15:04 INFO mapreduce.Job:  map 100% reduce 0%
18/04/07 10:15:05 INFO mapreduce.Job: Job job_1514521302404_0078 completed successfully
18/04/07 10:15:05 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=151684
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=1857
		HDFS: Number of bytes written=0
		HDFS: Number of read operations=7
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=0
	Job Counters 
		Launched map tasks=1
		Data-local map tasks=1
		Total time spent by all maps in occupied slots (ms)=2185
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=2185
		Total vcore-milliseconds taken by all map tasks=2185
		Total megabyte-milliseconds taken by all map tasks=2237440
	Map-Reduce Framework
		Map input records=38
		Map output records=38
		Input split bytes=319
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=22
		CPU time spent (ms)=610
		Physical memory (bytes) snapshot=204595200
		Virtual memory (bytes) snapshot=1576263680
		Total committed heap usage (bytes)=172490752
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
18/04/07 10:15:05 INFO mapreduce.ExportJobBase: Transferred 1.8135 KB in 11.6147 seconds (159.8837 bytes/sec)
18/04/07 10:15:05 INFO mapreduce.ExportJobBase: Exported 38 records.
~~~

* **Verify orders_daily_revenue table under MySQL**

~~~
mysql> select * from orders_daily_revenue;
+-----------------------+---------+
| order_date            | revenue |
+-----------------------+---------+
| 2013-07-25 00:00:00.0 | 68153.8 |
| 2013-07-26 00:00:00.0 |  136520 |
| 2013-07-27 00:00:00.0 |  101074 |
| 2013-07-28 00:00:00.0 | 87123.1 |
| 2013-07-29 00:00:00.0 |  137287 |
| 2013-07-30 00:00:00.0 |  102746 |
| 2013-07-31 00:00:00.0 |  131878 |
| 2013-08-01 00:00:00.0 |  129002 |
| 2013-08-02 00:00:00.0 |  109347 |
| 2013-08-03 00:00:00.0 | 95266.9 |
| 2013-08-04 00:00:00.0 |   90931 |
| 2013-08-05 00:00:00.0 | 75882.3 |
| 2013-08-06 00:00:00.0 |  120574 |
| 2013-08-07 00:00:00.0 |  103351 |
| 2013-08-08 00:00:00.0 | 76501.7 |
| 2013-08-09 00:00:00.0 | 62316.5 |
| 2013-08-10 00:00:00.0 |  129575 |
| 2013-08-11 00:00:00.0 | 71149.6 |
| 2013-08-12 00:00:00.0 |  121883 |
| 2013-08-13 00:00:00.0 | 39874.5 |
| 2013-08-14 00:00:00.0 |  103939 |
| 2013-08-15 00:00:00.0 |  103031 |
| 2013-08-16 00:00:00.0 | 69264.5 |
| 2013-08-17 00:00:00.0 |  127362 |
| 2013-08-18 00:00:00.0 |  106789 |
| 2013-08-19 00:00:00.0 | 50348.3 |
| 2013-08-20 00:00:00.0 | 87983.1 |
| 2013-08-21 00:00:00.0 | 64860.8 |
| 2013-08-22 00:00:00.0 | 94574.7 |
| 2013-08-23 00:00:00.0 | 99616.2 |
| 2013-08-24 00:00:00.0 |  128884 |
| 2013-08-25 00:00:00.0 | 98521.6 |
| 2013-08-26 00:00:00.0 | 88114.2 |
| 2013-08-27 00:00:00.0 | 91634.9 |
| 2013-08-28 00:00:00.0 | 55189.2 |
| 2013-08-29 00:00:00.0 | 99960.6 |
| 2013-08-30 00:00:00.0 | 57008.5 |
| 2013-08-31 00:00:00.0 | 75923.7 |
+-----------------------+---------+
38 rows in set (0.00 sec)
~~~
