## Examples - Import data (Using Query instead of table)

### Important Notes:
* --query is getting used while importing data from multiple tables by applying transformation & filtering logic 
* --query is mutually exclusive with --table and/or --columns
* --query usage with --split-by is mandatory in case of --num-mappers is > 1
* --query must have a placefolder called \$CONDITIONS

### Sample import using query

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--query "select o.order_id, o.order_date, o.order_status, sum(oi.order_item_subtotal) from orders o join order_items oi on (o.order_id = oi.order_item_order_id) where o.order_status = 'COMPLETE' and \$CONDITIONS group by o.order_id, o.order_date, o.order_status" \
--target-dir /user/cloudera/sqoop_import/retail_db/order_revenue \
--delete-target-dir \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/10 23:05:11 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/10 23:05:11 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
When importing query results in parallel, you must specify --split-by.
Try --help for usage instructions.
~~~

### Usage of --query

* Import order_items table from MySQL to HDFS using custom bondary query

~~~
[cloudera@quickstart ~]$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--query "select o.order_id, o.order_date, o.order_status, sum(oi.order_item_subtotal) from orders o join order_items oi on (o.order_id = oi.order_item_order_id) where o.order_status = 'COMPLETE' and \$CONDITIONS group by o.order_id, o.order_date, o.order_status" \
--split-by order_id \
--target-dir /user/cloudera/sqoop_import/retail_db/order_revenue \
--delete-target-dir \
--num-mappers 2

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/09 22:55:05 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/09 22:55:05 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/09 22:55:06 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/09 22:55:06 INFO tool.CodeGenTool: Beginning code generation
18/03/09 22:55:06 INFO manager.SqlManager: Executing SQL statement: select o.order_id, o.order_date, o.order_status, sum(oi.order_item_subtotal) from orders o join order_items oi on (o.order_id = oi.order_item_order_id) where o.order_status = 'COMPLETE' and  (1 = 0)  group by o.order_id, o.order_date, o.order_status
18/03/09 22:55:06 INFO manager.SqlManager: Executing SQL statement: select o.order_id, o.order_date, o.order_status, sum(oi.order_item_subtotal) from orders o join order_items oi on (o.order_id = oi.order_item_order_id) where o.order_status = 'COMPLETE' and  (1 = 0)  group by o.order_id, o.order_date, o.order_status
18/03/09 22:55:06 INFO manager.SqlManager: Executing SQL statement: select o.order_id, o.order_date, o.order_status, sum(oi.order_item_subtotal) from orders o join order_items oi on (o.order_id = oi.order_item_order_id) where o.order_status = 'COMPLETE' and  (1 = 0)  group by o.order_id, o.order_date, o.order_status
18/03/09 22:55:06 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/ab0338f0227091bfa1574792b4e18e80/QueryResult.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/09 22:55:07 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/ab0338f0227091bfa1574792b4e18e80/QueryResult.jar
18/03/09 22:55:08 INFO tool.ImportTool: Destination directory /user/cloudera/sqoop_import/retail_db/order_revenue deleted.
18/03/09 22:55:08 INFO mapreduce.ImportJobBase: Beginning query import.
18/03/09 22:55:08 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/09 22:55:08 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/09 22:55:08 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/09 22:55:08 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/09 22:55:09 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/09 22:55:09 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(order_id), MAX(order_id) FROM (select o.order_id, o.order_date, o.order_status, sum(oi.order_item_subtotal) from orders o join order_items oi on (o.order_id = oi.order_item_order_id) where o.order_status = 'COMPLETE' and  (1 = 1)  group by o.order_id, o.order_date, o.order_status) AS t1
18/03/09 22:55:09 INFO db.IntegerSplitter: Split size: 34439; Num splits: 2 from: 5 to: 68883
18/03/09 22:55:09 INFO mapreduce.JobSubmitter: number of splits:2
18/03/09 22:55:09 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0024
18/03/09 22:55:09 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0024
18/03/09 22:55:09 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0024/
18/03/09 22:55:09 INFO mapreduce.Job: Running job: job_1514521302404_0024
18/03/09 22:55:15 INFO mapreduce.Job: Job job_1514521302404_0024 running in uber mode : false
18/03/09 22:55:15 INFO mapreduce.Job:  map 0% reduce 0%
18/03/09 22:55:21 INFO mapreduce.Job:  map 50% reduce 0%
18/03/09 22:55:22 INFO mapreduce.Job:  map 100% reduce 0%
18/03/09 22:55:22 INFO mapreduce.Job: Job job_1514521302404_0024 completed successfully
18/03/09 22:55:22 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=304980
    FILE: Number of read operations=0
    FILE: Number of large read operations=0
    FILE: Number of write operations=0
    HDFS: Number of bytes read=225
    HDFS: Number of bytes written=1012769
    HDFS: Number of read operations=8
    HDFS: Number of large read operations=0
    HDFS: Number of write operations=4
  Job Counters 
    Launched map tasks=2
    Other local map tasks=2
    Total time spent by all maps in occupied slots (ms)=6274
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=6274
    Total vcore-milliseconds taken by all map tasks=6274
    Total megabyte-milliseconds taken by all map tasks=6424576
  Map-Reduce Framework
    Map input records=18965
    Map output records=18965
    Input split bytes=225
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=85
    CPU time spent (ms)=3830
    Physical memory (bytes) snapshot=449859584
    Virtual memory (bytes) snapshot=3167367168
    Total committed heap usage (bytes)=494927872
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=1012769
18/03/09 22:55:22 INFO mapreduce.ImportJobBase: Transferred 989.0322 KB in 13.932 seconds (70.9902 KB/sec)
18/03/09 22:55:22 INFO mapreduce.ImportJobBase: Retrieved 18965 records.
~~~

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0024/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_revenue
-rw-r--r--   1 cloudera cloudera          0 2018-03-09 22:55 /user/cloudera/sqoop_import/retail_db/order_revenue/_SUCCESS
-rw-r--r--   1 cloudera cloudera    488.9 K 2018-03-09 22:55 /user/cloudera/sqoop_import/retail_db/order_revenue/part-m-00000
-rw-r--r--   1 cloudera cloudera    500.2 K 2018-03-09 22:55 /user/cloudera/sqoop_import/retail_db/order_revenue/part-m-00001
~~~
