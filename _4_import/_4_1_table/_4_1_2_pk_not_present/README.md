## Examples - Import data from table (Primary Key NOT present in table)

### Pre-Requisite

* **Login to mysql**

~~~
[cloudera@quickstart ~]$ mysql -h quickstart.cloudera -u retail_dba -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 155
Server version: 5.1.73 Source distribution

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> use retail_db;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
~~~

* **Create table without primary key**

~~~
mysql> create table order_items_nopk as select * from order_items;
Query OK, 172198 rows affected (0.77 sec)
Records: 172198  Duplicates: 0  Warnings: 0

mysql> describe order_items_nopk;
+--------------------------+------------+------+-----+---------+-------+
| Field                    | Type       | Null | Key | Default | Extra |
+--------------------------+------------+------+-----+---------+-------+
| order_item_id            | int(11)    | NO   |     | 0       |       |
| order_item_order_id      | int(11)    | NO   |     | NULL    |       |
| order_item_product_id    | int(11)    | NO   |     | NULL    |       |
| order_item_quantity      | tinyint(4) | NO   |     | NULL    |       |
| order_item_subtotal      | float      | NO   |     | NULL    |       |
| order_item_product_price | float      | NO   |     | NULL    |       |
+--------------------------+------------+------+-----+---------+-------+
6 rows in set (0.00 sec)
~~~

### Sample import of table which does not contain primary key

* **Import order_items_nopk table from MySQL to HDFS**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items_nopk \
--warehouse-dir /user/cloudera/sqoop_import/retail_db

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/04 06:09:56 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/04 06:09:56 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/04 06:09:56 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/04 06:09:56 INFO tool.CodeGenTool: Beginning code generation
18/03/04 06:09:56 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items_nopk` AS t LIMIT 1
18/03/04 06:09:56 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items_nopk` AS t LIMIT 1
18/03/04 06:09:56 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/cb70bccfbeb5ee447f5372986de5f993/order_items_nopk.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/04 06:09:57 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/cb70bccfbeb5ee447f5372986de5f993/order_items_nopk.jar
18/03/04 06:09:57 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/04 06:09:57 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/04 06:09:57 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/04 06:09:57 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/04 06:09:57 ERROR tool.ImportTool: Import failed: No primary key could be found for table order_items_nopk. Please specify one with --split-by or perform a sequential import with '-m 1'.
~~~

* **Important Notes:**
  * Above command is getting failed due to primary key is not present on table
  * Possible Solutions:
    * Use --num-mappers 1 in above command
    * Use --split-by argument 

### Usage of --split-by (for numeric field)

* **Important Notes:**
  * Value specified in --split-by column should be sequence generated OR evenly incremented
  * Value specified in --split-by column should not contain null values
  * Column specified in --split-by column should be indexed at DB level else importing data may have performance impact

