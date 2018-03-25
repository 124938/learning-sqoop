## Examples - Import data from table (Primary Key present in table)

### Usage of --target-dir

* **Important Notes:**
  * --target-dir should work only in case of directory is not available in HDFS

* **Import orders table from MySQL to HDFS**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--target-dir /user/cloudera/sqoop_import/retail_db/orders

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/04 04:38:52 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/04 04:38:52 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/04 04:38:52 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/04 04:38:52 INFO tool.CodeGenTool: Beginning code generation
18/03/04 04:38:53 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/04 04:38:53 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/04 04:38:53 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/7c444262f801bfd8a269c91543335582/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/04 04:38:54 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/7c444262f801bfd8a269c91543335582/orders.jar
18/03/04 04:38:54 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/04 04:38:54 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/04 04:38:54 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/04 04:38:54 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/04 04:38:54 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/04 04:38:54 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/04 04:38:54 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/04 04:38:54 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/04 04:38:54 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/04 04:38:56 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/04 04:38:56 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders`
18/03/04 04:38:56 INFO db.IntegerSplitter: Split size: 17220; Num splits: 4 from: 1 to: 68883
18/03/04 04:38:56 INFO mapreduce.JobSubmitter: number of splits:4
18/03/04 04:38:56 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0010
18/03/04 04:38:56 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0010
18/03/04 04:38:56 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0010/
18/03/04 04:38:56 INFO mapreduce.Job: Running job: job_1514521302404_0010
18/03/04 04:39:01 INFO mapreduce.Job: Job job_1514521302404_0010 running in uber mode : false
18/03/04 04:39:01 INFO mapreduce.Job:  map 0% reduce 0%
18/03/04 04:39:09 INFO mapreduce.Job:  map 25% reduce 0%
18/03/04 04:39:10 INFO mapreduce.Job:  map 50% reduce 0%
18/03/04 04:39:11 INFO mapreduce.Job:  map 75% reduce 0%
18/03/04 04:39:12 INFO mapreduce.Job:  map 100% reduce 0%
18/03/04 04:39:12 INFO mapreduce.Job: Job job_1514521302404_0010 completed successfully
18/03/04 04:39:12 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=606676
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=469
    HDFS: Number of bytes written=2999944
    HDFS: Number of read operations=16
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=8
  Job Counters 
    Launched map tasks=4
    Other local map tasks=4
    Total time spent by all maps in occupied slots (ms)=18473
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=18473
    Total vcore-milliseconds taken by all map tasks=18473
    Total megabyte-milliseconds taken by all map tasks=18916352
  Map-Reduce Framework
    Map input records=68883
    Map output records=68883
    Input split bytes=469
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=189
    CPU time spent (ms)=9950
    Physical memory (bytes) snapshot=961425408
    Virtual memory (bytes) snapshot=6290026496
    Total committed heap usage (bytes)=984612864
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=2999944
18/03/04 04:39:12 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 17.2403 seconds (169.9293 KB/sec)
18/03/04 04:39:12 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
~~~

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0010/
~~~

* **Verify orders data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/cloudera/sqoop_import/retail_db/orders

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 04:39 /user/cloudera/sqoop_import/retail_db/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera     741614 2018-03-04 04:39 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     753022 2018-03-04 04:39 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera     752368 2018-03-04 04:39 /user/cloudera/sqoop_import/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     752940 2018-03-04 04:39 /user/cloudera/sqoop_import/retail_db/orders/part-m-00003
~~~

### Usage of --target-dir with --delete-target-dir

* **Important Notes:**
  * --delete-target-dir make sure to delete --target-dir under HDFS before importing table data

* **Import orders table from MySQL to HDFS**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--target-dir /user/cloudera/sqoop_import/retail_db/orders \
--delete-target-dir

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/04 04:47:26 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/04 04:47:26 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/04 04:47:26 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/04 04:47:26 INFO tool.CodeGenTool: Beginning code generation
18/03/04 04:47:26 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/04 04:47:26 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/04 04:47:26 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/2aed635398cddc8f3316626457fc4d8f/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/04 04:47:27 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/2aed635398cddc8f3316626457fc4d8f/orders.jar
18/03/04 04:47:28 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/orders deleted.
18/03/04 04:47:28 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/04 04:47:28 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/04 04:47:28 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/04 04:47:28 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/04 04:47:28 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/04 04:47:28 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/04 04:47:28 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/04 04:47:28 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/04 04:47:28 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/04 04:47:29 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/04 04:47:29 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders`
18/03/04 04:47:29 INFO db.IntegerSplitter: Split size: 17220; Num splits: 4 from: 1 to: 68883
18/03/04 04:47:29 INFO mapreduce.JobSubmitter: number of splits:4
18/03/04 04:47:29 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0011
18/03/04 04:47:29 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0011
18/03/04 04:47:29 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0011/
18/03/04 04:47:29 INFO mapreduce.Job: Running job: job_1514521302404_0011
18/03/04 04:47:35 INFO mapreduce.Job: Job job_1514521302404_0011 running in uber mode : false
18/03/04 04:47:35 INFO mapreduce.Job:  map 0% reduce 0%
18/03/04 04:47:41 INFO mapreduce.Job:  map 25% reduce 0%
18/03/04 04:47:42 INFO mapreduce.Job:  map 50% reduce 0%
18/03/04 04:47:43 INFO mapreduce.Job:  map 75% reduce 0%
18/03/04 04:47:44 INFO mapreduce.Job:  map 100% reduce 0%
18/03/04 04:47:44 INFO mapreduce.Job: Job job_1514521302404_0011 completed successfully
18/03/04 04:47:44 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=606672
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=469
    HDFS: Number of bytes written=2999944
    HDFS: Number of read operations=16
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=8
  Job Counters 
    Launched map tasks=4
    Other local map tasks=4
    Total time spent by all maps in occupied slots (ms)=17335
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=17335
    Total vcore-milliseconds taken by all map tasks=17335
    Total megabyte-milliseconds taken by all map tasks=17751040
  Map-Reduce Framework
    Map input records=68883
    Map output records=68883
    Input split bytes=469
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=206
    CPU time spent (ms)=10160
    Physical memory (bytes) snapshot=887062528
    Virtual memory (bytes) snapshot=6302167040
    Total committed heap usage (bytes)=916455424
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=2999944
18/03/04 04:47:44 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 15.6679 seconds (186.9826 KB/sec)
18/03/04 04:47:44 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
~~~

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0011/
~~~

