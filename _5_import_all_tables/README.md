## Sqoop Import All Tables

### Objective
* Sqoop import all tables tool imports data of all tables from RDBMS to HDFS.

### Syntax

~~~
Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/11 06:22:09 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
usage: sqoop import-all-tables [GENERIC-ARGS] [TOOL-ARGS]

Common arguments:
   --connect <jdbc-uri>                                       Specify JDBC
                                                              connect
                                                              string
   --connection-manager <class-name>                          Specify
                                                              connection
                                                              manager
                                                              class name
   --connection-param-file <properties-file>                  Specify
                                                              connection
                                                              parameters
                                                              file
   --driver <class-name>                                      Manually
                                                              specify JDBC
                                                              driver class
                                                              to use
   --hadoop-home <hdir>                                       Override
                                                              $HADOOP_MAPR
                                                              ED_HOME_ARG
   --hadoop-mapred-home <dir>                                 Override
                                                              $HADOOP_MAPR
                                                              ED_HOME_ARG
   --help                                                     Print usage
                                                              instructions
   --metadata-transaction-isolation-level <isolationlevel>    Defines the
                                                              transaction
                                                              isolation
                                                              level for
                                                              metadata
                                                              queries. For
                                                              more details
                                                              check
                                                              java.sql.Con
                                                              nection
                                                              javadoc or
                                                              the JDBC
                                                              specificaito
                                                              n
   --oracle-escaping-disabled <boolean>                       Disable the
                                                              escaping
                                                              mechanism of
                                                              the
                                                              Oracle/OraOo
                                                              p connection
                                                              managers
-P                                                            Read
                                                              password
                                                              from console
   --password <password>                                      Set
                                                              authenticati
                                                              on password
   --password-alias <password-alias>                          Credential
                                                              provider
                                                              password
                                                              alias
   --password-file <password-file>                            Set
                                                              authenticati
                                                              on password
                                                              file path
   --relaxed-isolation                                        Use
                                                              read-uncommi
                                                              tted
                                                              isolation
                                                              for imports
   --skip-dist-cache                                          Skip copying
                                                              jars to
                                                              distributed
                                                              cache
   --temporary-rootdir <rootdir>                              Defines the
                                                              temporary
                                                              root
                                                              directory
                                                              for the
                                                              import
   --throw-on-error                                           Rethrow a
                                                              RuntimeExcep
                                                              tion on
                                                              error
                                                              occurred
                                                              during the
                                                              job
   --username <username>                                      Set
                                                              authenticati
                                                              on username
   --verbose                                                  Print more
                                                              information
                                                              while
                                                              working

Import control arguments:
   --as-avrodatafile              Imports data to Avro data files
   --as-parquetfile               Imports data to Parquet files
   --as-sequencefile              Imports data to SequenceFiles
   --as-textfile                  Imports data as plain text (default)
   --autoreset-to-one-mapper      Reset the number of mappers to one
                                  mapper if no split key available
   --compression-codec <codec>    Compression codec to use for import
   --direct                       Use direct import fast path
   --direct-split-size <n>        Split the input stream every 'n' bytes
                                  when importing in direct mode
   --exclude-tables <tables>      Tables to exclude when importing all
                                  tables
   --fetch-size <n>               Set number 'n' of rows to fetch from the
                                  database when more rows are needed
   --inline-lob-limit <n>         Set the maximum size for an inline LOB
-m,--num-mappers <n>              Use 'n' map tasks to import in parallel
   --mapreduce-job-name <name>    Set name for generated mapreduce job
   --warehouse-dir <dir>          HDFS parent for table destination
-z,--compress                     Enable compression

Output line formatting arguments:
   --enclosed-by <char>               Sets a required field enclosing
                                      character
   --escaped-by <char>                Sets the escape character
   --fields-terminated-by <char>      Sets the field separator character
   --lines-terminated-by <char>       Sets the end-of-line character
   --mysql-delimiters                 Uses MySQL's default delimiter set:
                                      fields: ,  lines: \n  escaped-by: \
                                      optionally-enclosed-by: '
   --optionally-enclosed-by <char>    Sets a field enclosing character

