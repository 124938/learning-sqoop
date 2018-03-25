## Examples - Import data (Using incremental load)

### Approach -> 1 : Usage of --table with --where & --append

* **Import orders table with `Jan 2014` data from MySQL to HDFS**

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

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0026/
~~~

* **Verify orders data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/orders
-rw-r--r--   1 cloudera cloudera    209.3 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     43.2 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
~~~

### Approach -> 2 : Usage of --query with --split-by & --append

* **Import orders table with `Feb 2014` data from MySQL to HDFS**

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

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0027/
~~~

* **Verify orders data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/orders
-rw-r--r--   1 cloudera cloudera    209.3 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     43.2 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera    203.5 K 2018-03-10 04:51 /user/cloudera/sqoop_import/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     36.8 K 2018-03-10 04:51 /user/cloudera/sqoop_import/retail_db/orders/part-m-00003
~~~

### Approach -> 3 : Usage of --table with --check-column, --incremental & --last-value (New records only)

* **Important Notes:**
  * --incremental append means only new records will get imported to HDFS

* **Import orders table with greater then `28-Feb-2014` data from MySQL to HDFS**

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

* **Verify map reduce job**

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0028/
~~~

* **Verify orders data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/orders
-rw-r--r--   1 cloudera cloudera    209.3 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     43.2 K 2018-03-10 04:43 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera    203.5 K 2018-03-10 04:51 /user/cloudera/sqoop_import/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     36.8 K 2018-03-10 04:51 /user/cloudera/sqoop_import/retail_db/orders/part-m-00003
-rw-r--r--   1 cloudera cloudera    711.3 K 2018-03-10 05:31 /user/cloudera/sqoop_import/retail_db/orders/part-m-00004
-rw-r--r--   1 cloudera cloudera    427.5 K 2018-03-10 05:31 /user/cloudera/sqoop_import/retail_db/orders/part-m-00005
~~~

### Approach -> 4 : Usage of --table with --check-column, --incremental, --last-value & --merge-key (New & Updated records)

* **Important Notes:**
  * --incremental lastmodified means only new & updated records will get imported to HDFS

* **Login to MySql:**
~~~
[cloudera@quickstart ~]$ mysql -h quickstart.cloudera -u retail_dba -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 434
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
mysql> create table ticker(
 id int not null auto_increment,
 name varchar(100) not null,
 price float not null,
 last_updated_at DATETIME not null,
 primary key (id)
);

mysql>insert into ticker (name, price, last_updated_at) values ('T1', 10.20, '2018-01-01 01:01:01');
insert into ticker (name, price, last_updated_at) values ('T2', 20.40, '2018-01-01 01:01:01');
insert into ticker (name, price, last_updated_at) values ('T3', 25.65, '2018-01-02 01:01:01');
insert into ticker (name, price, last_updated_at) values ('T4', 15.20, '2018-01-03 01:01:01');
insert into ticker (name, price, last_updated_at) values ('T5', 45.40, '2018-01-04 01:01:01');
insert into ticker (name, price, last_updated_at) values ('T6', 87.65, '2018-01-05 01:01:01');

mysql> select * from ticker;
+----+------+-------+---------------------+
| id | name | price | last_updated_at     |
+----+------+-------+---------------------+
|  1 | T1   |  10.2 | 2018-01-01 01:01:01 |
|  2 | T2   |  20.4 | 2018-01-01 01:01:01 |
|  3 | T3   | 25.65 | 2018-01-02 01:01:01 |
|  4 | T4   |  15.2 | 2018-01-03 01:01:01 |
|  5 | T5   |  45.4 | 2018-01-04 01:01:01 |
|  6 | T6   | 87.65 | 2018-01-05 01:01:01 |
+----+------+-------+---------------------+
~~~