* **Import order_items_nopk table from MySQL to HDFS**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items_nopk \
--split-by order_item_id \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items_nopk

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/04 06:36:23 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/04 06:36:23 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/04 06:36:23 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/04 06:36:23 INFO tool.CodeGenTool: Beginning code generation
18/03/04 06:36:23 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items_nopk` AS t LIMIT 1
18/03/04 06:36:23 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items_nopk` AS t LIMIT 1
18/03/04 06:36:23 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/1af3438c146219d6945d29524dd77080/order_items_nopk.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/04 06:36:24 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/1af3438c146219d6945d29524dd77080/order_items_nopk.jar
18/03/04 06:36:24 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/04 06:36:24 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/04 06:36:24 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/04 06:36:24 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/04 06:36:24 INFO mapreduce.ImportJobBase: Beginning import of order_items_nopk
18/03/04 06:36:24 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/04 06:36:24 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/04 06:36:25 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/04 06:36:25 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/04 06:36:26 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/04 06:36:26 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items_nopk`
18/03/04 06:36:26 INFO db.IntegerSplitter: Split size: 43049; Num splits: 4 from: 1 to: 172198
18/03/04 06:36:26 INFO mapreduce.JobSubmitter: number of splits:4
18/03/04 06:36:26 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0016
18/03/04 06:36:26 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0016
18/03/04 06:36:26 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0016/
18/03/04 06:36:26 INFO mapreduce.Job: Running job: job_1514521302404_0016
18/03/04 06:36:33 INFO mapreduce.Job: Job job_1514521302404_0016 running in uber mode : false
18/03/04 06:36:33 INFO mapreduce.Job:  map 0% reduce 0%
18/03/04 06:36:39 INFO mapreduce.Job:  map 25% reduce 0%
18/03/04 06:36:41 INFO mapreduce.Job:  map 50% reduce 0%
18/03/04 06:36:42 INFO mapreduce.Job:  map 100% reduce 0%
18/03/04 06:36:42 INFO mapreduce.Job: Job job_1514521302404_0016 completed successfully
18/03/04 06:36:42 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=607824
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=512
    HDFS: Number of bytes written=5408880
    HDFS: Number of read operations=16
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=8
  Job Counters 
    Launched map tasks=4
    Other local map tasks=4
    Total time spent by all maps in occupied slots (ms)=17345
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=17345
    Total vcore-milliseconds taken by all map tasks=17345
    Total megabyte-milliseconds taken by all map tasks=17761280
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=512
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=208
    CPU time spent (ms)=9340
    Physical memory (bytes) snapshot=1072484352
    Virtual memory (bytes) snapshot=6303391744
    Total committed heap usage (bytes)=984088576
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=5408880
18/03/04 06:36:42 INFO mapreduce.ImportJobBase: Transferred 5.1583 MB in 16.8611 seconds (313.2714 KB/sec)
18/03/04 06:36:42 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
~~~

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0016/
~~~

* **Verify order_items_nopk data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items_nopk

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 06:36 /user/cloudera/sqoop_import/retail_db/order_items_nopk/_SUCCESS
-rw-r--r--   1 cloudera cloudera      1.2 M 2018-03-04 06:36 /user/cloudera/sqoop_import/retail_db/order_items_nopk/part-m-00000
-rw-r--r--   1 cloudera cloudera      1.3 M 2018-03-04 06:36 /user/cloudera/sqoop_import/retail_db/order_items_nopk/part-m-00001
-rw-r--r--   1 cloudera cloudera      1.3 M 2018-03-04 06:36 /user/cloudera/sqoop_import/retail_db/order_items_nopk/part-m-00002
-rw-r--r--   1 cloudera cloudera      1.3 M 2018-03-04 06:36 /user/cloudera/sqoop_import/retail_db/order_items_nopk/part-m-00003
~~~

### Usage of --split-by (for non-numeric field)

* **Import orders table from MySQL to HDFS (using order_status column as --split-by field)**

~~~
[cloudera@quickstart ~]$ sqoop-import \
-Dorg.apache.sqoop.splitter.allow_text_splitter=true \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--split-by order_status \
--target-dir /user/cloudera/sqoop_import/retail_db/orders_non_numeric

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/04 07:04:47 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/04 07:04:47 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/04 07:04:47 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/04 07:04:47 INFO tool.CodeGenTool: Beginning code generation
18/03/04 07:04:47 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/04 07:04:47 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/04 07:04:47 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/3e8dd4c3a6902a02a7e1c99a411f47fc/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/04 07:04:49 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/3e8dd4c3a6902a02a7e1c99a411f47fc/orders.jar
18/03/04 07:04:49 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/04 07:04:49 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/04 07:04:49 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/04 07:04:49 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/04 07:04:49 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/04 07:04:49 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/04 07:04:49 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/04 07:04:49 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/04 07:04:49 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/04 07:04:51 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/04 07:04:51 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_status`), MAX(`order_status`) FROM `orders`
18/03/04 07:04:51 WARN db.TextSplitter: Generating splits for a textual index column.
18/03/04 07:04:51 WARN db.TextSplitter: If your database sorts in a case-insensitive order, this may result in a partial import or duplicate records.
18/03/04 07:04:51 WARN db.TextSplitter: You are strongly encouraged to choose an integral split column.
18/03/04 07:04:51 INFO mapreduce.JobSubmitter: number of splits:5
18/03/04 07:04:51 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0017
18/03/04 07:04:51 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0017
18/03/04 07:04:51 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0017/
18/03/04 07:04:51 INFO mapreduce.Job: Running job: job_1514521302404_0017
18/03/04 07:04:57 INFO mapreduce.Job: Job job_1514521302404_0017 running in uber mode : false
18/03/04 07:04:57 INFO mapreduce.Job:  map 0% reduce 0%
18/03/04 07:05:03 INFO mapreduce.Job:  map 40% reduce 0%
18/03/04 07:05:05 INFO mapreduce.Job:  map 60% reduce 0%
18/03/04 07:05:07 INFO mapreduce.Job:  map 100% reduce 0%
18/03/04 07:05:07 INFO mapreduce.Job: Job job_1514521302404_0017 completed successfully
18/03/04 07:05:08 INFO mapreduce.Job: Counters: 31
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=760005
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=736
    HDFS: Number of bytes written=2999944
    HDFS: Number of read operations=20
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=10
  Job Counters 
    Killed map tasks=1
    Launched map tasks=5
    Other local map tasks=5
    Total time spent by all maps in occupied slots (ms)=20312
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=20312
    Total vcore-milliseconds taken by all map tasks=20312
    Total megabyte-milliseconds taken by all map tasks=20799488
  Map-Reduce Framework
    Map input records=68883
    Map output records=68883
    Input split bytes=736
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=222
    CPU time spent (ms)=8760
    Physical memory (bytes) snapshot=1087127552
    Virtual memory (bytes) snapshot=7886979072
    Total committed heap usage (bytes)=1015021568
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=2999944
18/03/04 07:05:08 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 18.2169 seconds (160.8197 KB/sec)
18/03/04 07:05:08 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
~~~

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0017/
~~~

