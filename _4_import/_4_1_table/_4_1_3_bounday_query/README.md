## Examples - Import data from table (Custom Bondary Query & Columns)

### Approach -> 1 : Usage of --boundary-query

* **Important Notes:**
  * --boundary-query is useful in case of filtering data from source table
  * --boundary-query must return two columns

* **Import order_items table from MySQL to HDFS using custom bondary query**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--boundary-query "SELECT MIN(order_item_id), MAX(order_item_id) FROM order_items WHERE order_item_id > 99999" \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/04 07:41:23 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/04 07:41:23 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/04 07:41:23 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/04 07:41:23 INFO tool.CodeGenTool: Beginning code generation
18/03/04 07:41:23 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/04 07:41:23 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/04 07:41:23 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/d0a4567a81ed735332d48d95d27f542a/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/04 07:41:25 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/d0a4567a81ed735332d48d95d27f542a/order_items.jar
18/03/04 07:41:25 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items is not present, hence not deleting.
18/03/04 07:41:25 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/04 07:41:25 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/04 07:41:25 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/04 07:41:25 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/04 07:41:25 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/04 07:41:25 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/04 07:41:25 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/04 07:41:25 WARN db.DataDrivenDBInputFormat: Could not find $CONDITIONS token in query: SELECT MIN(order_item_id), MAX(order_item_id) FROM order_items WHERE order_item_id > 99999; splits may not partition data.
18/03/04 07:41:25 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/04 07:41:25 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/04 07:41:27 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/04 07:41:27 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(order_item_id), MAX(order_item_id) FROM order_items WHERE order_item_id > 99999
18/03/04 07:41:27 INFO db.IntegerSplitter: Split size: 18049; Num splits: 4 from: 100000 to: 172198
18/03/04 07:41:27 INFO mapreduce.JobSubmitter: number of splits:4
18/03/04 07:41:27 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0019
18/03/04 07:41:27 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0019
18/03/04 07:41:27 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0019/
18/03/04 07:41:27 INFO mapreduce.Job: Running job: job_1514521302404_0019
18/03/04 07:41:32 INFO mapreduce.Job: Job job_1514521302404_0019 running in uber mode : false
18/03/04 07:41:32 INFO mapreduce.Job:  map 0% reduce 0%
18/03/04 07:41:38 INFO mapreduce.Job:  map 25% reduce 0%
18/03/04 07:41:39 INFO mapreduce.Job:  map 50% reduce 0%
18/03/04 07:41:40 INFO mapreduce.Job:  map 75% reduce 0%
18/03/04 07:41:41 INFO mapreduce.Job:  map 100% reduce 0%
18/03/04 07:41:41 INFO mapreduce.Job: Job job_1514521302404_0019 completed successfully
18/03/04 07:41:42 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=608968
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=521
    HDFS: Number of bytes written=2328327
    HDFS: Number of read operations=16
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=8
  Job Counters 
    Launched map tasks=4
    Other local map tasks=4
    Total time spent by all maps in occupied slots (ms)=16373
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=16373
    Total vcore-milliseconds taken by all map tasks=16373
    Total megabyte-milliseconds taken by all map tasks=16765952
  Map-Reduce Framework
    Map input records=72199
    Map output records=72199
    Input split bytes=521
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=168
    CPU time spent (ms)=7790
    Physical memory (bytes) snapshot=884666368
    Virtual memory (bytes) snapshot=6303793152
    Total committed heap usage (bytes)=914882560
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=2328327
18/03/04 07:41:42 INFO mapreduce.ImportJobBase: Transferred 2.2205 MB in 16.2345 seconds (140.0575 KB/sec)
18/03/04 07:41:42 INFO mapreduce.ImportJobBase: Retrieved 72199 records.
~~~

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0019/
~~~

* **Verify order_items data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 07:41 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    567.6 K 2018-03-04 07:41 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000
-rw-r--r--   1 cloudera cloudera    567.4 K 2018-03-04 07:41 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001
-rw-r--r--   1 cloudera cloudera    568.9 K 2018-03-04 07:41 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00002
-rw-r--r--   1 cloudera cloudera    569.9 K 2018-03-04 07:41 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00003
~~~

### Approach -> 2 : Usage of --boundary-query

