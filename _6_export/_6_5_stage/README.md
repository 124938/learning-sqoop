## Examples - Export data (Using Staging)

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

### Issue with table contains primary key with --num-mappers > 1 - Need for staging table

* **Populate orders_daily_revenue table under Hive**

~~~
hive> truncate table orders_daily_revenue;
~~~

~~~
hive> SET hive.auto.convert.join=false;
~~~

~~~
hive> insert into orders_daily_revenue 
select o.order_date, sum(oi.order_item_subtotal) revenue
from orders o join order_items oi on (o.order_id = oi.order_item_order_id)
group by o.order_date; 

Query ID = cloudera_20180407222929_f1ba6298-fd76-4051-99ec-a6d98c9a7fca
Total jobs = 2
Launching Job 1 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514521302404_0079, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514521302404_0079/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514521302404_0079
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
2018-04-07 22:29:20,717 Stage-1 map = 0%,  reduce = 0%
2018-04-07 22:29:27,045 Stage-1 map = 50%,  reduce = 0%, Cumulative CPU 3.14 sec
2018-04-07 22:29:28,107 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 6.24 sec
2018-04-07 22:29:33,322 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 8.85 sec
MapReduce Total cumulative CPU time: 8 seconds 850 msec
Ended Job = job_1514521302404_0079
Launching Job 2 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514521302404_0080, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514521302404_0080/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514521302404_0080
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2018-04-07 22:29:38,933 Stage-2 map = 0%,  reduce = 0%
2018-04-07 22:29:43,112 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 0.77 sec
2018-04-07 22:29:48,287 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 2.01 sec
MapReduce Total cumulative CPU time: 2 seconds 10 msec
Ended Job = job_1514521302404_0080
Loading data to table retail_db_sqoop_export.orders_daily_revenue
Table retail_db_sqoop_export.orders_daily_revenue stats: [numFiles=1, numRows=364, totalSize=14621, rawDataSize=14257]
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 8.85 sec   HDFS Read: 8422978 HDFS Write: 17364 SUCCESS
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 2.01 sec   HDFS Read: 22850 HDFS Write: 14724 SUCCESS
Total MapReduce CPU Time Spent: 10 seconds 860 msec
OK
Time taken: 34.406 seconds
~~~

~~~
hive> select count(*) from orders_daily_revenue;

Query ID = cloudera_20180407224040_ef2e7edc-8186-4eec-a058-fd3dedefba6b
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514521302404_0082, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514521302404_0082/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514521302404_0082
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2018-04-07 22:40:32,196 Stage-1 map = 0%,  reduce = 0%
2018-04-07 22:40:37,472 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.09 sec
2018-04-07 22:40:43,780 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 2.26 sec
MapReduce Total cumulative CPU time: 2 seconds 260 msec
Ended Job = job_1514521302404_0082
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 2.26 sec   HDFS Read: 21552 HDFS Write: 4 SUCCESS
Total MapReduce CPU Time Spent: 2 seconds 260 msec
OK
364
Time taken: 18.37 seconds, Fetched: 1 row(s)
~~~

* **Insert new record in orders_daily_revenue table manually under MySQL**

~~~
mysql> insert into orders_daily_revenue values("2013-08-06 00:00:00.0", 0);
Query OK, 1 row affected (0.01 sec)

mysql> select * from orders_daily_revenue;
+-----------------------+---------+
| order_date            | revenue |
+-----------------------+---------+
| 2013-08-06 00:00:00.0 |       0 |
+-----------------------+---------+
1 row in set (0.00 sec)
~~~

* **Export orders_daily_revenue table from HDFS to RDBMS with --num-mappers > 1**

