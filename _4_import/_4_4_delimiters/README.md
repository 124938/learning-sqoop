## Examples - Import data (Using delimiters & handling null values)

### Pre-Requisite

* Login to mysql

~~~
[cloudera@quickstart ~]$ mysql -h quickstart.cloudera -u retail_dba -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 268
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

* Create table with primary key

~~~
mysql> create table emp(
 id int not null auto_increment,
 f_name varchar(100) not null,
 l_name varchar(100) not null,
 manager varchar(100),
 dept_id int,
 primary key (id)
);
Query OK, 0 rows affected (0.01 sec)

mysql> insert into emp(f_name, l_name, manager, dept_id) values('shreyash','limbhetwala',NULL,NULL);
Query OK, 1 row affected (0.00 sec)

mysql> insert into emp(f_name, l_name, manager, dept_id) values('bonika','limbhetwala',NULL,NULL);
Query OK, 1 row affected (0.01 sec)

mysql> insert into emp(f_name, l_name, manager, dept_id) values('dherya','limbhetwala','boni',1);
Query OK, 1 row affected (0.01 sec)

mysql> insert into emp(f_name, l_name, manager, dept_id) values('hrisha','limbhetwala','shrey',1);
Query OK, 1 row affected (0.00 sec)

mysql> select * from emp;
+----+----------+-------------+---------+---------+
| id | f_name   | l_name      | manager | dept_id |
+----+----------+-------------+---------+---------+
|  1 | shreyash | limbhetwala | NULL    |    NULL |
|  2 | bonika   | limbhetwala | NULL    |    NULL |
|  3 | dherya   | limbhetwala | boni    |       1 |
|  4 | hrisha   | limbhetwala | shrey   |       1 |
+----+----------+-------------+---------+---------+
4 rows in set (0.00 sec)

mysql> exit;
Bye

[cloudera@quickstart ~]$
~~~

### Usage of --fields-terminated-by, --lines-terminated-by, --null-string & --null-non-string

* Note:
  * Default value of --fields-terminated-by is comma (i.e. ,)
  * Default value of --lines-terminated-by is new line character (i.e. \n)

* Import emp table data using specified delimiter from MySQL to HDFS 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table emp \
--fields-terminated-by "\t" \
--lines-terminated-by ":" \
--null-string "XXXX" \
--null-non-string "-1" \
--target-dir /user/cloudera/sqoop_import/retail_db/emp \
--num-mappers 1

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 07:04:19 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 07:04:19 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 07:04:19 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 07:04:19 INFO tool.CodeGenTool: Beginning code generation
18/03/10 07:04:19 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `emp` AS t LIMIT 1
18/03/10 07:04:19 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `emp` AS t LIMIT 1
18/03/10 07:04:19 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/84afd4b351fa89ea52e9bc96a87eac27/emp.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 07:04:21 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/84afd4b351fa89ea52e9bc96a87eac27/emp.jar
18/03/10 07:04:21 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 07:04:21 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 07:04:21 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 07:04:21 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 07:04:21 INFO mapreduce.ImportJobBase: Beginning import of emp
18/03/10 07:04:21 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 07:04:21 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 07:04:21 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 07:04:21 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 07:04:23 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 07:04:23 INFO mapreduce.JobSubmitter: number of splits:1
18/03/10 07:04:23 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0029
18/03/10 07:04:23 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0029
18/03/10 07:04:23 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0029/
18/03/10 07:04:23 INFO mapreduce.Job: Running job: job_1514521302404_0029
18/03/10 07:04:29 INFO mapreduce.Job: Job job_1514521302404_0029 running in uber mode : false
18/03/10 07:04:29 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 07:04:33 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 07:04:33 INFO mapreduce.Job: Job job_1514521302404_0029 completed successfully
18/03/10 07:04:34 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=151896
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=87
    HDFS: Number of bytes written=117
    HDFS: Number of read operations=4
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=2
  Job Counters 
    Launched map tasks=1
    Other local map tasks=1
    Total time spent by all maps in occupied slots (ms)=2253
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=2253
    Total vcore-milliseconds taken by all map tasks=2253
    Total megabyte-milliseconds taken by all map tasks=2307072
  Map-Reduce Framework
    Map input records=4
    Map output records=4
    Input split bytes=87
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=30
    CPU time spent (ms)=730
    Physical memory (bytes) snapshot=203878400
    Virtual memory (bytes) snapshot=1577840640
    Total committed heap usage (bytes)=172490752
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=117
18/03/10 07:04:34 INFO mapreduce.ImportJobBase: Transferred 117 bytes in 12.2745 seconds (9.5319 bytes/sec)
18/03/10 07:04:34 INFO mapreduce.ImportJobBase: Retrieved 4 records.
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0029/
~~~