Input parsing arguments:
   --input-enclosed-by <char>               Sets a required field encloser
   --input-escaped-by <char>                Sets the input escape
                                            character
   --input-fields-terminated-by <char>      Sets the input field separator
   --input-lines-terminated-by <char>       Sets the input end-of-line
                                            char
   --input-optionally-enclosed-by <char>    Sets a field enclosing
                                            character

Hive arguments:
   --create-hive-table                         Fail if the target hive
                                               table exists
   --hive-database <database-name>             Sets the database name to
                                               use when importing to hive
   --hive-delims-replacement <arg>             Replace Hive record \0x01
                                               and row delimiters (\n\r)
                                               from imported string fields
                                               with user-defined string
   --hive-drop-import-delims                   Drop Hive record \0x01 and
                                               row delimiters (\n\r) from
                                               imported string fields
   --hive-home <dir>                           Override $HIVE_HOME
   --hive-import                               Import tables into Hive
                                               (Uses Hive's default
                                               delimiters if none are
                                               set.)
   --hive-overwrite                            Overwrite existing data in
                                               the Hive table
   --hive-partition-key <partition-key>        Sets the partition key to
                                               use when importing to hive
   --hive-partition-value <partition-value>    Sets the partition value to
                                               use when importing to hive
   --hive-table <table-name>                   Sets the table name to use
                                               when importing to hive
   --map-column-hive <arg>                     Override mapping for
                                               specific column to hive
                                               types.

HBase arguments:
   --column-family <family>    Sets the target column family for the
                               import
   --hbase-bulkload            Enables HBase bulk loading
   --hbase-create-table        If specified, create missing HBase tables
   --hbase-row-key <col>       Specifies which input column to use as the
                               row key
   --hbase-table <table>       Import to <table> in HBase

HCatalog arguments:
   --hcatalog-database <arg>                        HCatalog database name
   --hcatalog-home <hdir>                           Override $HCAT_HOME
   --hcatalog-partition-keys <partition-key>        Sets the partition
                                                    keys to use when
                                                    importing to hive
   --hcatalog-partition-values <partition-value>    Sets the partition
                                                    values to use when
                                                    importing to hive
   --hcatalog-table <arg>                           HCatalog table name
   --hive-home <dir>                                Override $HIVE_HOME
   --hive-partition-key <partition-key>             Sets the partition key
                                                    to use when importing
                                                    to hive
   --hive-partition-value <partition-value>         Sets the partition
                                                    value to use when
                                                    importing to hive
   --map-column-hive <arg>                          Override mapping for
                                                    specific column to
                                                    hive types.

HCatalog import specific options:
   --create-hcatalog-table             Create HCatalog before import
   --drop-and-create-hcatalog-table    Drop and Create HCatalog before
                                       import
   --hcatalog-storage-stanza <arg>     HCatalog storage stanza for table
                                       creation

Accumulo arguments:
   --accumulo-batch-size <size>          Batch size in bytes
   --accumulo-column-family <family>     Sets the target column family for
                                         the import
   --accumulo-create-table               If specified, create missing
                                         Accumulo tables
   --accumulo-instance <instance>        Accumulo instance name.
   --accumulo-max-latency <latency>      Max write latency in milliseconds
   --accumulo-password <password>        Accumulo password.
   --accumulo-row-key <col>              Specifies which input column to
                                         use as the row key
   --accumulo-table <table>              Import to <table> in Accumulo
   --accumulo-user <user>                Accumulo user name.
   --accumulo-visibility <vis>           Visibility token to be applied to
                                         all rows imported
   --accumulo-zookeepers <zookeepers>    Comma-separated list of
                                         zookeepers (host:port)

Code generation arguments:
   --bindir <dir>                             Output directory for
                                              compiled objects
   --escape-mapping-column-names <boolean>    Disable special characters
                                              escaping in column names
   --input-null-non-string <null-str>         Input null non-string
                                              representation
   --input-null-string <null-str>             Input null string
                                              representation
   --jar-file <file>                          Disable code generation; use
                                              specified jar
   --map-column-java <arg>                    Override mapping for
                                              specific columns to java
                                              types
   --null-non-string <null-str>               Null non-string
                                              representation
   --null-string <null-str>                   Null string representation
   --outdir <dir>                             Output directory for
                                              generated code
   --package-name <name>                      Put auto-generated classes
                                              in this package