* **Import order_items table from MySQL to HDFS using custom bondary query**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--boundary-query "SELECT 100000, 172199" \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/04 07:47:24 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/04 07:47:24 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/04 07:47:24 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/04 07:47:24 INFO tool.CodeGenTool: Beginning code generation
18/03/04 07:47:24 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/04 07:47:24 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/04 07:47:24 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/e4f39f53ccf531dc534f9f24bad7991f/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/04 07:47:25 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/e4f39f53ccf531dc534f9f24bad7991f/order_items.jar
18/03/04 07:47:26 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items deleted.
18/03/04 07:47:26 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/04 07:47:26 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/04 07:47:26 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/04 07:47:26 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/04 07:47:26 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/04 07:47:26 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/04 07:47:26 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/04 07:47:26 WARN db.DataDrivenDBInputFormat: Could not find $CONDITIONS token in query: SELECT 100000, 172199; splits may not partition data.
18/03/04 07:47:26 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/04 07:47:26 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/04 07:47:27 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/04 07:47:27 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT 100000, 172199
18/03/04 07:47:27 INFO db.IntegerSplitter: Split size: 18049; Num splits: 4 from: 100000 to: 172199
18/03/04 07:47:27 INFO mapreduce.JobSubmitter: number of splits:4
18/03/04 07:47:27 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0020
18/03/04 07:47:27 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0020
18/03/04 07:47:27 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0020/
18/03/04 07:47:27 INFO mapreduce.Job: Running job: job_1514521302404_0020
18/03/04 07:47:32 INFO mapreduce.Job: Job job_1514521302404_0020 running in uber mode : false
18/03/04 07:47:32 INFO mapreduce.Job:  map 0% reduce 0%
18/03/04 07:47:39 INFO mapreduce.Job:  map 25% reduce 0%
18/03/04 07:47:40 INFO mapreduce.Job:  map 50% reduce 0%
18/03/04 07:47:41 INFO mapreduce.Job:  map 75% reduce 0%
18/03/04 07:47:42 INFO mapreduce.Job:  map 100% reduce 0%
18/03/04 07:47:42 INFO mapreduce.Job: Job job_1514521302404_0020 completed successfully
18/03/04 07:47:42 INFO mapreduce.Job: Counters: 31
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=608376
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=521
    HDFS: Number of bytes written=2328327
    HDFS: Number of read operations=16
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=8
  Job Counters 
    Killed map tasks=1
    Launched map tasks=4
    Other local map tasks=4
    Total time spent by all maps in occupied slots (ms)=15353
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=15353
    Total vcore-milliseconds taken by all map tasks=15353
    Total megabyte-milliseconds taken by all map tasks=15721472
  Map-Reduce Framework
    Map input records=72199
    Map output records=72199
    Input split bytes=521
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=186
    CPU time spent (ms)=7680
    Physical memory (bytes) snapshot=906539008
    Virtual memory (bytes) snapshot=6326386688
    Total committed heap usage (bytes)=989331456
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=2328327
18/03/04 07:47:42 INFO mapreduce.ImportJobBase: Transferred 2.2205 MB in 15.637 seconds (145.4086 KB/sec)
18/03/04 07:47:42 INFO mapreduce.ImportJobBase: Retrieved 72199 records.
~~~

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0020/
~~~

* **Verify order_items data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items
-rw-r--r--   1 cloudera cloudera          0 2018-03-04 07:47 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    567.6 K 2018-03-04 07:47 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000
-rw-r--r--   1 cloudera cloudera    567.4 K 2018-03-04 07:47 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001
-rw-r--r--   1 cloudera cloudera    569.0 K 2018-03-04 07:47 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00002
-rw-r--r--   1 cloudera cloudera    569.8 K 2018-03-04 07:47 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00003
~~~

### Usage of --columns

* **Import order_items table from MySQL to HDFS by specific columns**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--columns "order_item_id, order_item_order_id, order_item_subtotal" \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/09 21:59:24 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/09 21:59:24 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/09 21:59:24 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/09 21:59:24 INFO tool.CodeGenTool: Beginning code generation
18/03/09 21:59:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/09 21:59:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/09 21:59:25 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/c288276c7f60a4f009d5da56c08f9fdb/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/09 21:59:26 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/c288276c7f60a4f009d5da56c08f9fdb/order_items.jar
18/03/09 21:59:27 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items deleted.
18/03/09 21:59:27 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/09 21:59:27 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/09 21:59:27 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/09 21:59:27 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/09 21:59:27 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/09 21:59:27 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/09 21:59:27 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/09 21:59:27 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/09 21:59:27 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/09 21:59:28 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/09 21:59:28 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/09 21:59:28 INFO db.IntegerSplitter: Split size: 43049; Num splits: 4 from: 1 to: 172198
18/03/09 21:59:28 INFO mapreduce.JobSubmitter: number of splits:4
18/03/09 21:59:28 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0021
18/03/09 21:59:28 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0021
18/03/09 21:59:28 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0021/
18/03/09 21:59:28 INFO mapreduce.Job: Running job: job_1514521302404_0021
18/03/09 21:59:34 INFO mapreduce.Job: Job job_1514521302404_0021 running in uber mode : false
18/03/09 21:59:34 INFO mapreduce.Job:  map 0% reduce 0%
18/03/09 21:59:39 INFO mapreduce.Job:  map 25% reduce 0%
18/03/09 21:59:41 INFO mapreduce.Job:  map 50% reduce 0%
18/03/09 21:59:42 INFO mapreduce.Job:  map 75% reduce 0%
18/03/09 21:59:43 INFO mapreduce.Job:  map 100% reduce 0%
18/03/09 21:59:43 INFO mapreduce.Job: Job job_1514521302404_0021 completed successfully
18/03/09 21:59:44 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=607564
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=512
    HDFS: Number of bytes written=3245187
    HDFS: Number of read operations=16
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=8
  Job Counters 
    Launched map tasks=4
    Other local map tasks=4
    Total time spent by all maps in occupied slots (ms)=16757
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=16757
    Total vcore-milliseconds taken by all map tasks=16757
    Total megabyte-milliseconds taken by all map tasks=17159168
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=512
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=149
    CPU time spent (ms)=9110
    Physical memory (bytes) snapshot=1001385984
    Virtual memory (bytes) snapshot=6324170752
    Total committed heap usage (bytes)=984612864
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=3245187
18/03/09 21:59:44 INFO mapreduce.ImportJobBase: Transferred 3.0949 MB in 16.9327 seconds (187.1598 KB/sec)
18/03/09 21:59:44 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
~~~

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0020/
~~~

* **Verify order_items data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items 
-rw-r--r--   1 cloudera cloudera          0 2018-03-09 21:59 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    745.8 K 2018-03-09 21:59 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000
-rw-r--r--   1 cloudera cloudera    783.7 K 2018-03-09 21:59 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001
-rw-r--r--   1 cloudera cloudera    812.2 K 2018-03-09 21:59 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00002
-rw-r--r--   1 cloudera cloudera    827.5 K 2018-03-09 21:59 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00003
~~~