* Verify emp data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/emp
-rw-r--r--   1 cloudera cloudera          0 2018-03-10 07:04 /user/cloudera/sqoop_import/retail_db/emp/_SUCCESS
-rw-r--r--   1 cloudera cloudera        117 2018-03-10 07:04 /user/cloudera/sqoop_import/retail_db/emp/part-m-00000
~~~

~~~
[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop_import/retail_db/emp/part-m-00000
1 shreyash  limbhetwala XXXX  -1:2  bonika  limbhetwala XXXX  -1:3  dherya  limbhetwala boni  1:4 hrisha  limbhetwala shrey 1:
~~~

### Usage of --fields-terminated-by, --lines-terminated-by, --null-string & --null-non-string

* Import emp table data using specified delimiter from MySQL to HDFS 

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table emp \
--fields-terminated-by "\001" \
--lines-terminated-by "\n" \
--null-string "XXXX" \
--null-non-string "-1" \
--target-dir /user/cloudera/sqoop_import/retail_db/emp \
--delete-target-dir \
--num-mappers 1

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 07:18:45 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 07:18:45 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/10 07:18:46 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/10 07:18:46 INFO tool.CodeGenTool: Beginning code generation
18/03/10 07:18:46 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `emp` AS t LIMIT 1
18/03/10 07:18:46 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `emp` AS t LIMIT 1
18/03/10 07:18:46 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/a7000fbca07eb47a45e40b0d2b30fc57/emp.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/10 07:18:47 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/a7000fbca07eb47a45e40b0d2b30fc57/emp.jar
18/03/10 07:18:48 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/emp deleted.
18/03/10 07:18:48 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/10 07:18:48 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/10 07:18:48 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/10 07:18:48 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/10 07:18:48 INFO mapreduce.ImportJobBase: Beginning import of emp
18/03/10 07:18:48 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/10 07:18:48 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/10 07:18:48 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/10 07:18:48 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/10 07:18:49 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/10 07:18:49 INFO mapreduce.JobSubmitter: number of splits:1
18/03/10 07:18:49 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0032
18/03/10 07:18:50 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0032
18/03/10 07:18:50 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0032/
18/03/10 07:18:50 INFO mapreduce.Job: Running job: job_1514521302404_0032
18/03/10 07:18:55 INFO mapreduce.Job: Job job_1514521302404_0032 running in uber mode : false
18/03/10 07:18:55 INFO mapreduce.Job:  map 0% reduce 0%
18/03/10 07:19:00 INFO mapreduce.Job:  map 100% reduce 0%
18/03/10 07:19:00 INFO mapreduce.Job: Job job_1514521302404_0032 completed successfully
18/03/10 07:19:00 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=151895
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=87
    HDFS: Number of bytes written=117
    HDFS: Number of read operations=4
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=2
  Job Counters 
    Launched map tasks=1
    Other local map tasks=1
    Total time spent by all maps in occupied slots (ms)=2225
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=2225
    Total vcore-milliseconds taken by all map tasks=2225
    Total megabyte-milliseconds taken by all map tasks=2278400
  Map-Reduce Framework
    Map input records=4
    Map output records=4
    Input split bytes=87
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=17
    CPU time spent (ms)=790
    Physical memory (bytes) snapshot=211722240
    Virtual memory (bytes) snapshot=1562136576
    Total committed heap usage (bytes)=244842496
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=117
18/03/10 07:19:00 INFO mapreduce.ImportJobBase: Transferred 117 bytes in 12.2389 seconds (9.5597 bytes/sec)
18/03/10 07:19:00 INFO mapreduce.ImportJobBase: Retrieved 4 records.
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0032/
~~~

* Verify emp data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/emp
-rw-r--r--   1 cloudera cloudera          0 2018-03-10 07:04 /user/cloudera/sqoop_import/retail_db/emp/_SUCCESS
-rw-r--r--   1 cloudera cloudera        117 2018-03-10 07:04 /user/cloudera/sqoop_import/retail_db/emp/part-m-00000
~~~

~~~
[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop_import/retail_db/emp/part-m-00000
1shreyashlimbhetwalaXXXX-1
2bonikalimbhetwalaXXXX-1
3dheryalimbhetwalaboni1
4hrishalimbhetwalashrey1
~~~
