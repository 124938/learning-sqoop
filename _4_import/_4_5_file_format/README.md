## Examples - Import data (Using different file format)

### Important Notes:
* Text is default file format while importing data

### Usage of --as-textfile

* Import order_items table data from MySQL to HDFS in text file format 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir \
--as-textfile \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 08:13:52 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 08:13:52 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 08:13:52 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 08:13:52 INFO tool.CodeGenTool: Beginning code generation
18/03/10 08:13:53 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:13:53 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:13:53 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/e9fb138f992a95c1ef91d6ea325c6436/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 08:13:54 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/e9fb138f992a95c1ef91d6ea325c6436/order_items.jar
18/03/10 08:13:55 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items deleted.
18/03/10 08:13:55 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 08:13:55 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 08:13:55 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 08:13:55 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 08:13:55 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/10 08:13:55 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 08:13:55 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 08:13:55 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 08:13:55 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 08:13:56 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 08:13:56 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/10 08:13:56 INFO db.IntegerSplitter: Split size: 86098; Num splits: 2 from: 1 to: 172198
18/03/10 08:13:56 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 08:13:56 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0034
18/03/10 08:13:56 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0034
18/03/10 08:13:56 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0034/
18/03/10 08:13:56 INFO mapreduce.Job: Running job: job_1514521302404_0034
18/03/10 08:14:02 INFO mapreduce.Job: Job job_1514521302404_0034 running in uber mode : false
18/03/10 08:14:02 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 08:14:10 INFO mapreduce.Job:  map 50% reduce 0%
18/03/10 08:14:11 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 08:14:11 INFO mapreduce.Job: Job job_1514521302404_0034 completed successfully
18/03/10 08:14:11 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=303562
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=254
    HDFS: Number of bytes written=5408880
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=10853
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=10853
    Total vcore-milliseconds taken by all map tasks=10853
    Total megabyte-milliseconds taken by all map tasks=11113472
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=254
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=519
    CPU time spent (ms)=8890
    Physical memory (bytes) snapshot=587350016
    Virtual memory (bytes) snapshot=3174109184
    Total committed heap usage (bytes)=497025024
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=5408880
18/03/10 08:14:11 INFO mapreduce.ImportJobBase: Transferred 5.1583 MB in 16.7045 seconds (316.2082 KB/sec)
18/03/10 08:14:11 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0034/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items
-rw-r--r--   1 cloudera cloudera          0 2018-03-10 08:14 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera      2.5 M 2018-03-10 08:14 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000
-rw-r--r--   1 cloudera cloudera      2.6 M 2018-03-10 08:14 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001
~~~