~~~
[cloudera@quickstart ~]$ sqoop-export \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--export-dir /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue \
--input-fields-terminated-by '\001' \
--table orders_daily_revenue \
--num-mappers 4

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/04/07 22:37:22 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/04/07 22:37:22 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/04/07 22:37:22 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/04/07 22:37:22 INFO tool.CodeGenTool: Beginning code generation
18/04/07 22:37:23 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 22:37:23 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 22:37:23 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/4a110e99b6df08d0afed93c07a4212ce/orders_daily_revenue.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/04/07 22:37:24 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/4a110e99b6df08d0afed93c07a4212ce/orders_daily_revenue.jar
18/04/07 22:37:24 INFO mapreduce.ExportJobBase: Beginning export of orders_daily_revenue
18/04/07 22:37:24 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/04/07 22:37:24 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/04/07 22:37:25 INFO Configuration.deprecation: mapred.reduce.tasks.speculative.execution is deprecated. Instead, use mapreduce.reduce.speculative
18/04/07 22:37:25 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 22:37:25 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/04/07 22:37:25 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/04/07 22:37:26 INFO input.FileInputFormat: Total input paths to process : 1
18/04/07 22:37:26 INFO input.FileInputFormat: Total input paths to process : 1
18/04/07 22:37:26 INFO mapreduce.JobSubmitter: number of splits:4
18/04/07 22:37:26 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 22:37:26 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0081
18/04/07 22:37:27 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0081
18/04/07 22:37:27 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0081/
18/04/07 22:37:27 INFO mapreduce.Job: Running job: job_1514521302404_0081
18/04/07 22:37:32 INFO mapreduce.Job: Job job_1514521302404_0081 running in uber mode : false
18/04/07 22:37:32 INFO mapreduce.Job:  map 0% reduce 0%
18/04/07 22:37:37 INFO mapreduce.Job:  map 25% reduce 0%
18/04/07 22:37:38 INFO mapreduce.Job:  map 100% reduce 0%
18/04/07 22:37:38 INFO mapreduce.Job: Job job_1514521302404_0081 failed with state FAILED due to: Task failed task_1514521302404_0081_m_000001
Job failed as tasks failed. failedMaps:1 failedReduces:0

18/04/07 22:37:38 INFO mapreduce.Job: Counters: 32
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=151378
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=5802
		HDFS: Number of bytes written=0
		HDFS: Number of read operations=7
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=0
	Job Counters 
		Failed map tasks=1
		Killed map tasks=2
		Launched map tasks=4
		Data-local map tasks=4
		Total time spent by all maps in occupied slots (ms)=8605
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=8605
		Total vcore-milliseconds taken by all map tasks=8605
		Total megabyte-milliseconds taken by all map tasks=8811520
	Map-Reduce Framework
		Map input records=90
		Map output records=90
		Input split bytes=312
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=28
		CPU time spent (ms)=630
		Physical memory (bytes) snapshot=202506240
		Virtual memory (bytes) snapshot=1576382464
		Total committed heap usage (bytes)=172490752
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
18/04/07 22:37:38 INFO mapreduce.ExportJobBase: Transferred 5.666 KB in 13.0475 seconds (444.6839 bytes/sec)
18/04/07 22:37:38 INFO mapreduce.ExportJobBase: Exported 90 records.
18/04/07 22:37:38 ERROR tool.ExportTool: Error during export: 
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

* **Verify orders_daily_revenue table under MySQL**

~~~
mysql> select count(*) from orders_daily_revenue;

+----------+
| count(*) |
+----------+
|       91 |
+----------+
1 row in set (0.00 sec)
~~~

* **Result:**
  * Only few records are exported from HDFS to RDBMS i.e. records are exported partially which means consistency is not getting maintained 

### Usage of --staging-table & --clear-staging-table - Fail Run by maintaining data consistency

* **Important Notes:**
  * --staging-table  attributes comes into picture to maintain data consistency i.e. everything should be exported or nothing should be exported
  * Following steps are getting performed in case of using --staging-table  attribute:
    * Step-1 : Data will get removed from staging table in case of --clear-staging-table  is present in command
    * Step-2 : Data will get exported to stage table
    * Step-3 : Data will get copy from stage table to final table
    * Step-4 : Data will get removed from staging table automatically if command get executed successfully else data will remian in staging table for debugging purpose

* **Create orders_daily_revenue_stage table under MySQL**

~~~
mysql> drop table if exists orders_daily_revenue_stage;
Query OK, 0 rows affected (0.00 sec)

mysql> create table orders_daily_revenue_stage(
 order_date varchar(100),
 revenue float 
);
Query OK, 0 rows affected (0.00 sec)

mysql> describe orders_daily_revenue_stage;
+------------+--------------+------+-----+---------+-------+
| Field      | Type         | Null | Key | Default | Extra |
+------------+--------------+------+-----+---------+-------+
| order_date | varchar(100) | YES  |     | NULL    |       |
| revenue    | float        | YES  |     | NULL    |       |
+------------+--------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
~~~

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
| order_date | varchar(100) | NO   | PRI | NULL    |       |
| revenue    | float        | YES  |     | NULL    |       |
+------------+--------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
~~~

* **Insert new record in orders_daily_revenue table manually under MySQL**

~~~
mysql> insert into orders_daily_revenue values("2013-08-06 00:00:00.0", 0);
Query OK, 1 row affected (0.01 sec)

mysql> select * from orders_daily_revenue;
+-----------------------+---------+
| order_date            | revenue |
+-----------------------+---------+
| 2013-08-06 00:00:00.0 |       0 |
+-----------------------+---------+
1 row in set (0.00 sec)
~~~

