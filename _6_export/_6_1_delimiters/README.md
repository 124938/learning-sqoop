## Examples - Export data (Using delimiters)

### Pre-Requisite

* **Create orders_daily_revenue table under MySQL**

~~~
mysql> create table orders_daily_revenue(
 order_date varchar(100),
 revenue float
);
Query OK, 0 rows affected (0.02 sec)

mysql> describe orders_daily_revenue;
+------------+--------------+------+-----+---------+-------+
| Field      | Type         | Null | Key | Default | Extra |
+------------+--------------+------+-----+---------+-------+
| order_date | varchar(100) | YES  |     | NULL    |       |
| revenue    | float        | YES  |     | NULL    |       |
+------------+--------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
~~~

### Usage of --export-dir, --table, --input-fields-terminated-by & --num-mappers

* **Important Notes:**
  * --export-dir & --table is mandatory parameters while exporting data from HDFS to RDBMS
  * Default value of --input-fields-terminated-by is comma (",")
  * Default value of --num-mappers is 4  

* **Export orders_daily_revenue table from HDFS to RDBMS**

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
18/03/25 08:10:24 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/25 08:10:24 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/25 08:10:24 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/25 08:10:24 INFO tool.CodeGenTool: Beginning code generation
18/03/25 08:10:24 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/03/25 08:10:24 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders_daily_revenue` AS t LIMIT 1
18/03/25 08:10:24 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/716ff8ed7136e41277bbc5e4c1718e9d/orders_daily_revenue.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/25 08:10:26 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/716ff8ed7136e41277bbc5e4c1718e9d/orders_daily_revenue.jar
18/03/25 08:10:26 INFO mapreduce.ExportJobBase: Beginning export of orders_daily_revenue
18/03/25 08:10:26 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/25 08:10:26 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/25 08:10:27 INFO Configuration.deprecation: mapred.reduce.tasks.speculative.execution is deprecated. Instead, use mapreduce.reduce.speculative
18/03/25 08:10:27 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/03/25 08:10:27 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/25 08:10:27 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/25 08:10:28 INFO input.FileInputFormat: Total input paths to process : 1
18/03/25 08:10:28 INFO input.FileInputFormat: Total input paths to process : 1
18/03/25 08:10:28 INFO mapreduce.JobSubmitter: number of splits:1
18/03/25 08:10:28 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
18/03/25 08:10:28 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0069
18/03/25 08:10:28 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0069
18/03/25 08:10:28 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0069/
18/03/25 08:10:28 INFO mapreduce.Job: Running job: job_1514521302404_0069
18/03/25 08:10:34 INFO mapreduce.Job: Job job_1514521302404_0069 running in uber mode : false
18/03/25 08:10:34 INFO mapreduce.Job:  map 0% reduce 0%
18/03/25 08:10:38 INFO mapreduce.Job:  map 100% reduce 0%
18/03/25 08:10:38 INFO mapreduce.Job: Job job_1514521302404_0069 completed successfully
18/03/25 08:10:38 INFO mapreduce.Job: Counters: 30
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
    Total time spent by all maps in occupied slots (ms)=2146
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=2146
    Total vcore-milliseconds taken by all map tasks=2146
    Total megabyte-milliseconds taken by all map tasks=2197504
  Map-Reduce Framework
    Map input records=7
    Map output records=7
    Input split bytes=188
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=28
    CPU time spent (ms)=580
    Physical memory (bytes) snapshot=201707520
    Virtual memory (bytes) snapshot=1579806720
    Total committed heap usage (bytes)=172490752
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=0
18/03/25 08:10:38 INFO mapreduce.ExportJobBase: Transferred 475 bytes in 11.6815 seconds (40.6627 bytes/sec)
18/03/25 08:10:38 INFO mapreduce.ExportJobBase: Exported 7 records.
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