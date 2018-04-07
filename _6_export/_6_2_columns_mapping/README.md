## Examples - Export data (Using column mapping)

### Pre-Requisite

* **Create orders_daily_revenue table under MySQL**

~~~
mysql> drop table if exists orders_daily_revenue;
Query OK, 0 rows affected (0.03 sec)

mysql> create table orders_daily_revenue(
 revenue float, 
 order_date varchar(100),
 desctiption varchar(200)
);
Query OK, 0 rows affected (0.00 sec)

mysql> describe orders_daily_revenue;
+-------------+--------------+------+-----+---------+-------+
| Field       | Type         | Null | Key | Default | Extra |
+-------------+--------------+------+-----+---------+-------+
| revenue     | float        | YES  |     | NULL    |       |
| order_date  | varchar(100) | YES  |     | NULL    |       |
| desctiption | varchar(200) | YES  |     | NULL    |       |
+-------------+--------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
~~~

### Usage of --columns

* **Export orders_daily_revenue table from HDFS to RDBMS (without --columns)**

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
18/03/25 09:59:20 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/25 09:59:20 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/25 09:59:20 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/25 09:59:20 INFO tool.CodeGenTool: Beginning code generation
18/03/25 09:59:20 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/03/25 09:59:20 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/03/25 09:59:20 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/84d5668211a0adfc3be28dcc55400ab4/orders_daily_revenue.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/25 09:59:21 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/84d5668211a0adfc3be28dcc55400ab4/orders_daily_revenue.jar
18/03/25 09:59:21 INFO mapreduce.ExportJobBase: Beginning export of orders_daily_revenue
18/03/25 09:59:21 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/25 09:59:21 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/25 09:59:22 INFO Configuration.deprecation: mapred.reduce.tasks.speculative.execution is deprecated. Instead, use mapreduce.reduce.speculative
18/03/25 09:59:22 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/03/25 09:59:22 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/25 09:59:22 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/25 09:59:23 INFO input.FileInputFormat: Total input paths to process : 1
18/03/25 09:59:23 INFO input.FileInputFormat: Total input paths to process : 1
18/03/25 09:59:23 INFO mapreduce.JobSubmitter: number of splits:1
18/03/25 09:59:23 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/03/25 09:59:23 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0070
18/03/25 09:59:23 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0070
18/03/25 09:59:24 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0070/
18/03/25 09:59:24 INFO mapreduce.Job: Running job: job_1514521302404_0070
18/03/25 09:59:29 INFO mapreduce.Job: Job job_1514521302404_0070 running in uber mode : false
18/03/25 09:59:29 INFO mapreduce.Job:  map 0% reduce 0%
18/03/25 09:59:34 INFO mapreduce.Job:  map 100% reduce 0%
18/03/25 09:59:34 INFO mapreduce.Job: Job job_1514521302404_0070 failed with state FAILED due to: Task failed task_1514521302404_0070_m_000000
Job failed as tasks failed. failedMaps:1 failedReduces:0

18/03/25 09:59:34 INFO mapreduce.Job: Counters: 8
  Job Counters 
    Failed map tasks=1
    Launched map tasks=1
    Data-local map tasks=1
    Total time spent by all maps in occupied slots (ms)=2101
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=2101
    Total vcore-milliseconds taken by all map tasks=2101
    Total megabyte-milliseconds taken by all map tasks=2151424
18/03/25 09:59:34 WARN mapreduce.Counters: Group FileSystemCounters is deprecated. Use org.apache.hadoop.mapreduce.FileSystemCounter instead
18/03/25 09:59:34 INFO mapreduce.ExportJobBase: Transferred 0 bytes in 11.6953 seconds (0 bytes/sec)
18/03/25 09:59:34 WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
18/03/25 09:59:34 INFO mapreduce.ExportJobBase: Exported 0 records.
18/03/25 09:59:34 ERROR tool.ExportTool: Error during export: 
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

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0070/
~~~

