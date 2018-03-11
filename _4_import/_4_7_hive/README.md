## Examples - Import data into Hive

### Important Notes:
* Importing data from RDBMS to external hive table is not supported

### Pre-Requisite

* Create database in Hive

~~~
[cloudera@quickstart ~]$ hive
Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j.properties
WARNING: Hive CLI is deprecated and migration to Beeline is recommended.

hive> show databases;
OK
default

hive> create database retail_db_sqoop_import;
OK

hive> show databases;
OK
default
retail_db_sqoop_import

hive> use retail_db_sqoop_import;
OK
~~~

* Verify database on HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/hive/
drwxrwxrwx   - hive supergroup          0 2018-03-11 04:09 /user/hive/warehouse
drwxrwxrwx   - cloudera supergroup          0 2018-03-11 04:09 /user/hive/warehouse/retail_db_sqoop_import.db
~~~

### Usage of --hive-import, --hive-database, --hive-table (Hive table does not exist)

* Import orders table from MySQL to Hive

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--hive-import \
--hive-database retail_db_sqoop_import \
--hive-table orders \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/11 04:23:31 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/11 04:23:31 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/11 04:23:31 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
18/03/11 04:23:31 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
18/03/11 04:23:31 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/11 04:23:31 INFO tool.CodeGenTool: Beginning code generation
18/03/11 04:23:31 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/11 04:23:32 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/11 04:23:32 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/6da412d423bd328a45e6f93c7b330b5f/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 04:23:33 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/6da412d423bd328a45e6f93c7b330b5f/orders.jar
18/03/11 04:23:33 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/11 04:23:33 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/11 04:23:33 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/11 04:23:33 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/11 04:23:33 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/11 04:23:33 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 04:23:33 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/11 04:23:33 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/11 04:23:33 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 04:23:34 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 04:23:34 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders`
18/03/11 04:23:34 INFO db.IntegerSplitter: Split size: 34441; Num splits: 2 from: 1 to: 68883
18/03/11 04:23:34 INFO mapreduce.JobSubmitter: number of splits:2
18/03/11 04:23:35 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0042
18/03/11 04:23:35 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0042
18/03/11 04:23:35 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0042/
18/03/11 04:23:35 INFO mapreduce.Job: Running job: job_1514521302404_0042
18/03/11 04:23:41 INFO mapreduce.Job: Job job_1514521302404_0042 running in uber mode : false
18/03/11 04:23:41 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 04:23:46 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 04:23:47 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 04:23:47 INFO mapreduce.Job: Job job_1514521302404_0042 completed successfully
18/03/11 04:23:47 INFO mapreduce.Job: Counters: 30
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
        Total time spent by all maps in occupied slots (ms)=7150
        Total time spent by all reduces in occupied slots (ms)=0
        Total time spent by all map tasks (ms)=7150
        Total vcore-milliseconds taken by all map tasks=7150
        Total megabyte-milliseconds taken by all map tasks=7321600
    Map-Reduce Framework
        Map input records=68883
        Map output records=68883
        Input split bytes=233
        Spilled Records=0
        Failed Shuffles=0
        Merged Map outputs=0
        GC time elapsed (ms)=104
        CPU time spent (ms)=5710
        Physical memory (bytes) snapshot=516579328
        Virtual memory (bytes) snapshot=3168460800
        Total committed heap usage (bytes)=492306432
    File Input Format Counters 
        Bytes Read=0
    File Output Format Counters 
        Bytes Written=2999944
18/03/11 04:23:47 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 13.8411 seconds (211.6623 KB/sec)
18/03/11 04:23:47 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
18/03/11 04:23:47 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/11 04:23:47 WARN hive.TableDefWriter: Column order_date had to be cast to a less precise type in Hive
18/03/11 04:23:47 INFO hive.HiveImport: Loading uploaded data into Hive

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 1.879 seconds
Loading data to table retail_db_sqoop_import.orders
Table retail_db_sqoop_import.orders stats: [numFiles=2, totalSize=2999944]
OK
Time taken: 0.782 seconds
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0042/
~~~

* Verify orders data under Hive

~~~
[cloudera@quickstart ~]$ hive
Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j.properties
WARNING: Hive CLI is deprecated and migration to Beeline is recommended.

hive> use retail_db_sqoop_import;
OK

hive> show tables;
OK
orders

hive> select count(1) from orders;
Query ID = cloudera_20180311043030_cb25ba36-5745-4d31-a693-cf02b3dac658
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514521302404_0044, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514521302404_0044/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514521302404_0044
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2018-03-11 04:30:23,737 Stage-1 map = 0%,  reduce = 0%
2018-03-11 04:30:27,920 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.52 sec
2018-03-11 04:30:34,196 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 2.67 sec
MapReduce Total cumulative CPU time: 2 seconds 670 msec
Ended Job = job_1514521302404_0044
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 2.67 sec   HDFS Read: 6007465 HDFS Write: 7 SUCCESS
Total MapReduce CPU Time Spent: 2 seconds 670 msec
OK
68883
~~~

* Verify orders data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/hive/warehouse/retail_db_sqoop_import.db

drwxrwxrwx   - cloudera supergroup          0 2018-03-11 04:23 /user/hive/warehouse/retail_db_sqoop_import.db/orders
-rw-r--r--   1 cloudera cloudera        1.4 M 2018-03-11 04:23 /user/hive/warehouse/retail_db_sqoop_import.db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera        1.4 M 2018-03-11 04:23 /user/hive/warehouse/retail_db_sqoop_import.db/orders/part-m-00001
~~~

### Usage of --hive-import, --hive-database, --hive-table & --create-hive-table (Hive table is already exist)

* Import orders table from MySQL to Hive 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--hive-import \
--hive-database retail_db_sqoop_import \
--hive-table orders \
--create-hive-table \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/11 05:08:43 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/11 05:08:43 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/11 05:08:43 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
18/03/11 05:08:43 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
18/03/11 05:08:43 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/11 05:08:43 INFO tool.CodeGenTool: Beginning code generation
18/03/11 05:08:44 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/11 05:08:44 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/11 05:08:44 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/8df8b8d01bf83b94e3baaf46acc33f09/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 05:08:45 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/8df8b8d01bf83b94e3baaf46acc33f09/orders.jar
18/03/11 05:08:45 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/11 05:08:45 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/11 05:08:45 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/11 05:08:45 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/11 05:08:45 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/11 05:08:45 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 05:08:45 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/11 05:08:46 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/11 05:08:46 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 05:08:47 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 05:08:47 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders`
18/03/11 05:08:47 INFO db.IntegerSplitter: Split size: 34441; Num splits: 2 from: 1 to: 68883
18/03/11 05:08:47 INFO mapreduce.JobSubmitter: number of splits:2
18/03/11 05:08:47 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0047
18/03/11 05:08:47 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0047
18/03/11 05:08:47 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0047/
18/03/11 05:08:47 INFO mapreduce.Job: Running job: job_1514521302404_0047
18/03/11 05:08:53 INFO mapreduce.Job: Job job_1514521302404_0047 running in uber mode : false
18/03/11 05:08:53 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 05:08:58 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 05:08:59 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 05:08:59 INFO mapreduce.Job: Job job_1514521302404_0047 completed successfully
18/03/11 05:08:59 INFO mapreduce.Job: Counters: 30
    File System Counters
        FILE: Number of bytes read=0
        FILE: Number of bytes written=303530
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
        Total time spent by all maps in occupied slots (ms)=7012
        Total time spent by all reduces in occupied slots (ms)=0
        Total time spent by all map tasks (ms)=7012
        Total vcore-milliseconds taken by all map tasks=7012
        Total megabyte-milliseconds taken by all map tasks=7180288
    Map-Reduce Framework
        Map input records=68883
        Map output records=68883
        Input split bytes=233
        Spilled Records=0
        Failed Shuffles=0
        Merged Map outputs=0
        GC time elapsed (ms)=83
        CPU time spent (ms)=5850
        Physical memory (bytes) snapshot=574889984
        Virtual memory (bytes) snapshot=3174133760
        Total committed heap usage (bytes)=489684992
    File Input Format Counters 
        Bytes Read=0
    File Output Format Counters 
        Bytes Written=2999944