* **Verify orders_non_numeric data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/orders_non_numeric

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 07:05 /user/cloudera/sqoop_import/retail_db/orders_non_numeric/_SUCCESS
-rw-r--r--   1 cloudera cloudera      1.3 M 2018-03-04 07:05 /user/cloudera/sqoop_import/retail_db/orders_non_numeric/part-m-00000
-rw-r--r--   1 cloudera cloudera          0 2018-03-04 07:05 /user/cloudera/sqoop_import/retail_db/orders_non_numeric/part-m-00001
-rw-r--r--   1 cloudera cloudera    151.9 K 2018-03-04 07:05 /user/cloudera/sqoop_import/retail_db/orders_non_numeric/part-m-00002
-rw-r--r--   1 cloudera cloudera      1.4 M 2018-03-04 07:05 /user/cloudera/sqoop_import/retail_db/orders_non_numeric/part-m-00003
-rw-r--r--   1 cloudera cloudera     74.5 K 2018-03-04 07:05 /user/cloudera/sqoop_import/retail_db/orders_non_numeric/part-m-00004

[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop_import/retail_db/orders_non_numeric/part-m-00000 | more
1,2013-07-25 00:00:00.0,11599,CLOSED
3,2013-07-25 00:00:00.0,12111,COMPLETE
4,2013-07-25 00:00:00.0,8827,CLOSED
5,2013-07-25 00:00:00.0,11318,COMPLETE
6,2013-07-25 00:00:00.0,7130,COMPLETE
7,2013-07-25 00:00:00.0,4530,COMPLETE
12,2013-07-25 00:00:00.0,1837,CLOSED
15,2013-07-25 00:00:00.0,2568,COMPLETE
17,2013-07-25 00:00:00.0,2667,COMPLETE
~~~

### Usage of --autoreset-to-one-mapper

* **Important Notes:**
  * --autoreset-to-one-mapper is equal to --num-mappers 1 i.e. it will automatically reset number of mapper to 1, in case of table does not contain primary key 

* **Import order_items_nopk table from MySQL to HDFS**

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table order_items_nopk \
--target-dir /user/cloudera/sqoop_import/retail_db/order_items_nopk \
--delete-target-dir \
--autoreset-to-one-mapper

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/04 07:13:42 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/04 07:13:42 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/04 07:13:42 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/04 07:13:42 INFO tool.CodeGenTool: Beginning code generation
18/03/04 07:13:42 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items_nopk` AS t LIMIT 1
18/03/04 07:13:42 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items_nopk` AS t LIMIT 1
18/03/04 07:13:42 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/eb234c9202feb458bfaf60ceccce35ea/order_items_nopk.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/04 07:13:44 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/eb234c9202feb458bfaf60ceccce35ea/order_items_nopk.jar
18/03/04 07:13:44 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_items_nopk deleted.
18/03/04 07:13:44 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/04 07:13:44 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/04 07:13:44 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/04 07:13:44 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/04 07:13:44 WARN manager.SqlManager: Split by column not provided or can't be inferred.  Resetting to one mapper
18/03/04 07:13:44 INFO mapreduce.ImportJobBase: Beginning import of order_items_nopk
18/03/04 07:13:44 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/04 07:13:44 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/04 07:13:44 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/04 07:13:44 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/04 07:13:45 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/04 07:13:45 INFO mapreduce.JobSubmitter: number of splits:1
18/03/04 07:13:45 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0018
18/03/04 07:13:46 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0018
18/03/04 07:13:46 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0018/
18/03/04 07:13:46 INFO mapreduce.Job: Running job: job_1514521302404_0018
18/03/04 07:13:52 INFO mapreduce.Job: Job job_1514521302404_0018 running in uber mode : false
18/03/04 07:13:52 INFO mapreduce.Job:  map 0% reduce 0%
18/03/04 07:13:57 INFO mapreduce.Job:  map 100% reduce 0%
18/03/04 07:13:57 INFO mapreduce.Job: Job job_1514521302404_0018 completed successfully
18/03/04 07:13:57 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=151657
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=87
    HDFS: Number of bytes written=5408880
    HDFS: Number of read operations=4
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=2
  Job Counters 
    Launched map tasks=1
    Other local map tasks=1
    Total time spent by all maps in occupied slots (ms)=3271
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=3271
    Total vcore-milliseconds taken by all map tasks=3271
    Total megabyte-milliseconds taken by all map tasks=3349504
  Map-Reduce Framework
    Map input records=172198
    Map output records=172198
    Input split bytes=87
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=37
    CPU time spent (ms)=3330
    Physical memory (bytes) snapshot=347795456
    Virtual memory (bytes) snapshot=1592016896
    Total committed heap usage (bytes)=369623040
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=5408880
18/03/04 07:13:57 INFO mapreduce.ImportJobBase: Transferred 5.1583 MB in 12.7615 seconds (413.9082 KB/sec)
18/03/04 07:13:57 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
~~~

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0018/
~~~

* **Verify order_items_nopk data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items_nopk

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 07:13 /user/cloudera/sqoop_import/retail_db/order_items_nopk/_SUCCESS
-rw-r--r--   1 cloudera cloudera      5.2 M 2018-03-04 07:13 /user/cloudera/sqoop_import/retail_db/order_items_nopk/part-m-00000
~~~