* **Export orders_daily_revenue table from HDFS to RDBMS using --num-mappers > 1**

~~~
[cloudera@quickstart ~]$ sqoop-export \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--export-dir /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue \
--input-fields-terminated-by '\001' \
--staging-table orders_daily_revenue_stage \
--clear-staging-table \
--table orders_daily_revenue \
--num-mappers 4

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/04/07 23:15:49 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/04/07 23:15:49 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/04/07 23:15:49 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/04/07 23:15:49 INFO tool.CodeGenTool: Beginning code generation
18/04/07 23:15:49 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 23:15:50 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 23:15:50 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/7d4ba5f787b5978b40c7f5e146064efb/orders_daily_revenue.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/04/07 23:15:51 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/7d4ba5f787b5978b40c7f5e146064efb/orders_daily_revenue.jar
18/04/07 23:15:51 INFO mapreduce.ExportJobBase: Data will be staged in the table: orders_daily_revenue_stage
18/04/07 23:15:51 INFO mapreduce.ExportJobBase: Beginning export of orders_daily_revenue
18/04/07 23:15:51 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/04/07 23:15:51 INFO manager.SqlManager: Deleted 0 records from `orders_daily_revenue_stage`
18/04/07 23:15:51 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/04/07 23:15:51 INFO Configuration.deprecation: mapred.reduce.tasks.speculative.execution is deprecated. Instead, use mapreduce.reduce.speculative
18/04/07 23:15:51 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 23:15:51 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/04/07 23:15:52 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/04/07 23:15:52 INFO input.FileInputFormat: Total input paths to process : 1
18/04/07 23:15:52 INFO input.FileInputFormat: Total input paths to process : 1
18/04/07 23:15:52 INFO mapreduce.JobSubmitter: number of splits:4
18/04/07 23:15:52 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 23:15:53 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0084
18/04/07 23:15:53 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0084
18/04/07 23:15:53 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0084/
18/04/07 23:15:53 INFO mapreduce.Job: Running job: job_1514521302404_0084
18/04/07 23:15:58 INFO mapreduce.Job: Job job_1514521302404_0084 running in uber mode : false
18/04/07 23:15:58 INFO mapreduce.Job:  map 0% reduce 0%
18/04/07 23:16:03 INFO mapreduce.Job:  map 25% reduce 0%
18/04/07 23:16:04 INFO mapreduce.Job:  map 50% reduce 0%
18/04/07 23:16:05 INFO mapreduce.Job:  map 75% reduce 0%
18/04/07 23:16:06 INFO mapreduce.Job:  map 100% reduce 0%
18/04/07 23:16:06 INFO mapreduce.Job: Job job_1514521302404_0084 completed successfully
18/04/07 23:16:06 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=606192
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=29188
		HDFS: Number of bytes written=0
		HDFS: Number of read operations=19
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=0
	Job Counters 
		Launched map tasks=4
		Data-local map tasks=4
		Total time spent by all maps in occupied slots (ms)=12370
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=12370
		Total vcore-milliseconds taken by all map tasks=12370
		Total megabyte-milliseconds taken by all map tasks=12666880
	Map-Reduce Framework
		Map input records=364
		Map output records=364
		Input split bytes=876
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=141
		CPU time spent (ms)=2820
		Physical memory (bytes) snapshot=798699520
		Virtual memory (bytes) snapshot=6285520896
		Total committed heap usage (bytes)=689963008
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
18/04/07 23:16:06 INFO mapreduce.ExportJobBase: Transferred 28.5039 KB in 14.6683 seconds (1.9432 KB/sec)
18/04/07 23:16:06 INFO mapreduce.ExportJobBase: Exported 364 records.
18/04/07 23:16:06 INFO mapreduce.ExportJobBase: Starting to migrate data from staging table to destination.
18/04/07 23:16:06 ERROR manager.SqlManager: Unable to migrate data from `orders_daily_revenue_stage` to `orders_daily_revenue`
com.mysql.jdbc.exceptions.jdbc4.MySQLIntegrityConstraintViolationException: Duplicate entry '2013-08-06 00:00:00.0' for key 'PRIMARY'
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
	at com.mysql.jdbc.Util.handleNewInstance(Util.java:377)
	at com.mysql.jdbc.Util.getInstance(Util.java:360)
	at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:971)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3887)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3823)
	at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:2435)
	at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2582)
	at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2526)
	at com.mysql.jdbc.StatementImpl.executeUpdate(StatementImpl.java:1618)
	at com.mysql.jdbc.StatementImpl.executeUpdate(StatementImpl.java:1549)
	at org.apache.sqoop.manager.SqlManager.migrateData(SqlManager.java:1107)
	at org.apache.sqoop.mapreduce.ExportJobBase.runExport(ExportJobBase.java:459)
	at org.apache.sqoop.manager.SqlManager.exportTable(SqlManager.java:931)
	at org.apache.sqoop.tool.ExportTool.exportTable(ExportTool.java:80)
	at org.apache.sqoop.tool.ExportTool.run(ExportTool.java:99)
	at org.apache.sqoop.Sqoop.run(Sqoop.java:147)
	at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:70)
	at org.apache.sqoop.Sqoop.runSqoop(Sqoop.java:183)
	at org.apache.sqoop.Sqoop.runTool(Sqoop.java:234)
	at org.apache.sqoop.Sqoop.runTool(Sqoop.java:243)
	at org.apache.sqoop.Sqoop.main(Sqoop.java:252)
