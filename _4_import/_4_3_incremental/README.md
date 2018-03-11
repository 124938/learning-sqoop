## Examples - Import data (Using incremental load)

### Approach -> 1 : Usage of --table with --where & --append

* Import orders table with `Jan 2014` data from MySQL to HDFS 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--where "order_date like '2014-01%'" \
--target-dir /user/cloudera/sqoop_import/retail_db/orders \
--append \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 04:43:36 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 04:43:36 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 04:43:36 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 04:43:36 INFO tool.CodeGenTool: Beginning code generation
18/03/10 04:43:37 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/10 04:43:37 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/10 04:43:37 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/8b34ad0f00a2c88d74c9f8d2c001272c/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 04:43:38 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/8b34ad0f00a2c88d74c9f8d2c001272c/orders.jar
18/03/10 04:43:38 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 04:43:38 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 04:43:38 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 04:43:38 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 04:43:38 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/10 04:43:38 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 04:43:38 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 04:43:38 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 04:43:39 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 04:43:40 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 04:43:40 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders` WHERE ( order_date like '2014-01%' )
18/03/10 04:43:40 INFO db.IntegerSplitter: Split size: 21459; Num splits: 2 from: 25876 to: 68794
18/03/10 04:43:40 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 04:43:40 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0026
18/03/10 04:43:40 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0026
18/03/10 04:43:40 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0026/
18/03/10 04:43:40 INFO mapreduce.Job: Running job: job_1514521302404_0026
18/03/10 04:43:46 INFO mapreduce.Job: Job job_1514521302404_0026 running in uber mode : false
18/03/10 04:43:46 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 04:43:51 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 04:43:52 INFO mapreduce.Job: Job job_1514521302404_0026 completed successfully
18/03/10 04:43:52 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=304018
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=237
    HDFS: Number of bytes written=258589
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=5636
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=5636
    Total vcore-milliseconds taken by all map tasks=5636
    Total megabyte-milliseconds taken by all map tasks=5771264
  Map-Reduce Framework
    Map input records=5908
    Map output records=5908
    Input split bytes=237
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=100
    CPU time spent (ms)=2470
    Physical memory (bytes) snapshot=420790272
    Virtual memory (bytes) snapshot=3156074496
    Total committed heap usage (bytes)=417333248
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=258589
18/03/10 04:43:52 INFO mapreduce.ImportJobBase: Transferred 252.5283 KB in 13.7313 seconds (18.3907 KB/sec)
18/03/10 04:43:52 INFO mapreduce.ImportJobBase: Retrieved 5908 records.
18/03/10 04:43:52 INFO util.AppendUtils: Creating missing output directory - orders
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0026/
~~~

* Verify orders data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/orders
-rw-r--r--   1 cloudera cloudera    209.3 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     43.2 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
~~~

### Approach -> 2 : Usage of --query with --split-by & --append

* Import orders table with `Feb 2014` data from MySQL to HDFS 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--query "select * from orders where order_date like '2014-02%' and \$CONDITIONS" \
--split-by order_id \
--target-dir /user/cloudera/sqoop_import/retail_db/orders \
--append \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 04:50:59 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 04:50:59 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 04:50:59 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 04:50:59 INFO tool.CodeGenTool: Beginning code generation
18/03/10 04:50:59 INFO manager.SqlManager: Executing SQL statement: select * from orders where order_date like '2014-02%' and  (1 = 0) 
18/03/10 04:50:59 INFO manager.SqlManager: Executing SQL statement: select * from orders where order_date like '2014-02%' and  (1 = 0) 
18/03/10 04:50:59 INFO manager.SqlManager: Executing SQL statement: select * from orders where order_date like '2014-02%' and  (1 = 0) 
18/03/10 04:50:59 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/5a83a330122ac856dd5c9f3370ae0c18/QueryResult.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 04:51:00 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/5a83a330122ac856dd5c9f3370ae0c18/QueryResult.jar
18/03/10 04:51:00 INFO mapreduce.ImportJobBase: Beginning query import.
18/03/10 04:51:00 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 04:51:01 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 04:51:01 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 04:51:01 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 04:51:02 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 04:51:02 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(order_id), MAX(order_id) FROM (select * from orders where order_date like '2014-02%' and  (1 = 1) ) AS t1
18/03/10 04:51:02 INFO db.IntegerSplitter: Split size: 19014; Num splits: 2 from: 30776 to: 68804
18/03/10 04:51:02 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 04:51:02 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0027
18/03/10 04:51:03 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0027
18/03/10 04:51:03 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0027/
18/03/10 04:51:03 INFO mapreduce.Job: Running job: job_1514521302404_0027
18/03/10 04:51:09 INFO mapreduce.Job: Job job_1514521302404_0027 running in uber mode : false
18/03/10 04:51:09 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 04:51:14 INFO mapreduce.Job:  map 50% reduce 0%
18/03/10 04:51:15 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 04:51:15 INFO mapreduce.Job: Job job_1514521302404_0027 completed successfully
18/03/10 04:51:15 INFO mapreduce.Job: Counters: 31
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=303890
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=229
    HDFS: Number of bytes written=246132
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Killed map tasks=1
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=5769
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=5769
    Total vcore-milliseconds taken by all map tasks=5769
    Total megabyte-milliseconds taken by all map tasks=5907456
  Map-Reduce Framework
    Map input records=5635
    Map output records=5635
    Input split bytes=229
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=116
    CPU time spent (ms)=2670
    Physical memory (bytes) snapshot=429924352
    Virtual memory (bytes) snapshot=3163865088
    Total committed heap usage (bytes)=419430400
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=246132
18/03/10 04:51:15 INFO mapreduce.ImportJobBase: Transferred 240.3633 KB in 13.7614 seconds (17.4665 KB/sec)
18/03/10 04:51:15 INFO mapreduce.ImportJobBase: Retrieved 5635 records.
18/03/10 04:51:15 INFO util.AppendUtils: Appending to directory orders
18/03/10 04:51:15 INFO util.AppendUtils: Using found partition 2
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0027/
~~~

* Verify orders data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/orders
-rw-r--r--   1 cloudera cloudera    209.3 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     43.2 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera    203.5 K 2018-03-10 04:51 /user/cloudera/sqoop_import/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     36.8 K 2018-03-10 04:51 /user/cloudera/sqoop_import/retail_db/orders/part-m-00003
~~~

### Approach -> 3 : Usage of --table with --check-column, --incremental & --last-value

* Import orders table with greater then `28-Feb-2014` data from MySQL to HDFS 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--check-column order_date \
--incremental append \
--last-value '2014-02-28' \
--target-dir /user/cloudera/sqoop_import/retail_db/orders \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 05:30:49 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 05:30:49 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 05:30:49 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 05:30:49 INFO tool.CodeGenTool: Beginning code generation
18/03/10 05:30:49 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/10 05:30:49 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/10 05:30:49 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/8843d47555a8d0a0b29c74b62a95659c/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 05:30:50 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/8843d47555a8d0a0b29c74b62a95659c/orders.jar
18/03/10 05:30:51 INFO tool.ImportTool: Maximal id query for free form incremental import: SELECT MAX(`order_date`) FROM `orders`
18/03/10 05:30:51 INFO tool.ImportTool: Incremental import based on column `order_date`
18/03/10 05:30:51 INFO tool.ImportTool: Lower bound value: '2014-02-28'
18/03/10 05:30:51 INFO tool.ImportTool: Upper bound value: '2014-07-24 00:00:00.0'
18/03/10 05:30:51 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 05:30:51 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 05:30:51 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 05:30:51 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 05:30:51 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/10 05:30:51 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 05:30:51 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 05:30:51 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 05:30:51 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 05:30:52 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 05:30:52 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders` WHERE ( `order_date` > '2014-02-28' AND `order_date` <= '2014-07-24 00:00:00.0' )
18/03/10 05:30:52 INFO db.IntegerSplitter: Split size: 16667; Num splits: 2 from: 35549 to: 68883
18/03/10 05:30:52 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 05:30:52 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0028
18/03/10 05:30:52 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0028
18/03/10 05:30:52 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0028/
18/03/10 05:30:52 INFO mapreduce.Job: Running job: job_1514521302404_0028
18/03/10 05:30:59 INFO mapreduce.Job: Job job_1514521302404_0028 running in uber mode : false
18/03/10 05:30:59 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 05:31:05 INFO mapreduce.Job:  map 50% reduce 0%
18/03/10 05:31:06 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 05:31:06 INFO mapreduce.Job: Job job_1514521302404_0028 completed successfully
18/03/10 05:31:06 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=304842
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=237
    HDFS: Number of bytes written=1166050
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=7054
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=7054
    Total vcore-milliseconds taken by all map tasks=7054
    Total megabyte-milliseconds taken by all map tasks=7223296
  Map-Reduce Framework
    Map input records=26678
    Map output records=26678
    Input split bytes=237
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=89
    CPU time spent (ms)=4760
    Physical memory (bytes) snapshot=441532416
    Virtual memory (bytes) snapshot=3159359488
    Total committed heap usage (bytes)=494403584
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=1166050
18/03/10 05:31:06 INFO mapreduce.ImportJobBase: Transferred 1.112 MB in 14.8899 seconds (76.4763 KB/sec)
18/03/10 05:31:06 INFO mapreduce.ImportJobBase: Retrieved 26678 records.
18/03/10 05:31:06 INFO util.AppendUtils: Appending to directory orders
18/03/10 05:31:06 INFO util.AppendUtils: Using found partition 4
18/03/10 05:31:06 INFO tool.ImportTool: Incremental import complete! To run another incremental import of all data following this import, supply the following arguments:
18/03/10 05:31:06 INFO tool.ImportTool:  --incremental append
18/03/10 05:31:06 INFO tool.ImportTool:   --check-column order_date
18/03/10 05:31:06 INFO tool.ImportTool:   --last-value 2014-07-24 00:00:00.0
18/03/10 05:31:06 INFO tool.ImportTool: (Consider saving this with 'sqoop job --create')
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0028/
~~~

* Verify orders data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/orders
-rw-r--r--   1 cloudera cloudera    209.3 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     43.2 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera    203.5 K 2018-03-10 04:51 /user/cloudera/sqoop_import/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     36.8 K 2018-03-10 04:51 /user/cloudera/sqoop_import/retail_db/orders/part-m-00003
-rw-r--r--   1 cloudera cloudera    711.3 K 2018-03-10 05:31 /user/cloudera/sqoop_import/retail_db/orders/part-m-00004
-rw-r--r--   1 cloudera cloudera    427.5 K 2018-03-10 05:31 /user/cloudera/sqoop_import/retail_db/orders/part-m-00005
~~~