Generic Hadoop command-line arguments:
(must preceed any tool-specific arguments)
Generic options supported are
-conf <configuration file>     specify an application configuration file
-D <property=value>            use value for given property
-fs <local|namenode:port>      specify a namenode
-jt <local|resourcemanager:port>    specify a ResourceManager
-files <comma separated list of files>    specify comma separated files to be copied to the map reduce cluster
-libjars <comma separated list of jars>    specify comma separated jar files to include in the classpath.
-archives <comma separated list of archives>    specify comma separated archives to be unarchived on the compute machines.

The general command line syntax is
bin/hadoop command [genericOptions] [commandOptions]


At minimum, you must specify --connect
Arguments to mysqldump and other subprograms may be supplied
after a '--' on the command line.
~~~

## Examples

### Usage of --import-all-tables

* **Important Notes:**
  * Does not support many control aruments like --table, --query, --split-by, --where, --cols etc. i.e. filtering, transformation is not possible
  * As few tables may not have primary key present, better to use --autoreset-to-one-mapper  
  * --warehouse-dir is mandatory

* **Import all table from MySQL to HDFS**

~~~
[cloudera@quickstart ~]$ sqoop-import-all-tables \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--warehouse-dir /user/cloudera/sqoop_import_all_tables/retail_db \
--autoreset-to-one-mapper

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/11 06:31:26 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/11 06:31:26 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/11 06:31:26 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/11 06:31:26 INFO tool.CodeGenTool: Beginning code generation
18/03/11 06:31:26 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `TMP` AS t LIMIT 1
18/03/11 06:31:26 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `TMP` AS t LIMIT 1
18/03/11 06:31:26 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/TMP.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 06:31:27 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/TMP.jar
18/03/11 06:31:27 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/11 06:31:27 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/11 06:31:27 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/11 06:31:27 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/11 06:31:27 WARN manager.SqlManager: Split by column not provided or can't be inferred.  Resetting to one mapper
18/03/11 06:31:27 INFO mapreduce.ImportJobBase: Beginning import of TMP
18/03/11 06:31:27 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 06:31:28 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/11 06:31:28 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/11 06:31:28 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 06:31:30 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 06:31:30 INFO mapreduce.JobSubmitter: number of splits:1
18/03/11 06:31:30 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0049
18/03/11 06:31:30 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0049
18/03/11 06:31:30 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0049/
18/03/11 06:31:30 INFO mapreduce.Job: Running job: job_1514521302404_0049
18/03/11 06:31:37 INFO mapreduce.Job: Job job_1514521302404_0049 running in uber mode : false
18/03/11 06:31:37 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 06:31:41 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 06:31:41 INFO mapreduce.Job: Job job_1514521302404_0049 completed successfully
18/03/11 06:31:41 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=151345
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=87
		HDFS: Number of bytes written=0
		HDFS: Number of read operations=4
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=2
	Job Counters 
		Launched map tasks=1
		Other local map tasks=1
		Total time spent by all maps in occupied slots (ms)=2207
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=2207
		Total vcore-milliseconds taken by all map tasks=2207
		Total megabyte-milliseconds taken by all map tasks=2259968
	Map-Reduce Framework
		Map input records=0
		Map output records=0
		Input split bytes=87
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=28
		CPU time spent (ms)=520
		Physical memory (bytes) snapshot=199299072
		Virtual memory (bytes) snapshot=1580007424
		Total committed heap usage (bytes)=172490752
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
18/03/11 06:31:41 INFO mapreduce.ImportJobBase: Transferred 0 bytes in 12.6346 seconds (0 bytes/sec)
18/03/11 06:31:41 INFO mapreduce.ImportJobBase: Retrieved 0 records.
18/03/11 06:31:41 INFO tool.CodeGenTool: Beginning code generation
18/03/11 06:31:41 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `categories` AS t LIMIT 1
18/03/11 06:31:41 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/categories.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 06:31:41 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/categories.jar
18/03/11 06:31:41 INFO mapreduce.ImportJobBase: Beginning import of categories
18/03/11 06:31:41 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 06:31:41 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 06:31:42 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 06:31:42 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`category_id`), MAX(`category_id`) FROM `categories`
18/03/11 06:31:42 INFO db.IntegerSplitter: Split size: 14; Num splits: 4 from: 1 to: 58
18/03/11 06:31:42 INFO mapreduce.JobSubmitter: number of splits:4
18/03/11 06:31:42 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0050
18/03/11 06:31:42 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0050
18/03/11 06:31:42 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0050/
18/03/11 06:31:42 INFO mapreduce.Job: Running job: job_1514521302404_0050
18/03/11 06:31:47 INFO mapreduce.Job: Job job_1514521302404_0050 running in uber mode : false
18/03/11 06:31:47 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 06:31:52 INFO mapreduce.Job:  map 25% reduce 0%
18/03/11 06:31:53 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 06:31:55 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 06:31:55 INFO mapreduce.Job: Job job_1514521302404_0050 completed successfully
18/03/11 06:31:55 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=606284
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=472
		HDFS: Number of bytes written=1029
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=12395
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=12395
		Total vcore-milliseconds taken by all map tasks=12395
		Total megabyte-milliseconds taken by all map tasks=12692480
	Map-Reduce Framework
		Map input records=58
		Map output records=58
		Input split bytes=472
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=112
		CPU time spent (ms)=3260
		Physical memory (bytes) snapshot=835706880
		Virtual memory (bytes) snapshot=6293893120
		Total committed heap usage (bytes)=907018240
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=1029
18/03/11 06:31:55 INFO mapreduce.ImportJobBase: Transferred 1.0049 KB in 14.2515 seconds (72.2031 bytes/sec)
18/03/11 06:31:55 INFO mapreduce.ImportJobBase: Retrieved 58 records.
18/03/11 06:31:55 INFO tool.CodeGenTool: Beginning code generation
18/03/11 06:31:55 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `customers` AS t LIMIT 1
18/03/11 06:31:55 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/customers.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 06:31:56 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/customers.jar
18/03/11 06:31:56 INFO mapreduce.ImportJobBase: Beginning import of customers
18/03/11 06:31:56 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 06:31:56 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 06:31:57 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 06:31:57 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`customer_id`), MAX(`customer_id`) FROM `customers`
18/03/11 06:31:57 INFO db.IntegerSplitter: Split size: 3108; Num splits: 4 from: 1 to: 12435
18/03/11 06:31:57 INFO mapreduce.JobSubmitter: number of splits:4
18/03/11 06:31:57 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0051
18/03/11 06:31:57 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0051
18/03/11 06:31:57 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0051/
18/03/11 06:31:57 INFO mapreduce.Job: Running job: job_1514521302404_0051
18/03/11 06:32:03 INFO mapreduce.Job: Job job_1514521302404_0051 running in uber mode : false
18/03/11 06:32:03 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 06:32:08 INFO mapreduce.Job:  map 25% reduce 0%
18/03/11 06:32:10 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 06:32:11 INFO mapreduce.Job:  map 75% reduce 0%
18/03/11 06:32:12 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 06:32:12 INFO mapreduce.Job: Job job_1514521302404_0051 completed successfully
18/03/11 06:32:12 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=606668
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=487
		HDFS: Number of bytes written=953525
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=13749
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=13749
		Total vcore-milliseconds taken by all map tasks=13749
		Total megabyte-milliseconds taken by all map tasks=14078976
	Map-Reduce Framework
		Map input records=12435
		Map output records=12435
		Input split bytes=487
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=197
		CPU time spent (ms)=4940
		Physical memory (bytes) snapshot=855793664
		Virtual memory (bytes) snapshot=6322425856
		Total committed heap usage (bytes)=844103680
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=953525
18/03/11 06:32:12 INFO mapreduce.ImportJobBase: Transferred 931.1768 KB in 16.2959 seconds (57.1418 KB/sec)
18/03/11 06:32:12 INFO mapreduce.ImportJobBase: Retrieved 12435 records.
18/03/11 06:32:12 INFO tool.CodeGenTool: Beginning code generation
18/03/11 06:32:12 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `departments` AS t LIMIT 1
18/03/11 06:32:12 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/departments.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 06:32:12 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/departments.jar
18/03/11 06:32:12 INFO mapreduce.ImportJobBase: Beginning import of departments
18/03/11 06:32:12 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 06:32:12 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 06:32:13 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 06:32:13 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`department_id`), MAX(`department_id`) FROM `departments`
18/03/11 06:32:13 INFO db.IntegerSplitter: Split size: 1; Num splits: 4 from: 2 to: 7
18/03/11 06:32:13 INFO mapreduce.JobSubmitter: number of splits:4
18/03/11 06:32:13 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0052
18/03/11 06:32:13 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0052
18/03/11 06:32:13 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0052/
18/03/11 06:32:13 INFO mapreduce.Job: Running job: job_1514521302404_0052
18/03/11 06:32:18 INFO mapreduce.Job: Job job_1514521302404_0052 running in uber mode : false
18/03/11 06:32:18 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 06:32:23 INFO mapreduce.Job:  map 25% reduce 0%
18/03/11 06:32:24 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 06:32:25 INFO mapreduce.Job:  map 75% reduce 0%
18/03/11 06:32:26 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 06:32:26 INFO mapreduce.Job: Job job_1514521302404_0052 completed successfully
18/03/11 06:32:26 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=606224
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=481
		HDFS: Number of bytes written=60
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=12181
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=12181
		Total vcore-milliseconds taken by all map tasks=12181
		Total megabyte-milliseconds taken by all map tasks=12473344
	Map-Reduce Framework
		Map input records=6
		Map output records=6
		Input split bytes=481
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=134
		CPU time spent (ms)=3420
		Physical memory (bytes) snapshot=820768768
		Virtual memory (bytes) snapshot=6319722496
		Total committed heap usage (bytes)=689963008
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=60
18/03/11 06:32:26 INFO mapreduce.ImportJobBase: Transferred 60 bytes in 13.9058 seconds (4.3147 bytes/sec)
18/03/11 06:32:26 INFO mapreduce.ImportJobBase: Retrieved 6 records.
18/03/11 06:32:26 INFO tool.CodeGenTool: Beginning code generation
18/03/11 06:32:26 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `emp` AS t LIMIT 1
18/03/11 06:32:26 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/emp.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 06:32:26 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/emp.jar
18/03/11 06:32:26 INFO mapreduce.ImportJobBase: Beginning import of emp
18/03/11 06:32:26 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 06:32:26 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 06:32:27 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 06:32:27 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`id`), MAX(`id`) FROM `emp`
18/03/11 06:32:27 INFO db.IntegerSplitter: Split size: 0; Num splits: 4 from: 1 to: 4
18/03/11 06:32:27 INFO mapreduce.JobSubmitter: number of splits:4
18/03/11 06:32:27 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0053
18/03/11 06:32:27 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0053
18/03/11 06:32:27 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0053/
18/03/11 06:32:27 INFO mapreduce.Job: Running job: job_1514521302404_0053
18/03/11 06:32:33 INFO mapreduce.Job: Job job_1514521302404_0053 running in uber mode : false
18/03/11 06:32:33 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 06:32:39 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 06:32:40 INFO mapreduce.Job:  map 75% reduce 0%
18/03/11 06:32:41 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 06:32:41 INFO mapreduce.Job: Job job_1514521302404_0053 completed successfully
18/03/11 06:32:41 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=606088
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=393
		HDFS: Number of bytes written=121
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=13195
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=13195
		Total vcore-milliseconds taken by all map tasks=13195
		Total megabyte-milliseconds taken by all map tasks=13511680
	Map-Reduce Framework
		Map input records=4
		Map output records=4
		Input split bytes=393
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=133
		CPU time spent (ms)=3500
		Physical memory (bytes) snapshot=823631872
		Virtual memory (bytes) snapshot=6325575680
		Total committed heap usage (bytes)=762314752
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=121
18/03/11 06:32:41 INFO mapreduce.ImportJobBase: Transferred 121 bytes in 14.3032 seconds (8.4597 bytes/sec)
18/03/11 06:32:41 INFO mapreduce.ImportJobBase: Retrieved 4 records.
18/03/11 06:32:41 INFO tool.CodeGenTool: Beginning code generation
18/03/11 06:32:41 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items` AS t LIMIT 1
18/03/11 06:32:41 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 06:32:41 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/order_items.jar
18/03/11 06:32:41 INFO mapreduce.ImportJobBase: Beginning import of order_items
18/03/11 06:32:41 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 06:32:41 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 06:32:42 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 06:32:42 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_item_id`), MAX(`order_item_id`) FROM `order_items`
18/03/11 06:32:42 INFO db.IntegerSplitter: Split size: 43049; Num splits: 4 from: 1 to: 172198
18/03/11 06:32:42 INFO mapreduce.JobSubmitter: number of splits:4
18/03/11 06:32:42 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0054
18/03/11 06:32:43 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0054
18/03/11 06:32:43 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0054/
18/03/11 06:32:43 INFO mapreduce.Job: Running job: job_1514521302404_0054
18/03/11 06:32:49 INFO mapreduce.Job: Job job_1514521302404_0054 running in uber mode : false
18/03/11 06:32:49 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 06:32:55 INFO mapreduce.Job:  map 25% reduce 0%
18/03/11 06:32:57 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 06:32:58 INFO mapreduce.Job:  map 75% reduce 0%
18/03/11 06:32:59 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 06:32:59 INFO mapreduce.Job: Job job_1514521302404_0054 completed successfully
18/03/11 06:32:59 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=606620
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
		Total time spent by all maps in occupied slots (ms)=17339
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=17339
		Total vcore-milliseconds taken by all map tasks=17339
		Total megabyte-milliseconds taken by all map tasks=17755136
	Map-Reduce Framework
		Map input records=172198
		Map output records=172198
		Input split bytes=512
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=223
		CPU time spent (ms)=9240
		Physical memory (bytes) snapshot=1029283840
		Virtual memory (bytes) snapshot=6298783744
		Total committed heap usage (bytes)=987234304
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=5408880
18/03/11 06:32:59 INFO mapreduce.ImportJobBase: Transferred 5.1583 MB in 18.0128 seconds (293.242 KB/sec)
18/03/11 06:32:59 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
18/03/11 06:32:59 INFO tool.CodeGenTool: Beginning code generation
18/03/11 06:32:59 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `order_items_nopk` AS t LIMIT 1
18/03/11 06:32:59 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/order_items_nopk.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 06:32:59 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/order_items_nopk.jar
18/03/11 06:32:59 WARN manager.SqlManager: Split by column not provided or can't be inferred.  Resetting to one mapper
18/03/11 06:32:59 INFO mapreduce.ImportJobBase: Beginning import of order_items_nopk
18/03/11 06:32:59 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 06:32:59 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 06:33:01 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 06:33:01 INFO mapreduce.JobSubmitter: number of splits:1
18/03/11 06:33:01 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0055
18/03/11 06:33:01 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0055
18/03/11 06:33:01 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0055/
18/03/11 06:33:01 INFO mapreduce.Job: Running job: job_1514521302404_0055
18/03/11 06:33:07 INFO mapreduce.Job: Job job_1514521302404_0055 running in uber mode : false
18/03/11 06:33:07 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 06:33:12 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 06:33:13 INFO mapreduce.Job: Job job_1514521302404_0055 completed successfully
18/03/11 06:33:13 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=151526
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
		Total time spent by all maps in occupied slots (ms)=3321
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=3321
		Total vcore-milliseconds taken by all map tasks=3321
		Total megabyte-milliseconds taken by all map tasks=3400704
	Map-Reduce Framework
		Map input records=172198
		Map output records=172198
		Input split bytes=87
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=66
		CPU time spent (ms)=3700
		Physical memory (bytes) snapshot=291147776
		Virtual memory (bytes) snapshot=1578553344
		Total committed heap usage (bytes)=370147328
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=5408880
18/03/11 06:33:13 INFO mapreduce.ImportJobBase: Transferred 5.1583 MB in 14.2069 seconds (371.7981 KB/sec)
18/03/11 06:33:13 INFO mapreduce.ImportJobBase: Retrieved 172198 records.
18/03/11 06:33:13 INFO tool.CodeGenTool: Beginning code generation
18/03/11 06:33:13 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/11 06:33:13 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 06:33:14 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/orders.jar
18/03/11 06:33:14 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/11 06:33:14 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 06:33:14 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 06:33:15 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 06:33:15 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders`
18/03/11 06:33:15 INFO db.IntegerSplitter: Split size: 17220; Num splits: 4 from: 1 to: 68883
18/03/11 06:33:15 INFO mapreduce.JobSubmitter: number of splits:4
18/03/11 06:33:15 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0056
18/03/11 06:33:16 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0056
18/03/11 06:33:16 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0056/
18/03/11 06:33:16 INFO mapreduce.Job: Running job: job_1514521302404_0056
18/03/11 06:33:22 INFO mapreduce.Job: Job job_1514521302404_0056 running in uber mode : false
18/03/11 06:33:22 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 06:33:28 INFO mapreduce.Job:  map 25% reduce 0%
18/03/11 06:33:29 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 06:33:30 INFO mapreduce.Job:  map 75% reduce 0%
18/03/11 06:33:31 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 06:33:32 INFO mapreduce.Job: Job job_1514521302404_0056 completed successfully
18/03/11 06:33:32 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=606224
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
		Total time spent by all maps in occupied slots (ms)=18011
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=18011
		Total vcore-milliseconds taken by all map tasks=18011
		Total megabyte-milliseconds taken by all map tasks=18443264
	Map-Reduce Framework
		Map input records=68883
		Map output records=68883
		Input split bytes=469
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=222
		CPU time spent (ms)=10160
		Physical memory (bytes) snapshot=919007232
		Virtual memory (bytes) snapshot=6313586688
		Total committed heap usage (bytes)=987234304
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=2999944
18/03/11 06:33:32 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 18.2419 seconds (160.5991 KB/sec)
18/03/11 06:33:32 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
18/03/11 06:33:32 INFO tool.CodeGenTool: Beginning code generation
18/03/11 06:33:32 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `products` AS t LIMIT 1
18/03/11 06:33:32 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/products.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/11 06:33:32 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/87d9b13c3a81b89b6a2ca0a778803e8a/products.jar
18/03/11 06:33:32 INFO mapreduce.ImportJobBase: Beginning import of products
18/03/11 06:33:32 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/11 06:33:32 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/11 06:33:33 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/11 06:33:33 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`product_id`), MAX(`product_id`) FROM `products`
18/03/11 06:33:33 INFO db.IntegerSplitter: Split size: 336; Num splits: 4 from: 1 to: 1345
18/03/11 06:33:33 INFO mapreduce.JobSubmitter: number of splits:4
18/03/11 06:33:33 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0057
18/03/11 06:33:34 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0057
18/03/11 06:33:34 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0057/
18/03/11 06:33:34 INFO mapreduce.Job: Running job: job_1514521302404_0057
18/03/11 06:33:39 INFO mapreduce.Job: Job job_1514521302404_0057 running in uber mode : false
18/03/11 06:33:39 INFO mapreduce.Job:  map 0% reduce 0%
18/03/11 06:33:45 INFO mapreduce.Job:  map 50% reduce 0%
18/03/11 06:33:46 INFO mapreduce.Job:  map 75% reduce 0%
18/03/11 06:33:47 INFO mapreduce.Job:  map 100% reduce 0%
18/03/11 06:33:47 INFO mapreduce.Job: Job job_1514521302404_0057 completed successfully
18/03/11 06:33:47 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=606444
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=474
		HDFS: Number of bytes written=173993
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=13559
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=13559
		Total vcore-milliseconds taken by all map tasks=13559
		Total megabyte-milliseconds taken by all map tasks=13884416
	Map-Reduce Framework
		Map input records=1345
		Map output records=1345
		Input split bytes=474
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=135
		CPU time spent (ms)=3530
		Physical memory (bytes) snapshot=833159168
		Virtual memory (bytes) snapshot=6320467968
		Total committed heap usage (bytes)=834666496
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=173993
18/03/11 06:33:47 INFO mapreduce.ImportJobBase: Transferred 169.915 KB in 14.3427 seconds (11.8468 KB/sec)
18/03/11 06:33:47 INFO mapreduce.ImportJobBase: Retrieved 1345 records.
~~~

* **Verify all tables under HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import_all_tables/retail_db

drwxr-xr-x   - cloudera cloudera          0 2018-03-11 06:31 /user/cloudera/sqoop_import_all_tables/retail_db/TMP
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 06:31 /user/cloudera/sqoop_import_all_tables/retail_db/TMP/_SUCCESS
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 06:31 /user/cloudera/sqoop_import_all_tables/retail_db/TMP/part-m-00000
drwxr-xr-x   - cloudera cloudera          0 2018-03-11 06:31 /user/cloudera/sqoop_import_all_tables/retail_db/categories
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 06:31 /user/cloudera/sqoop_import_all_tables/retail_db/categories/_SUCCESS
-rw-r--r--   1 cloudera cloudera        271 2018-03-11 06:31 /user/cloudera/sqoop_import_all_tables/retail_db/categories/part-m-00000
-rw-r--r--   1 cloudera cloudera        263 2018-03-11 06:31 /user/cloudera/sqoop_import_all_tables/retail_db/categories/part-m-00001
-rw-r--r--   1 cloudera cloudera        266 2018-03-11 06:31 /user/cloudera/sqoop_import_all_tables/retail_db/categories/part-m-00002
-rw-r--r--   1 cloudera cloudera        229 2018-03-11 06:31 /user/cloudera/sqoop_import_all_tables/retail_db/categories/part-m-00003
drwxr-xr-x   - cloudera cloudera          0 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/customers
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/customers/_SUCCESS
-rw-r--r--   1 cloudera cloudera    231.6 K 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/customers/part-m-00000
-rw-r--r--   1 cloudera cloudera    232.4 K 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/customers/part-m-00001
-rw-r--r--   1 cloudera cloudera    232.5 K 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/customers/part-m-00002
-rw-r--r--   1 cloudera cloudera    234.7 K 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/customers/part-m-00003
drwxr-xr-x   - cloudera cloudera          0 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/departments
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/departments/_SUCCESS
-rw-r--r--   1 cloudera cloudera         21 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/departments/part-m-00000
-rw-r--r--   1 cloudera cloudera         10 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/departments/part-m-00001
-rw-r--r--   1 cloudera cloudera          7 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/departments/part-m-00002
-rw-r--r--   1 cloudera cloudera         22 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/departments/part-m-00003
drwxr-xr-x   - cloudera cloudera          0 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/emp
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/emp/_SUCCESS
-rw-r--r--   1 cloudera cloudera         33 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/emp/part-m-00000
-rw-r--r--   1 cloudera cloudera         31 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/emp/part-m-00001
-rw-r--r--   1 cloudera cloudera         28 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/emp/part-m-00002
-rw-r--r--   1 cloudera cloudera         29 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/emp/part-m-00003
drwxr-xr-x   - cloudera cloudera          0 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/order_items
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera      1.2 M 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/order_items/part-m-00000
-rw-r--r--   1 cloudera cloudera      1.3 M 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/order_items/part-m-00001
-rw-r--r--   1 cloudera cloudera      1.3 M 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/order_items/part-m-00002
-rw-r--r--   1 cloudera cloudera      1.3 M 2018-03-11 06:32 /user/cloudera/sqoop_import_all_tables/retail_db/order_items/part-m-00003
drwxr-xr-x   - cloudera cloudera          0 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/order_items_nopk
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/order_items_nopk/_SUCCESS
-rw-r--r--   1 cloudera cloudera      5.2 M 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/order_items_nopk/part-m-00000
drwxr-xr-x   - cloudera cloudera          0 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/orders
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera    724.2 K 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera    735.4 K 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera    734.7 K 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera    735.3 K 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/orders/part-m-00003
drwxr-xr-x   - cloudera cloudera          0 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/products
-rw-r--r--   1 cloudera cloudera          0 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/products/_SUCCESS
-rw-r--r--   1 cloudera cloudera     40.4 K 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/products/part-m-00000
-rw-r--r--   1 cloudera cloudera     42.6 K 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/products/part-m-00001
-rw-r--r--   1 cloudera cloudera     41.2 K 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/products/part-m-00002
-rw-r--r--   1 cloudera cloudera     45.6 K 2018-03-11 06:33 /user/cloudera/sqoop_import_all_tables/retail_db/products/part-m-00003
~~~