18/04/07 23:16:06 ERROR mapreduce.ExportJobBase: Failed to move data from staging table (orders_daily_revenue_stage) to target table (orders_daily_revenue)
com.mysql.jdbc.exceptions.jdbc4.MySQLIntegrityConstraintViolationException: Duplicate entry '2013-08-06 00:00:00.0' for key 'PRIMARY'
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
	at com.mysql.jdbc.Util.handleNewInstance(Util.java:377)
	at com.mysql.jdbc.Util.getInstance(Util.java:360)
	at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:971)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3887)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3823)
	at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:2435)
	at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2582)
	at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2526)
	at com.mysql.jdbc.StatementImpl.executeUpdate(StatementImpl.java:1618)
	at com.mysql.jdbc.StatementImpl.executeUpdate(StatementImpl.java:1549)
	at org.apache.sqoop.manager.SqlManager.migrateData(SqlManager.java:1107)
	at org.apache.sqoop.mapreduce.ExportJobBase.runExport(ExportJobBase.java:459)
	at org.apache.sqoop.manager.SqlManager.exportTable(SqlManager.java:931)
	at org.apache.sqoop.tool.ExportTool.exportTable(ExportTool.java:80)
	at org.apache.sqoop.tool.ExportTool.run(ExportTool.java:99)
	at org.apache.sqoop.Sqoop.run(Sqoop.java:147)
	at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:70)
	at org.apache.sqoop.Sqoop.runSqoop(Sqoop.java:183)
	at org.apache.sqoop.Sqoop.runTool(Sqoop.java:234)
	at org.apache.sqoop.Sqoop.runTool(Sqoop.java:243)
	at org.apache.sqoop.Sqoop.main(Sqoop.java:252)
18/04/07 23:16:06 ERROR tool.ExportTool: Error during export: 
Failed to move data from staging table
	at org.apache.sqoop.mapreduce.ExportJobBase.runExport(ExportJobBase.java:464)
	at org.apache.sqoop.manager.SqlManager.exportTable(SqlManager.java:931)
	at org.apache.sqoop.tool.ExportTool.exportTable(ExportTool.java:80)
	at org.apache.sqoop.tool.ExportTool.run(ExportTool.java:99)
	at org.apache.sqoop.Sqoop.run(Sqoop.java:147)
	at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:70)
	at org.apache.sqoop.Sqoop.runSqoop(Sqoop.java:183)
	at org.apache.sqoop.Sqoop.runTool(Sqoop.java:234)
	at org.apache.sqoop.Sqoop.runTool(Sqoop.java:243)
	at org.apache.sqoop.Sqoop.main(Sqoop.java:252)
Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLIntegrityConstraintViolationException: Duplicate entry '2013-08-06 00:00:00.0' for key 'PRIMARY'
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
	at com.mysql.jdbc.Util.handleNewInstance(Util.java:377)
	at com.mysql.jdbc.Util.getInstance(Util.java:360)
	at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:971)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3887)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3823)
	at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:2435)
	at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2582)
	at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2526)
	at com.mysql.jdbc.StatementImpl.executeUpdate(StatementImpl.java:1618)
	at com.mysql.jdbc.StatementImpl.executeUpdate(StatementImpl.java:1549)
	at org.apache.sqoop.manager.SqlManager.migrateData(SqlManager.java:1107)
	at org.apache.sqoop.mapreduce.ExportJobBase.runExport(ExportJobBase.java:459)
	... 9 more
~~~

* **Verify orders_daily_revenue table under MySQL**

~~~
mysql> select * from orders_daily_revenue;
+-----------------------+---------+
| order_date            | revenue |
+-----------------------+---------+
| 2013-08-06 00:00:00.0 |       0 |
+-----------------------+---------+
1 row in set (0.00 sec)
~~~