~~~
[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000 | more
1,1,957,1,299.98,299.98
2,2,1073,1,199.99,199.99
3,2,502,5,250.0,50.0
4,2,403,1,129.99,129.99
5,4,897,2,49.98,24.99
6,4,365,5,299.95,59.99
7,4,502,3,150.0,50.0
~~~

### Usage of --as-sequencefile

* Import order_items table data from MySQL to HDFS in sequence file format 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir \
--as-sequencefile \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 08:23:07 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 08:23:07 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 08:23:07 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 08:23:07 INFO tool.CodeGenTool: Beginning code generation
18/03/10 08:23:08 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:23:08 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:23:08 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/85b8f6b4887b7781a019cdeda610d185/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 08:23:09 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/85b8f6b4887b7781a019cdeda610d185/order_items.jar
18/03/10 08:23:10 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items deleted.
18/03/10 08:23:10 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 08:23:10 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 08:23:10 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 08:23:10 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 08:23:10 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/10 08:23:10 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 08:23:10 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 08:23:10 INFO Configuration.deprecation: mapred.output.value.class is deprecated. Instead, use mapreduce.job.output.value.class
18/03/10 08:23:10 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 08:23:10 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 08:23:11 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 08:23:11 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/10 08:23:11 INFO db.IntegerSplitter: Split size: 86098; Num splits: 2 from: 1 to: 172198
18/03/10 08:23:11 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 08:23:11 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0035
18/03/10 08:23:11 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0035
18/03/10 08:23:12 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0035/
18/03/10 08:23:12 INFO mapreduce.Job: Running job: job_1514521302404_0035
18/03/10 08:23:17 INFO mapreduce.Job: Job job_1514521302404_0035 running in uber mode : false
18/03/10 08:23:17 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 08:23:23 INFO mapreduce.Job:  map 50% reduce 0%
18/03/10 08:23:24 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 08:23:24 INFO mapreduce.Job: Job job_1514521302404_0035 completed successfully
18/03/10 08:23:24 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=303304
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=254
    HDFS: Number of bytes written=7999492
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=7062
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=7062
    Total vcore-milliseconds taken by all map tasks=7062
    Total megabyte-milliseconds taken by all map tasks=7231488
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=254
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=119
    CPU time spent (ms)=3940
    Physical memory (bytes) snapshot=427405312
    Virtual memory (bytes) snapshot=3126714368
    Total committed heap usage (bytes)=422051840
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=7999492
18/03/10 08:23:24 INFO mapreduce.ImportJobBase: Transferred 7.6289 MB in 14.1939 seconds (550.3791 KB/sec)
18/03/10 08:23:24 INFO mapreduce.ImportJobBase: Retrieved 172198 records.

~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0035/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items
-rw-r--r--   1 cloudera cloudera          0 2018-03-10 08:23 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera      3.8 M 2018-03-10 08:23 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000
-rw-r--r--   1 cloudera cloudera      3.8 M 2018-03-10 08:23 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001
~~~

~~~
[cloudera@quickstart ~]$ hadoop fs -text /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000
-text: Fatal internal error
java.lang.RuntimeException: java.io.IOException: WritableName can't load class: order_items
  at org.apache.hadoop.io.SequenceFile$Reader.getValueClass(SequenceFile.java:2110)
  at org.apache.hadoop.io.SequenceFile$Reader.init(SequenceFile.java:2040)
  at org.apache.hadoop.io.SequenceFile$Reader.initialize(SequenceFile.java:1881)
  at org.apache.hadoop.io.SequenceFile$Reader.<init>(SequenceFile.java:1830)
  at org.apache.hadoop.fs.shell.Display$TextRecordInputStream.<init>(Display.java:222)
  at org.apache.hadoop.fs.shell.Display$Text.getInputStream(Display.java:152)
  at org.apache.hadoop.fs.shell.Display$Cat.processPath(Display.java:101)
  at org.apache.hadoop.fs.shell.Command.processPaths(Command.java:317)
  at org.apache.hadoop.fs.shell.Command.processPathArgument(Command.java:289)
  at org.apache.hadoop.fs.shell.Command.processArgument(Command.java:271)
  at org.apache.hadoop.fs.shell.Command.processArguments(Command.java:255)
  at org.apache.hadoop.fs.shell.FsCommand.processRawArguments(FsCommand.java:118)
  at org.apache.hadoop.fs.shell.Command.run(Command.java:165)
  at org.apache.hadoop.fs.FsShell.run(FsShell.java:315)
  at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:70)
  at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:84)
  at org.apache.hadoop.fs.FsShell.main(FsShell.java:372)
Caused by: java.io.IOException: WritableName can't load class: order_items
  at org.apache.hadoop.io.WritableName.getClass(WritableName.java:77)
  at org.apache.hadoop.io.SequenceFile$Reader.getValueClass(SequenceFile.java:2108)
  ... 16 more
Caused by: java.lang.ClassNotFoundException: Class order_items not found
  at org.apache.hadoop.conf.Configuration.getClassByName(Configuration.java:2108)
  at org.apache.hadoop.io.WritableName.getClass(WritableName.java:75)
~~~

### Usage of --as-avrodatafile