* **Verify orders data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/cloudera/sqoop_import/retail_db/orders

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 04:47 /user/cloudera/sqoop_import/retail_db/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera     741614 2018-03-04 04:47 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     753022 2018-03-04 04:47 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera     752368 2018-03-04 04:47 /user/cloudera/sqoop_import/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     752940 2018-03-04 04:47 /user/cloudera/sqoop_import/retail_db/orders/part-m-00003
~~~

### Usage of --warehouse-dir

* **Important Notes:**
  * --warehouse-dir make sure to create directory under HDFS with name provided in --table clause

* **Remove orders directory from HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -rm -R /user/cloudera/sqoop_import/retail_db/orders
Deleted /user/cloudera/sqoop_import/retail_db/orders
~~~

* **Import orders table from MySQL to HDFS**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--warehouse-dir /user/cloudera/sqoop_import/retail_db

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/04 05:20:26 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/04 05:20:26 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/04 05:20:26 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/04 05:20:26 INFO tool.CodeGenTool: Beginning code generation
18/03/04 05:20:26 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/04 05:20:26 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/04 05:20:26 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/ba8290fb210bb5d7bb7c8973e13decb6/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/04 05:20:27 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/ba8290fb210bb5d7bb7c8973e13decb6/orders.jar
18/03/04 05:20:27 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/04 05:20:27 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/04 05:20:27 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/04 05:20:27 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/04 05:20:27 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/04 05:20:27 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/04 05:20:27 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/04 05:20:28 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/04 05:20:28 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/04 05:20:30 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/04 05:20:30 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders`
18/03/04 05:20:30 INFO db.IntegerSplitter: Split size: 17220; Num splits: 4 from: 1 to: 68883
18/03/04 05:20:30 INFO mapreduce.JobSubmitter: number of splits:4
18/03/04 05:20:30 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0013
18/03/04 05:20:30 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0013
18/03/04 05:20:30 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0013/
18/03/04 05:20:30 INFO mapreduce.Job: Running job: job_1514521302404_0013
18/03/04 05:20:35 INFO mapreduce.Job: Job job_1514521302404_0013 running in uber mode : false
18/03/04 05:20:35 INFO mapreduce.Job:  map 0% reduce 0%
18/03/04 05:20:42 INFO mapreduce.Job:  map 25% reduce 0%
18/03/04 05:20:43 INFO mapreduce.Job:  map 50% reduce 0%
18/03/04 05:20:44 INFO mapreduce.Job:  map 75% reduce 0%
18/03/04 05:20:45 INFO mapreduce.Job:  map 100% reduce 0%
18/03/04 05:20:45 INFO mapreduce.Job: Job job_1514521302404_0013 completed successfully
18/03/04 05:20:45 INFO mapreduce.Job: Counters: 31
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=606660
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=469
    HDFS: Number of bytes written=2999944
    HDFS: Number of read operations=16
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=8
  Job Counters 
    Killed map tasks=1
    Launched map tasks=4
    Other local map tasks=4
    Total time spent by all maps in occupied slots (ms)=18628
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=18628
    Total vcore-milliseconds taken by all map tasks=18628
    Total megabyte-milliseconds taken by all map tasks=19075072
  Map-Reduce Framework
    Map input records=68883
    Map output records=68883
    Input split bytes=469
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=184
    CPU time spent (ms)=9600
    Physical memory (bytes) snapshot=922529792
    Virtual memory (bytes) snapshot=6297198592
    Total committed heap usage (bytes)=987234304
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=2999944
18/03/04 05:20:45 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 17.2585 seconds (169.7505 KB/sec)
18/03/04 05:20:45 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
~~~

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0013/
~~~

* **Verify orders data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/cloudera/sqoop_import/retail_db/orders

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 05:20 /user/cloudera/sqoop_import/retail_db/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera     741614 2018-03-04 05:20 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     753022 2018-03-04 05:20 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera     752368 2018-03-04 05:20 /user/cloudera/sqoop_import/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     752940 2018-03-04 05:20 /user/cloudera/sqoop_import/retail_db/orders/part-m-00003
~~~

### Usage of --num-mappers

* **Important Notes:**
  * Import command generate 4 files under HDFS because default value of --num-mappers is 4, this can be override by specifying --num-mappers value in command 

* **Remove orders directory from HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -rm -R /user/cloudera/sqoop_import/retail_db/orders
Deleted /user/cloudera/sqoop_import/retail_db/orders
~~~

* **Import orders table from MySQL to HDFS**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--target-dir /user/cloudera/sqoop_import/retail_db/orders \
--num-mappers 1

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/04 05:39:29 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/04 05:39:29 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/04 05:39:29 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/04 05:39:29 INFO tool.CodeGenTool: Beginning code generation
18/03/04 05:39:29 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/04 05:39:29 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/04 05:39:29 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/feff37d20971cdc0ee49a4da6e26ab4f/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/04 05:39:31 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/feff37d20971cdc0ee49a4da6e26ab4f/orders.jar
18/03/04 05:39:31 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/04 05:39:31 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/04 05:39:31 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/04 05:39:31 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/04 05:39:31 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/04 05:39:31 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/04 05:39:31 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/04 05:39:31 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/04 05:39:31 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/04 05:39:32 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/04 05:39:32 INFO mapreduce.JobSubmitter: number of splits:1
18/03/04 05:39:32 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0014
18/03/04 05:39:33 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0014
18/03/04 05:39:33 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0014/
18/03/04 05:39:33 INFO mapreduce.Job: Running job: job_1514521302404_0014
18/03/04 05:39:38 INFO mapreduce.Job: Job job_1514521302404_0014 running in uber mode : false
18/03/04 05:39:38 INFO mapreduce.Job:  map 0% reduce 0%
18/03/04 05:39:44 INFO mapreduce.Job:  map 100% reduce 0%
18/03/04 05:39:44 INFO mapreduce.Job: Job job_1514521302404_0014 completed successfully
18/03/04 05:39:44 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=151669
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=87
    HDFS: Number of bytes written=2999944
    HDFS: Number of read operations=4
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=2
  Job Counters 
    Launched map tasks=1
    Other local map tasks=1
    Total time spent by all maps in occupied slots (ms)=3077
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=3077
    Total vcore-milliseconds taken by all map tasks=3077
    Total megabyte-milliseconds taken by all map tasks=3150848
  Map-Reduce Framework
    Map input records=68883
    Map output records=68883
    Input split bytes=87
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=26
    CPU time spent (ms)=3000
    Physical memory (bytes) snapshot=289284096
    Virtual memory (bytes) snapshot=1579343872
    Total committed heap usage (bytes)=244842496
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=2999944
18/03/04 05:39:44 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 12.7538 seconds (229.7073 KB/sec)
18/03/04 05:39:44 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
~~~

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0014/
~~~

* **Verify orders data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/orders

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 05:39 /user/cloudera/sqoop_import/retail_db/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera      2.9 M 2018-03-04 05:39 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
~~~