### Usage of --staging-table & --clear-staging-table - Successful Run by maintaining data consistency

* **Truncate orders_daily_revenue table under MySQL**

~~~
mysql> truncate table orders_daily_revenue;
Query OK, 1 row affected (0.01 sec)

mysql> select * from orders_daily_revenue;
Empty set (0.00 sec)
~~~

* **Export orders_daily_revenue table from HDFS to RDBMS using --num-mappers > 1**

~~~
[cloudera@quickstart ~]$ sqoop-export \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--export-dir /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue \
--input-fields-terminated-by '\001' \
--staging-table orders_daily_revenue_stage \
--clear-staging-table \
--table orders_daily_revenue \
--num-mappers 4

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/04/07 23:19:53 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/04/07 23:19:53 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/04/07 23:19:53 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/04/07 23:19:53 INFO tool.CodeGenTool: Beginning code generation
18/04/07 23:19:54 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 23:19:54 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/04/07 23:19:54 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/9966cb3f78aeeb050d038c569f89feb0/orders_daily_revenue.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/04/07 23:19:55 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/9966cb3f78aeeb050d038c569f89feb0/orders_daily_revenue.jar
18/04/07 23:19:55 INFO mapreduce.ExportJobBase: Data will be staged in the table: orders_daily_revenue_stage
18/04/07 23:19:55 INFO mapreduce.ExportJobBase: Beginning export of orders_daily_revenue
18/04/07 23:19:55 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/04/07 23:19:55 INFO manager.SqlManager: Deleted 364 records from `orders_daily_revenue_stage`
18/04/07 23:19:55 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/04/07 23:19:56 INFO Configuration.deprecation: mapred.reduce.tasks.speculative.execution is deprecated. Instead, use mapreduce.reduce.speculative
18/04/07 23:19:56 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 23:19:56 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/04/07 23:19:56 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/04/07 23:19:57 INFO input.FileInputFormat: Total input paths to process : 1
18/04/07 23:19:57 INFO input.FileInputFormat: Total input paths to process : 1
18/04/07 23:19:57 INFO mapreduce.JobSubmitter: number of splits:4
18/04/07 23:19:57 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/04/07 23:19:57 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0085
18/04/07 23:19:57 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0085
18/04/07 23:19:57 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0085/
18/04/07 23:19:57 INFO mapreduce.Job: Running job: job_1514521302404_0085
18/04/07 23:20:03 INFO mapreduce.Job: Job job_1514521302404_0085 running in uber mode : false
18/04/07 23:20:03 INFO mapreduce.Job:  map 0% reduce 0%
18/04/07 23:20:08 INFO mapreduce.Job:  map 25% reduce 0%
18/04/07 23:20:09 INFO mapreduce.Job:  map 50% reduce 0%
18/04/07 23:20:10 INFO mapreduce.Job:  map 75% reduce 0%
18/04/07 23:20:11 INFO mapreduce.Job:  map 100% reduce 0%
18/04/07 23:20:11 INFO mapreduce.Job: Job job_1514521302404_0085 completed successfully
18/04/07 23:20:11 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=606192
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=29188
		HDFS: Number of bytes written=0
		HDFS: Number of read operations=19
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=0
	Job Counters 
		Launched map tasks=4
		Data-local map tasks=4
		Total time spent by all maps in occupied slots (ms)=10865
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=10865
		Total vcore-milliseconds taken by all map tasks=10865
		Total megabyte-milliseconds taken by all map tasks=11125760
	Map-Reduce Framework
		Map input records=364
		Map output records=364
		Input split bytes=876
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=104
		CPU time spent (ms)=2430
		Physical memory (bytes) snapshot=803262464
		Virtual memory (bytes) snapshot=6271406080
		Total committed heap usage (bytes)=689963008
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
18/04/07 23:20:11 INFO mapreduce.ExportJobBase: Transferred 28.5039 KB in 15.5863 seconds (1.8288 KB/sec)
18/04/07 23:20:11 INFO mapreduce.ExportJobBase: Exported 364 records.
18/04/07 23:20:11 INFO mapreduce.ExportJobBase: Starting to migrate data from staging table to destination.
18/04/07 23:20:12 INFO manager.SqlManager: Migrated 364 records from `orders_daily_revenue_stage` to `orders_daily_revenue`
~~~

* **Verify orders_daily_revenue table under MySQL**

~~~
mysql> select count(*) from orders_daily_revenue;
+----------+
| count(*) |
+----------+
|      364 |
+----------+
1 row in set (0.00 sec)
~~~