* Import order_items table data from MySQL to HDFS in avro file format 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir \
--as-avrodatafile \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 08:40:02 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 08:40:02 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 08:40:02 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 08:40:02 INFO tool.CodeGenTool: Beginning code generation
18/03/10 08:40:02 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:40:02 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:40:02 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/6ae0881e80ad9e2d080d1b679d94706b/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 08:40:03 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/6ae0881e80ad9e2d080d1b679d94706b/order_items.jar
18/03/10 08:40:04 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items deleted.
18/03/10 08:40:04 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 08:40:04 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 08:40:04 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 08:40:04 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 08:40:04 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/10 08:40:04 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 08:40:04 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 08:40:04 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:40:04 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:40:04 INFO mapreduce.DataDrivenImportJob: Writing Avro schema file: /tmp/sqoop-cloudera/compile/6ae0881e80ad9e2d080d1b679d94706b/order_items.avsc
18/03/10 08:40:04 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 08:40:04 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 08:40:05 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 08:40:05 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/10 08:40:05 INFO db.IntegerSplitter: Split size: 86098; Num splits: 2 from: 1 to: 172198
18/03/10 08:40:05 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 08:40:05 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0036
18/03/10 08:40:06 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0036
18/03/10 08:40:06 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0036/
18/03/10 08:40:06 INFO mapreduce.Job: Running job: job_1514521302404_0036
18/03/10 08:40:11 INFO mapreduce.Job: Job job_1514521302404_0036 running in uber mode : false
18/03/10 08:40:11 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 08:40:17 INFO mapreduce.Job:  map 50% reduce 0%
18/03/10 08:40:20 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 08:40:20 INFO mapreduce.Job: Job job_1514521302404_0036 completed successfully
18/03/10 08:40:20 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=304798
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=254
    HDFS: Number of bytes written=3933864
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=9198
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=9198
    Total vcore-milliseconds taken by all map tasks=9198
    Total megabyte-milliseconds taken by all map tasks=9418752
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=254
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=166
    CPU time spent (ms)=6810
    Physical memory (bytes) snapshot=602050560
    Virtual memory (bytes) snapshot=3166998528
    Total committed heap usage (bytes)=736100352
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=3933864
18/03/10 08:40:20 INFO mapreduce.ImportJobBase: Transferred 3.7516 MB in 15.638 seconds (245.6628 KB/sec)
18/03/10 08:40:20 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0036/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items
-rw-r--r--   1 cloudera cloudera          0 2018-03-10 08:40 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera      1.9 M 2018-03-10 08:40 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000.avro
-rw-r--r--   1 cloudera cloudera      1.9 M 2018-03-10 08:40 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001.avro
~~~

~~~
[cloudera@quickstart ~]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop_import/retail_db/order_items/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{
  "type" : "record",
  "name" : "order_items",
  "doc" : "Sqoop import of order_items",
  "fields" : [ {
    "name" : "order_item_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "order_item_id",
    "sqlType" : "4"
  }, {
    "name" : "order_item_order_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "order_item_order_id",
    "sqlType" : "4"
  }, {
    "name" : "order_item_product_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "order_item_product_id",
    "sqlType" : "4"
  }, {
    "name" : "order_item_quantity",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "order_item_quantity",
    "sqlType" : "-6"
  }, {
    "name" : "order_item_subtotal",
    "type" : [ "null", "float" ],
    "default" : null,
    "columnName" : "order_item_subtotal",
    "sqlType" : "7"
  }, {
    "name" : "order_item_product_price",
    "type" : [ "null", "float" ],
    "default" : null,
    "columnName" : "order_item_product_price",
    "sqlType" : "7"
  } ],
  "tableName" : "order_items"
}
~~~

~~~
[cloudera@quickstart ~]$ avro-tools tojson hdfs://quickstart.cloudera/user/cloudera/sqoop_import/retail_db/order_items/part-m-00000.avro | more
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{"order_item_id":{"int":1},"order_item_order_id":{"int":1},"order_item_product_id":{"int":957},"order_item_quantity":{"int":1},"order_item_subtotal":{"float":299.98},"order_item_product_price":{"float":29
9.98}}
{"order_item_id":{"int":2},"order_item_order_id":{"int":2},"order_item_product_id":{"int":1073},"order_item_quantity":{"int":1},"order_item_subtotal":{"float":199.99},"order_item_product_price":{"float":1
99.99}}
{"order_item_id":{"int":3},"order_item_order_id":{"int":2},"order_item_product_id":{"int":502},"order_item_quantity":{"int":5},"order_item_subtotal":{"float":250.0},"order_item_product_price":{"float":50.
0}}
{"order_item_id":{"int":4},"order_item_order_id":{"int":2},"order_item_product_id":{"int":403},"order_item_quantity":{"int":1},"order_item_subtotal":{"float":129.99},"order_item_product_price":{"float":12
9.99}}
~~~

### Usage of --as-parquetfile