* **Import ticker table with greater then `2018-01-03 00:00:00` data from MySQL to HDFS** 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table ticker \
--check-column last_updated_at \
--incremental lastmodified \
--last-value '2018-01-03 00:00:00' \
--merge-key id \
--target-dir /user/cloudera/sqoop_import/retail_db/ticker \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/11 07:57:20 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/11 07:57:20 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/11 07:57:20 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/11 07:57:20 INFO tool.CodeGenTool: Beginning code generation
18/03/11 07:57:20 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `ticker` AS t LIMIT 1
18/03/11 07:57:20 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `ticker` AS t LIMIT 1
18/03/11 07:57:20 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/486a933b861bfae8e8a43841ce8a7354/ticker.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 07:57:21 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/486a933b861bfae8e8a43841ce8a7354/ticker.jar
18/03/11 07:57:22 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `ticker` AS t LIMIT 1
18/03/11 07:57:22 INFO tool.ImportTool: Incremental import based on column `last_updated_at`
18/03/11 07:57:22 INFO tool.ImportTool: Lower bound value: '2018-01-03 00:00:00'
18/03/11 07:57:22 INFO tool.ImportTool: Upper bound value: '2018-03-11 07:57:22.0'
18/03/11 07:57:22 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/11 07:57:22 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/11 07:57:22 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/11 07:57:22 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/11 07:57:22 INFO mapreduce.ImportJobBase: Beginning import of ticker
18/03/11 07:57:22 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 07:57:22 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/11 07:57:22 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/11 07:57:22 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 07:57:23 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 07:57:23 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`id`), MAX(`id`) FROM `ticker` WHERE ( `last_updated_at` >= '2018-01-03 00:00:00' AND `last_updated_at` < '2018-03-11 07:57:22.0' )
18/03/11 07:57:23 INFO db.IntegerSplitter: Split size: 1; Num splits: 2 from: 4 to: 6
18/03/11 07:57:23 INFO mapreduce.JobSubmitter: number of splits:3
18/03/11 07:57:23 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0059
18/03/11 07:57:23 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0059
18/03/11 07:57:23 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0059/
18/03/11 07:57:23 INFO mapreduce.Job: Running job: job_1514521302404_0059
18/03/11 07:57:30 INFO mapreduce.Job: Job job_1514521302404_0059 running in uber mode : false
18/03/11 07:57:30 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 07:57:36 INFO mapreduce.Job:  map 33% reduce 0%
18/03/11 07:57:37 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 07:57:37 INFO mapreduce.Job: Job job_1514521302404_0059 completed successfully
18/03/11 07:57:37 INFO mapreduce.Job: Counters: 31
    File System Counters
        FILE: Number of bytes read=0
        FILE: Number of bytes written=457725
        FILE: Number of read operations=0
        FILE: Number of large read operations=0
        FILE: Number of write operations=0
        HDFS: Number of bytes read=295
        HDFS: Number of bytes written=97
        HDFS: Number of read operations=12
        HDFS: Number of large read operations=0
        HDFS: Number of write operations=6
    Job Counters 
        Killed map tasks=1
        Launched map tasks=3
        Other local map tasks=3
        Total time spent by all maps in occupied slots (ms)=10757
        Total time spent by all reduces in occupied slots (ms)=0
        Total time spent by all map tasks (ms)=10757
        Total vcore-milliseconds taken by all map tasks=10757
        Total megabyte-milliseconds taken by all map tasks=11015168
    Map-Reduce Framework
        Map input records=3
        Map output records=3
        Input split bytes=295
        Spilled Records=0
        Failed Shuffles=0
        Merged Map outputs=0
        GC time elapsed (ms)=86
        CPU time spent (ms)=2940
        Physical memory (bytes) snapshot=639643648
        Virtual memory (bytes) snapshot=4733132800
        Total committed heap usage (bytes)=662175744
    File Input Format Counters 
        Bytes Read=0
    File Output Format Counters 
        Bytes Written=97
