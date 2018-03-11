## Examples - Import data (Using Compression)

### Important Notes:
* gzip is default compression technique

### Usage of --compress (Gzip)

* Import order_items table data from MySQL to HDFS in text file using gzip format 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir \
--as-textfile \
--compress \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 21:24:12 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 21:24:12 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 21:24:12 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 21:24:12 INFO tool.CodeGenTool: Beginning code generation
18/03/10 21:24:12 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 21:24:12 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 21:24:12 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/e79f9991b582a7d24cf5bd097b5f344b/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 21:24:13 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/e79f9991b582a7d24cf5bd097b5f344b/order_items.jar
18/03/10 21:24:14 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items deleted.
18/03/10 21:24:14 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 21:24:14 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 21:24:14 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 21:24:14 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 21:24:14 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/10 21:24:14 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 21:24:14 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 21:24:14 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 21:24:14 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 21:24:15 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 21:24:15 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/10 21:24:15 INFO db.IntegerSplitter: Split size: 86098; Num splits: 2 from: 1 to: 172198
18/03/10 21:24:15 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 21:24:15 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0038
18/03/10 21:24:15 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0038
18/03/10 21:24:15 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0038/
18/03/10 21:24:15 INFO mapreduce.Job: Running job: job_1514521302404_0038
18/03/10 21:24:22 INFO mapreduce.Job: Job job_1514521302404_0038 running in uber mode : false
18/03/10 21:24:22 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 21:24:27 INFO mapreduce.Job:  map 50% reduce 0%
18/03/10 21:24:28 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 21:24:28 INFO mapreduce.Job: Job job_1514521302404_0038 completed successfully
18/03/10 21:24:28 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=303540
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=254
    HDFS: Number of bytes written=1030557
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=7155
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=7155
    Total vcore-milliseconds taken by all map tasks=7155
    Total megabyte-milliseconds taken by all map tasks=7326720
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=254
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=103
    CPU time spent (ms)=5770
    Physical memory (bytes) snapshot=561246208
    Virtual memory (bytes) snapshot=3166609408
    Total committed heap usage (bytes)=493355008
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=1030557
18/03/10 21:24:28 INFO mapreduce.ImportJobBase: Transferred 1,006.4033 KB in 13.7164 seconds (73.372 KB/sec)
18/03/10 21:24:28 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0037/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items
-rw-r--r--   1 cloudera cloudera          0 2018-03-10 21:24 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    504.1 K 2018-03-10 21:24 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000.gz
-rw-r--r--   1 cloudera cloudera    502.3 K 2018-03-10 21:24 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001.gz
~~~

### Usage of --compress & --compression-codec (Snappy)

* Import order_items table data from MySQL to HDFS in text file using snappy format 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir \
--as-textfile \
--compress \
--compression-codec org.apache.hadoop.io.compress.SnappyCodec \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 21:46:36 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 21:46:36 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 21:46:36 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 21:46:36 INFO tool.CodeGenTool: Beginning code generation
18/03/10 21:46:37 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 21:46:37 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 21:46:37 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/f336d1da5c74cc7ccd4509a46fb67bc3/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 21:46:38 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/f336d1da5c74cc7ccd4509a46fb67bc3/order_items.jar
18/03/10 21:46:39 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items deleted.
18/03/10 21:46:39 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 21:46:39 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 21:46:39 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 21:46:39 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 21:46:39 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/10 21:46:39 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 21:46:39 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 21:46:39 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 21:46:39 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 21:46:40 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 21:46:40 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/10 21:46:40 INFO db.IntegerSplitter: Split size: 86098; Num splits: 2 from: 1 to: 172198
18/03/10 21:46:40 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 21:46:40 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0039
18/03/10 21:46:40 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0039
18/03/10 21:46:40 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0039/
18/03/10 21:46:40 INFO mapreduce.Job: Running job: job_1514521302404_0039
18/03/10 21:46:45 INFO mapreduce.Job: Job job_1514521302404_0039 running in uber mode : false
18/03/10 21:46:45 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 21:46:51 INFO mapreduce.Job:  map 50% reduce 0%
18/03/10 21:46:52 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 21:46:52 INFO mapreduce.Job: Job job_1514521302404_0039 completed successfully
18/03/10 21:46:52 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=303892
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=254
    HDFS: Number of bytes written=1877490
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=7928
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=7928
    Total vcore-milliseconds taken by all map tasks=7928
    Total megabyte-milliseconds taken by all map tasks=8118272
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=254
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=168
    CPU time spent (ms)=6510
    Physical memory (bytes) snapshot=602329088
    Virtual memory (bytes) snapshot=3171786752
    Total committed heap usage (bytes)=489684992
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=1877490
18/03/10 21:46:52 INFO mapreduce.ImportJobBase: Transferred 1.7905 MB in 13.6129 seconds (134.6871 KB/sec)
18/03/10 21:46:52 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0039/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items
-rw-r--r--   1 cloudera cloudera          0 2018-03-10 21:46 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    919.3 K 2018-03-10 21:46 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000.snappy
-rw-r--r--   1 cloudera cloudera    914.2 K 2018-03-10 21:46 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001.snappy
~~~