* Import order_items table data from MySQL to HDFS in parquet file format 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items \
--delete-target-dir \
--as-parquetfile \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 08:46:24 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 08:46:24 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 08:46:25 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 08:46:25 INFO tool.CodeGenTool: Beginning code generation
18/03/10 08:46:25 INFO tool.CodeGenTool: Will generate java class as codegen_order_items
18/03/10 08:46:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:46:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:46:25 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/6825ec74fb04a8dbac483e6186f522b1/codegen_order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 08:46:26 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/6825ec74fb04a8dbac483e6186f522b1/codegen_order_items.jar
18/03/10 08:46:27 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items deleted.
18/03/10 08:46:27 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 08:46:27 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 08:46:27 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 08:46:27 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 08:46:27 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/10 08:46:27 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 08:46:27 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 08:46:27 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:46:27 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/10 08:46:28 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 08:46:28 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 08:46:29 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 08:46:29 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/10 08:46:29 INFO db.IntegerSplitter: Split size: 86098; Num splits: 2 from: 1 to: 172198
18/03/10 08:46:29 INFO mapreduce.JobSubmitter: number of splits:2
18/03/10 08:46:29 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0037
18/03/10 08:46:29 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0037
18/03/10 08:46:29 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0037/
18/03/10 08:46:29 INFO mapreduce.Job: Running job: job_1514521302404_0037
18/03/10 08:46:35 INFO mapreduce.Job: Job job_1514521302404_0037 running in uber mode : false
18/03/10 08:46:35 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 08:46:43 INFO mapreduce.Job:  map 50% reduce 0%
18/03/10 08:46:44 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 08:46:44 INFO mapreduce.Job: Job job_1514521302404_0037 completed successfully
18/03/10 08:46:45 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=306602
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=21740
    HDFS: Number of bytes written=1725069
    HDFS: Number of read operations=136
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=20
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=10229
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=10229
    Total vcore-milliseconds taken by all map tasks=10229
    Total megabyte-milliseconds taken by all map tasks=10474496
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=254
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=212
    CPU time spent (ms)=5790
    Physical memory (bytes) snapshot=624320512
    Virtual memory (bytes) snapshot=3156594688
    Total committed heap usage (bytes)=736100352
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=0
18/03/10 08:46:45 INFO mapreduce.ImportJobBase: Transferred 1.6452 MB in 16.6528 seconds (101.1626 KB/sec)
18/03/10 08:46:45 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0037/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items
drwxr-xr-x   - cloudera cloudera          0 2018-03-10 08:46 /user/cloudera/sqoop_import/retail_db/order_items/.metadata
-rw-r--r--   1 cloudera cloudera        206 2018-03-10 08:46 /user/cloudera/sqoop_import/retail_db/order_items/.metadata/descriptor.properties
-rw-r--r--   1 cloudera cloudera      1.1 K 2018-03-10 08:46 /user/cloudera/sqoop_import/retail_db/order_items/.metadata/schema.avsc
drwxr-xr-x   - cloudera cloudera          0 2018-03-10 08:46 /user/cloudera/sqoop_import/retail_db/order_items/.metadata/schemas
-rw-r--r--   1 cloudera cloudera      1.1 K 2018-03-10 08:46 /user/cloudera/sqoop_import/retail_db/order_items/.metadata/schemas/1.avsc
drwxr-xr-x   - cloudera cloudera          0 2018-03-10 08:46 /user/cloudera/sqoop_import/retail_db/order_items/.signals
-rw-r--r--   1 cloudera cloudera          0 2018-03-10 08:46 /user/cloudera/sqoop_import/retail_db/order_items/.signals/unbounded
-rw-r--r--   1 cloudera cloudera    823.9 K 2018-03-10 08:46 /user/cloudera/sqoop_import/retail_db/order_items/01117a60-715c-47aa-ae25-23435076208c.parquet
-rw-r--r--   1 cloudera cloudera    855.8 K 2018-03-10 08:46 /user/cloudera/sqoop_import/retail_db/order_items/5e5e9888-1437-4dac-b684-cd7b95ccd2ba.parquet
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools schema --detailed hdfs://quickstart.cloudera/user/cloudera/sqoop_import/retail_db/order_items/01117a60-715c-47aa-ae25-23435076208c.parquet
message order_items {
  optional int32 order_item_id;
  optional int32 order_item_order_id;
  optional int32 order_item_product_id;
  optional int32 order_item_quantity;
  optional float order_item_subtotal;
  optional float order_item_product_price;
}