18/03/11 05:08:59 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 13.8925 seconds (210.8786 KB/sec)
18/03/11 05:08:59 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
18/03/11 05:08:59 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/11 05:08:59 WARN hive.TableDefWriter: Column order_date had to be cast to a less precise type in Hive
18/03/11 05:08:59 INFO hive.HiveImport: Loading uploaded data into Hive

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. AlreadyExistsException(message:Table orders already exists)
~~~

### Usage of --hive-import, --hive-database, --hive-table & --hive-overwrite (Overwrite data in existing hive table)

* Import orders table from MySQL to Hive

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--hive-import \
--hive-database retail_db_sqoop_import \
--hive-table orders \
--hive-overwrite \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/11 04:33:10 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/11 04:33:10 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/11 04:33:10 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
18/03/11 04:33:10 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
18/03/11 04:33:10 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/11 04:33:10 INFO tool.CodeGenTool: Beginning code generation
18/03/11 04:33:11 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/11 04:33:11 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/11 04:33:11 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/596286b45e499e52929082c4ebfdfccb/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 04:33:12 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/596286b45e499e52929082c4ebfdfccb/orders.jar
18/03/11 04:33:12 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/11 04:33:12 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/11 04:33:12 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/11 04:33:12 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/11 04:33:12 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/11 04:33:12 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 04:33:12 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/11 04:33:13 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/11 04:33:13 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 04:33:14 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 04:33:14 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders`
18/03/11 04:33:14 INFO db.IntegerSplitter: Split size: 34441; Num splits: 2 from: 1 to: 68883
18/03/11 04:33:14 INFO mapreduce.JobSubmitter: number of splits:2
18/03/11 04:33:14 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0045
18/03/11 04:33:14 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0045
18/03/11 04:33:14 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0045/
18/03/11 04:33:14 INFO mapreduce.Job: Running job: job_1514521302404_0045
18/03/11 04:33:19 INFO mapreduce.Job: Job job_1514521302404_0045 running in uber mode : false
18/03/11 04:33:19 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 04:33:24 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 04:33:25 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 04:33:25 INFO mapreduce.Job: Job job_1514521302404_0045 completed successfully
18/03/11 04:33:25 INFO mapreduce.Job: Counters: 30
    File System Counters
        FILE: Number of bytes read=0
        FILE: Number of bytes written=303530
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
        Total time spent by all maps in occupied slots (ms)=7030
        Total time spent by all reduces in occupied slots (ms)=0
        Total time spent by all map tasks (ms)=7030
        Total vcore-milliseconds taken by all map tasks=7030
        Total megabyte-milliseconds taken by all map tasks=7198720
    Map-Reduce Framework
        Map input records=68883
        Map output records=68883
        Input split bytes=233
        Spilled Records=0
        Failed Shuffles=0
        Merged Map outputs=0
        GC time elapsed (ms)=78
        CPU time spent (ms)=5670
        Physical memory (bytes) snapshot=473690112
        Virtual memory (bytes) snapshot=3169787904
        Total committed heap usage (bytes)=494927872
    File Input Format Counters 
        Bytes Read=0
    File Output Format Counters 
        Bytes Written=2999944
18/03/11 04:33:25 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 12.6936 seconds (230.7959 KB/sec)
18/03/11 04:33:25 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
18/03/11 04:33:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/11 04:33:25 WARN hive.TableDefWriter: Column order_date had to be cast to a less precise type in Hive
18/03/11 04:33:25 INFO hive.HiveImport: Loading uploaded data into Hive

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 1.103 seconds
Loading data to table retail_db_sqoop_import.orders
chgrp: changing ownership of 'hdfs://quickstart.cloudera:8020/user/hive/warehouse/retail_db_sqoop_import.db/orders': User does not belong to supergroup
Table retail_db_sqoop_import.orders stats: [numFiles=2, numRows=0, totalSize=2999944, rawDataSize=0]
OK
Time taken: 0.631 seconds
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0045/
~~~

* Verify orders data under Hive

~~~
[cloudera@quickstart ~]$ hive

Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j.properties
WARNING: Hive CLI is deprecated and migration to Beeline is recommended.
hive> use retail_db_sqoop_import;
OK

hive> show tables;
OK
orders

hive> select * from orders limit 10;
OK
1   2013-07-25 00:00:00.0   11599   CLOSED
2   2013-07-25 00:00:00.0   256 PENDING_PAYMENT
3   2013-07-25 00:00:00.0   12111   COMPLETE
4   2013-07-25 00:00:00.0   8827    CLOSED
5   2013-07-25 00:00:00.0   11318   COMPLETE
6   2013-07-25 00:00:00.0   7130    COMPLETE
7   2013-07-25 00:00:00.0   4530    COMPLETE
8   2013-07-25 00:00:00.0   2911    PROCESSING
9   2013-07-25 00:00:00.0   5657    PENDING_PAYMENT
10  2013-07-25 00:00:00.0   5648    PENDING_PAYMENT

hive> select count(1) from orders;
Query ID = cloudera_20180311043535_00ebfe5e-2e63-4b97-9f81-a06ce56c7247
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514521302404_0046, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514521302404_0046/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514521302404_0046
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2018-03-11 04:35:44,757 Stage-1 map = 0%,  reduce = 0%
2018-03-11 04:35:48,937 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.46 sec
2018-03-11 04:35:55,182 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 2.61 sec
MapReduce Total cumulative CPU time: 2 seconds 610 msec
Ended Job = job_1514521302404_0046
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 2.61 sec   HDFS Read: 3007342 HDFS Write: 6 SUCCESS
Total MapReduce CPU Time Spent: 2 seconds 610 msec
OK
68883
~~~

* Verify orders data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/hive/warehouse/retail_db_sqoop_import.db
drwxrwxrwx   - cloudera cloudera          0 2018-03-11 04:33 /user/hive/warehouse/retail_db_sqoop_import.db/orders
-rwxrwxrwx   1 cloudera cloudera          0 2018-03-11 04:33 /user/hive/warehouse/retail_db_sqoop_import.db/orders/_SUCCESS
-rwxrwxrwx   1 cloudera cloudera      1.4 M 2018-03-11 04:33 /user/hive/warehouse/retail_db_sqoop_import.db/orders/part-m-00000
-rwxrwxrwx   1 cloudera cloudera      1.4 M 2018-03-11 04:33 /user/hive/warehouse/retail_db_sqoop_import.db/orders/part-m-00001
~~~

### Usage of --hive-import, --hive-database, --hive-table (Hive table is already exist)

* Create emp table in hive

~~~
hive> create table emp(
 id int,
 f_name string,
 l_name string,
 manager string,
 dept_id int)
 location '/user/cloudera/emp';

hive> show tables;
emp
orders

hive> describe formatted emp;
# col_name              data_type               comment             
         
id                      int                                         
f_name                  string                                      
l_name                  string                                      
manager                 string                                      
dept_id                 int                                         
         
# Detailed Table Information         
Database:               retail_db_sqoop_import   
Owner:                  cloudera                 
CreateTime:             Sun Mar 11 06:01:17 PDT 2018     
LastAccessTime:         UNKNOWN                  
Protect Mode:           None                     
Retention:              0                        
Location:               hdfs://quickstart.cloudera:8020/user/hive/warehouse/retail_db_sqoop_import.db/emp    
Table Type:             MANAGED_TABLE            
Table Parameters:        
    transient_lastDdlTime   1520773277          
         
# Storage Information        
SerDe Library:          org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe   
InputFormat:            org.apache.hadoop.mapred.TextInputFormat     
OutputFormat:           org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat   
Compressed:             No                       
Num Buckets:            -1                       
Bucket Columns:         []                       
Sort Columns:           []                       
Storage Desc Params:         
    serialization.format    1                   
~~~

* Import emp table data from RDBMS to existing hive table

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table emp \
--hive-import \
--hive-database retail_db_sqoop_import \
--hive-table emp \
--num-mappers 1

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/11 06:02:33 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/11 06:02:33 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/11 06:02:33 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
18/03/11 06:02:33 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
18/03/11 06:02:33 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/11 06:02:33 INFO tool.CodeGenTool: Beginning code generation
18/03/11 06:02:33 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `emp` AS t LIMIT 1
18/03/11 06:02:33 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `emp` AS t LIMIT 1
18/03/11 06:02:33 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/8ba5fe689c6c1c9f7a816de4a6989c22/emp.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 06:02:35 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/8ba5fe689c6c1c9f7a816de4a6989c22/emp.jar
18/03/11 06:02:35 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/11 06:02:35 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/11 06:02:35 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/11 06:02:35 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/11 06:02:35 INFO mapreduce.ImportJobBase: Beginning import of emp
18/03/11 06:02:35 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 06:02:35 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/11 06:02:35 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/11 06:02:35 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 06:02:37 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 06:02:37 INFO mapreduce.JobSubmitter: number of splits:1
18/03/11 06:02:37 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0048
18/03/11 06:02:37 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0048
18/03/11 06:02:37 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0048/
18/03/11 06:02:37 INFO mapreduce.Job: Running job: job_1514521302404_0048
18/03/11 06:02:42 INFO mapreduce.Job: Job job_1514521302404_0048 running in uber mode : false
18/03/11 06:02:42 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 06:02:46 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 06:02:47 INFO mapreduce.Job: Job job_1514521302404_0048 completed successfully
18/03/11 06:02:48 INFO mapreduce.Job: Counters: 30
    File System Counters
        FILE: Number of bytes read=0
        FILE: Number of bytes written=151726
        FILE: Number of read operations=0
        FILE: Number of large read operations=0
        FILE: Number of write operations=0
        HDFS: Number of bytes read=87
        HDFS: Number of bytes written=121
        HDFS: Number of read operations=4
        HDFS: Number of large read operations=0
        HDFS: Number of write operations=2
    Job Counters 
        Launched map tasks=1
        Other local map tasks=1
        Total time spent by all maps in occupied slots (ms)=2228
        Total time spent by all reduces in occupied slots (ms)=0
        Total time spent by all map tasks (ms)=2228
        Total vcore-milliseconds taken by all map tasks=2228
        Total megabyte-milliseconds taken by all map tasks=2281472
    Map-Reduce Framework
        Map input records=4
        Map output records=4
        Input split bytes=87
        Spilled Records=0
        Failed Shuffles=0
        Merged Map outputs=0
        GC time elapsed (ms)=24
        CPU time spent (ms)=710
        Physical memory (bytes) snapshot=203386880
        Virtual memory (bytes) snapshot=1576071168
        Total committed heap usage (bytes)=172490752
    File Input Format Counters 
        Bytes Read=0
    File Output Format Counters 
        Bytes Written=121
18/03/11 06:02:48 INFO mapreduce.ImportJobBase: Transferred 121 bytes in 12.2556 seconds (9.873 bytes/sec)
18/03/11 06:02:48 INFO mapreduce.ImportJobBase: Retrieved 4 records.
18/03/11 06:02:48 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `emp` AS t LIMIT 1
18/03/11 06:02:48 INFO hive.HiveImport: Loading uploaded data into Hive

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 1.052 seconds
Loading data to table retail_db_sqoop_import.emp
Table retail_db_sqoop_import.emp stats: [numFiles=1, totalSize=121]
OK
Time taken: 0.553 seconds
~~~

* Verify emp table data in Hive

~~~
[cloudera@quickstart ~]$ hive
Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j.properties
dbWARNING: Hive CLI is deprecated and migration to Beeline is recommended.

hive> use retail_db_sqoop_import;
OK

hive> select * from emp;
OK
1   shreyash    limbhetwala null    NULL
2   bonika  limbhetwala null    NULL
3   dherya  limbhetwala boni    1
4   hrisha  limbhetwala shrey   1
Time taken: 0.491 seconds, Fetched: 4 row(s)
~~~