18/03/11 07:57:37 INFO mapreduce.ImportJobBase: Transferred 97 bytes in 14.7105 seconds (6.5939 bytes/sec)
18/03/11 07:57:37 INFO mapreduce.ImportJobBase: Retrieved 3 records.
18/03/11 07:57:37 INFO tool.ImportTool: Final destination exists, will run merge job.
18/03/11 07:57:37 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 07:57:37 WARN mapreduce.ExportJobBase: IOException checking input file header: java.io.EOFException
18/03/11 07:57:37 INFO Configuration.deprecation: mapred.output.key.class is deprecated. Instead, use mapreduce.job.output.key.class
18/03/11 07:57:37 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 07:57:37 INFO input.FileInputFormat: Total input paths to process : 4
18/03/11 07:57:38 INFO mapreduce.JobSubmitter: number of splits:4
18/03/11 07:57:38 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0060
18/03/11 07:57:38 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0060
18/03/11 07:57:38 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0060/
18/03/11 07:57:38 INFO mapreduce.Job: Running job: job_1514521302404_0060
18/03/11 07:57:43 INFO mapreduce.Job: Job job_1514521302404_0060 running in uber mode : false
18/03/11 07:57:43 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 07:57:49 INFO mapreduce.Job:  map 25% reduce 0%
18/03/11 07:57:50 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 07:57:51 INFO mapreduce.Job:  map 75% reduce 0%
18/03/11 07:57:52 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 07:57:54 INFO mapreduce.Job:  map 100% reduce 100%
18/03/11 07:57:54 INFO mapreduce.Job: Job job_1514521302404_0060 completed successfully
18/03/11 07:57:54 INFO mapreduce.Job: Counters: 51
    File System Counters
        FILE: Number of bytes read=123
        FILE: Number of bytes written=764145
        FILE: Number of read operations=0
        FILE: Number of large read operations=0
        FILE: Number of write operations=0
        HDFS: Number of bytes read=760
        HDFS: Number of bytes written=97
        HDFS: Number of read operations=15
        HDFS: Number of large read operations=0
        HDFS: Number of write operations=2
    Job Counters 
        Killed map tasks=1
        Launched map tasks=4
        Launched reduce tasks=1
        Other local map tasks=1
        Data-local map tasks=3
        Total time spent by all maps in occupied slots (ms)=10775
        Total time spent by all reduces in occupied slots (ms)=2345
        Total time spent by all map tasks (ms)=10775
        Total time spent by all reduce tasks (ms)=2345
        Total vcore-milliseconds taken by all map tasks=10775
        Total vcore-milliseconds taken by all reduce tasks=2345
        Total megabyte-milliseconds taken by all map tasks=11033600
        Total megabyte-milliseconds taken by all reduce tasks=2401280
    Map-Reduce Framework
        Map input records=3
        Map output records=3
        Map output bytes=111
        Map output materialized bytes=141
        Input split bytes=663
        Combine input records=0
        Combine output records=0
        Reduce input groups=3
        Reduce shuffle bytes=141
        Reduce input records=3
        Reduce output records=3
        Spilled Records=6
        Shuffled Maps =4
        Failed Shuffles=0
        Merged Map outputs=4
        GC time elapsed (ms)=135
        CPU time spent (ms)=2350
        Physical memory (bytes) snapshot=1367683072
        Virtual memory (bytes) snapshot=7850708992
        Total committed heap usage (bytes)=1323302912
    Shuffle Errors
        BAD_ID=0
        CONNECTION=0
        IO_ERROR=0
        WRONG_LENGTH=0
        WRONG_MAP=0
        WRONG_REDUCE=0
    File Input Format Counters 
        Bytes Read=97
    File Output Format Counters 
        Bytes Written=97
18/03/11 07:57:54 INFO tool.ImportTool: Incremental import complete! To run another incremental import of all data following this import, supply the following arguments:
18/03/11 07:57:54 INFO tool.ImportTool:  --incremental lastmodified
18/03/11 07:57:54 INFO tool.ImportTool:   --check-column last_updated_at
18/03/11 07:57:54 INFO tool.ImportTool:   --last-value 2018-03-11 07:57:22.0
18/03/11 07:57:54 INFO tool.ImportTool: (Consider saving this with 'sqoop job --create')
~~~

* **Verify ticker data under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls /user/cloudera/sqoop_import/retail_db/ticker
Found 2 items
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 07:57 /user/cloudera/sqoop_import/retail_db/ticker/_SUCCESS
-rw-r--r--   1 cloudera cloudera         97 2018-03-11 07:57 /user/cloudera/sqoop_import/retail_db/ticker/part-r-00000

[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop_import/retail_db/ticker/part-r-00000
4,T4,15.2,2018-01-03 01:01:01.0
5,T5,45.4,2018-01-04 01:01:01.0
6,T6,87.65,2018-01-05 01:01:01.0
~~~