### Usage of --compress & --compression-codec (bzip)

* Import order_items table data from MySQL to HDFS in text file using bzip format 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir \
--as-textfile \
--compress \
--compression-codec org.apache.hadoop.io.compress.BZip2Codec \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 21:49:51 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 21:49:51 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 21:49:51 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 21:49:51 INFO tool.CodeGenTool: Beginning code generation
18/03/10 21:49:51 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 21:49:51 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 21:49:51 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/a39f623508b9503cae0b621d5e508af4/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 21:49:52 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/a39f623508b9503cae0b621d5e508af4/order_items.jar
18/03/10 21:49:53 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items deleted.
18/03/10 21:49:53 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 21:49:53 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 21:49:53 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 21:49:53 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 21:49:53 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/10 21:49:53 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 21:49:53 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 21:49:53 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 21:49:53 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 21:49:54 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 21:49:54 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/10 21:49:54 INFO db.IntegerSplitter: Split size: 86098; Num splits: 2 from: 1 to: 172198
18/03/10 21:49:54 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 21:49:54 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0040
18/03/10 21:49:55 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0040
18/03/10 21:49:55 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0040/
18/03/10 21:49:55 INFO mapreduce.Job: Running job: job_1514521302404_0040
18/03/10 21:50:01 INFO mapreduce.Job: Job job_1514521302404_0040 running in uber mode : false
18/03/10 21:50:01 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 21:50:06 INFO mapreduce.Job:  map 50% reduce 0%
18/03/10 21:50:07 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 21:50:07 INFO mapreduce.Job: Job job_1514521302404_0040 completed successfully
18/03/10 21:50:07 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=303888
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=254
    HDFS: Number of bytes written=810609
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=7256
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=7256
    Total vcore-milliseconds taken by all map tasks=7256
    Total megabyte-milliseconds taken by all map tasks=7430144
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=254
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=145
    CPU time spent (ms)=6110
    Physical memory (bytes) snapshot=582529024
    Virtual memory (bytes) snapshot=3161698304
    Total committed heap usage (bytes)=497025024
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=810609
18/03/10 21:50:07 INFO mapreduce.ImportJobBase: Transferred 791.6104 KB in 13.8399 seconds (57.1976 KB/sec)
18/03/10 21:50:07 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0040/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items
-rw-r--r--   1 cloudera cloudera          0 2018-03-10 21:50 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    397.9 K 2018-03-10 21:50 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000.bz2
-rw-r--r--   1 cloudera cloudera    393.7 K 2018-03-10 21:50 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001.bz2
~~~

### Usage of --compress & --compression-codec (deflate)

* Import order_items table data from MySQL to HDFS in text file using deflat format 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir \
--as-textfile \
--compress \
--compression-codec org.apache.hadoop.io.compress.DeflateCodec \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 21:53:59 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 21:54:00 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 21:54:00 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 21:54:00 INFO tool.CodeGenTool: Beginning code generation
18/03/10 21:54:00 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 21:54:00 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 21:54:00 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/456f7367ce247b1608d364c16603398c/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 21:54:01 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/456f7367ce247b1608d364c16603398c/order_items.jar
18/03/10 21:54:02 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items is not present, hence not deleting.
18/03/10 21:54:02 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 21:54:02 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 21:54:02 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 21:54:02 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 21:54:02 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/10 21:54:02 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 21:54:02 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 21:54:02 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 21:54:02 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 21:54:03 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 21:54:03 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/10 21:54:03 INFO db.IntegerSplitter: Split size: 86098; Num splits: 2 from: 1 to: 172198
18/03/10 21:54:03 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 21:54:03 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0041
18/03/10 21:54:04 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0041
18/03/10 21:54:04 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0041/
18/03/10 21:54:04 INFO mapreduce.Job: Running job: job_1514521302404_0041
18/03/10 21:54:10 INFO mapreduce.Job: Job job_1514521302404_0041 running in uber mode : false
18/03/10 21:54:10 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 21:54:15 INFO mapreduce.Job:  map 50% reduce 0%
18/03/10 21:54:16 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 21:54:16 INFO mapreduce.Job: Job job_1514521302404_0041 completed successfully
18/03/10 21:54:16 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=303896
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=254
    HDFS: Number of bytes written=1030533
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=7029
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=7029
    Total vcore-milliseconds taken by all map tasks=7029
    Total megabyte-milliseconds taken by all map tasks=7197696
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=254
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=97
    CPU time spent (ms)=5840
    Physical memory (bytes) snapshot=569233408
    Virtual memory (bytes) snapshot=3148525568
    Total committed heap usage (bytes)=497025024
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=1030533
18/03/10 21:54:16 INFO mapreduce.ImportJobBase: Transferred 1,006.3799 KB in 14.0672 seconds (71.5407 KB/sec)
18/03/10 21:54:16 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0041/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items
-rw-r--r--   1 cloudera cloudera          0 2018-03-10 21:54 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    504.1 K 2018-03-10 21:54 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000.deflate
-rw-r--r--   1 cloudera cloudera    502.3 K 2018-03-10 21:54 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001.deflate
~~~