* **Export orders_daily_revenue table from HDFS to RDBMS (with --columns)**

~~~
[cloudera@quickstart ~]$ sqoop-export \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--export-dir /user/hive/warehouse/retail_db_sqoop_export.db/orders_daily_revenue \
--input-fields-terminated-by '\001' \
--table orders_daily_revenue \
--columns 'order_date,revenue' \
--num-mappers 1

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/25 10:03:36 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/25 10:03:36 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/25 10:03:36 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/25 10:03:36 INFO tool.CodeGenTool: Beginning code generation
18/03/25 10:03:36 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/03/25 10:03:36 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/03/25 10:03:36 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/e2673e12835a6428b81eea875f73b2a1/orders_daily_revenue.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/25 10:03:37 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/e2673e12835a6428b81eea875f73b2a1/orders_daily_revenue.jar
18/03/25 10:03:37 INFO mapreduce.ExportJobBase: Beginning export of orders_daily_revenue
18/03/25 10:03:37 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/25 10:03:37 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/25 10:03:38 INFO Configuration.deprecation: mapred.reduce.tasks.speculative.execution is deprecated. Instead, use mapreduce.reduce.speculative
18/03/25 10:03:38 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/03/25 10:03:38 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/25 10:03:38 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/25 10:03:40 INFO input.FileInputFormat: Total input paths to process : 1
18/03/25 10:03:40 INFO input.FileInputFormat: Total input paths to process : 1
18/03/25 10:03:40 INFO mapreduce.JobSubmitter: number of splits:1
18/03/25 10:03:40 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/03/25 10:03:40 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0071
18/03/25 10:03:40 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0071
18/03/25 10:03:40 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0071/
18/03/25 10:03:40 INFO mapreduce.Job: Running job: job_1514521302404_0071
18/03/25 10:03:46 INFO mapreduce.Job: Job job_1514521302404_0071 running in uber mode : false
18/03/25 10:03:46 INFO mapreduce.Job:  map 0% reduce 0%
18/03/25 10:03:50 INFO mapreduce.Job:  map 100% reduce 0%
18/03/25 10:03:50 INFO mapreduce.Job: Job job_1514521302404_0071 completed successfully
18/03/25 10:03:50 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=151526
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
    Total time spent by all maps in occupied slots (ms)=2178
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=2178
    Total vcore-milliseconds taken by all map tasks=2178
    Total megabyte-milliseconds taken by all map tasks=2230272
  Map-Reduce Framework
    Map input records=7
    Map output records=7
    Input split bytes=188
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=25
    CPU time spent (ms)=550
    Physical memory (bytes) snapshot=204546048
    Virtual memory (bytes) snapshot=1577517056
    Total committed heap usage (bytes)=172490752
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=0
18/03/25 10:03:50 INFO mapreduce.ExportJobBase: Transferred 475 bytes in 12.2181 seconds (38.8769 bytes/sec)
18/03/25 10:03:50 INFO mapreduce.ExportJobBase: Exported 7 records.
~~~

* **Verify orders_daily_revenue table under MySQL**

~~~
mysql> select * from orders_daily_revenue;
+---------+-----------------------+-------------+
| revenue | order_date            | desctiption |
+---------+-----------------------+-------------+
| 68153.8 | 2013-07-25 00:00:00.0 | NULL        |
|  136520 | 2013-07-26 00:00:00.0 | NULL        |
|  101074 | 2013-07-27 00:00:00.0 | NULL        |
| 87123.1 | 2013-07-28 00:00:00.0 | NULL        |
|  137287 | 2013-07-29 00:00:00.0 | NULL        |
|  102746 | 2013-07-30 00:00:00.0 | NULL        |
|  131878 | 2013-07-31 00:00:00.0 | NULL        |
+---------+-----------------------+-------------+
7 rows in set (0.00 sec)
~~~