creator: parquet-mr version 1.5.0-cdh5.12.0 (build ${buildNumber})
extra: parquet.avro.schema = {"type":"record","name":"order_items","doc":"Sqoop import of order_items","fields":[{"name":"order_item_id","type":["null","int"],"default":null,"columnName":"order_item_id","sqlType":"4"},{"name":"order_item_order_id","type":["null","int"],"default":null,"columnName":"order_item_order_id","sqlType":"4"},{"name":"order_item_product_id","type":["null","int"],"default":null,"columnName":"order_item_product_id","sqlType":"4"},{"name":"order_item_quantity","type":["null","int"],"default":null,"columnName":"order_item_quantity","sqlType":"-6"},{"name":"order_item_subtotal","type":["null","float"],"default":null,"columnName":"order_item_subtotal","sqlType":"7"},{"name":"order_item_product_price","type":["null","float"],"default":null,"columnName":"order_item_product_price","sqlType":"7"}],"tableName":"order_items"}

file schema: order_items
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
order_item_id: OPTIONAL INT32 R:0 D:1
order_item_order_id: OPTIONAL INT32 R:0 D:1
order_item_product_id: OPTIONAL INT32 R:0 D:1
order_item_quantity: OPTIONAL INT32 R:0 D:1
order_item_subtotal: OPTIONAL FLOAT R:0 D:1
order_item_product_price: OPTIONAL FLOAT R:0 D:1

row group 1: RC:86099 TS:848955
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
order_item_id:  INT32 SNAPPY DO:0 FPO:4 SZ:344464/344443/1.00 VC:86099 ENC:PLAIN,BIT_PACKED,RLE
order_item_order_id:  INT32 SNAPPY DO:0 FPO:344468 SZ:269877/276767/1.03 VC:86099 ENC:PLAIN_DICTIONARY,BIT_PACKED,RLE
order_item_product_id:  INT32 SNAPPY DO:0 FPO:614345 SZ:65063/65028/1.00 VC:86099 ENC:PLAIN_DICTIONARY,BIT_PACKED,RLE
order_item_quantity:  INT32 SNAPPY DO:0 FPO:679408 SZ:32504/32480/1.00 VC:86099 ENC:PLAIN_DICTIONARY,BIT_PACKED,RLE
order_item_subtotal:  FLOAT SNAPPY DO:0 FPO:711912 SZ:76085/76072/1.00 VC:86099 ENC:PLAIN_DICTIONARY,BIT_PACKED,RLE
order_item_product_price:  FLOAT SNAPPY DO:0 FPO:787997 SZ:54179/54165/1.00 VC:86099 ENC:PLAIN_DICTIONARY,BIT_PACKED,RLE

~~~

~~~
[cloudera@quickstart ~]$ parquet-tools cat --json hdfs://quickstart.cloudera/user/cloudera/sqoop_import/retail_db/order_items/01117a60-715c-47aa-ae25-23435076208c.parquet | more
{"order_item_id":1,"order_item_order_id":1,"order_item_product_id":957,"order_item_quantity":1,"order_item_subtotal":299.98,"order_item_product_price":299.98}
{"order_item_id":2,"order_item_order_id":2,"order_item_product_id":1073,"order_item_quantity":1,"order_item_subtotal":199.99,"order_item_product_price":199.99}
{"order_item_id":3,"order_item_order_id":2,"order_item_product_id":502,"order_item_quantity":5,"order_item_subtotal":250.0,"order_item_product_price":50.0}
{"order_item_id":4,"order_item_order_id":2,"order_item_product_id":403,"order_item_quantity":1,"order_item_subtotal":129.99,"order_item_product_price":129.99}
{"order_item_id":5,"order_item_order_id":4,"order_item_product_id":897,"order_item_quantity":2,"order_item_subtotal":49.98,"order_item_product_price":24.99}
{"order_item_id":6,"order_item_order_id":4,"order_item_product_id":365,"order_item_quantity":5,"order_item_subtotal":299.95,"order_item_product_price":59.99}
{"order_item_id":7,"order_item_order_id":4,"order_item_product_id":502,"order_item_quantity":3,"order_item_subtotal":150.0,"order_item_product_price":50.0}
~~~
