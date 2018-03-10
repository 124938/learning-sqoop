## Sqoop Import

### Objective
* Sqoop import tool imports data from RDBMS to HDFS.

### Life Cycle
  
  ![Alt text](_images/sqoop_import_life_cycle.png?raw=true "Sqoop Import - Life Cycle")  

### Login to Quick Start VM OR Gateway Node of hadoop cluster

~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Wed Jan  3 02:59:27 2018 from 192.168.211.1

[cloudera@quickstart ~]$
~~~

### Syntax

~~~
[cloudera@quickstart ~]$ sqoop import --help
Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/03 04:40:05 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
usage: sqoop import [GENERIC-ARGS] [TOOL-ARGS]

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
   --append                                                   Imports data
                                                              in append
                                                              mode
   --as-avrodatafile                                          Imports data
                                                              to Avro data
                                                              files
   --as-parquetfile                                           Imports data
                                                              to Parquet
                                                              files
   --as-sequencefile                                          Imports data
                                                              to
                                                              SequenceFile
                                                              s
   --as-textfile                                              Imports data
                                                              as plain
                                                              text
                                                              (default)
   --autoreset-to-one-mapper                                  Reset the
                                                              number of
                                                              mappers to
                                                              one mapper
                                                              if no split
                                                              key
                                                              available
   --boundary-query <statement>                               Set boundary
                                                              query for
                                                              retrieving
                                                              max and min
                                                              value of the
                                                              primary key
   --columns <col,col,col...>                                 Columns to
                                                              import from
                                                              table
   --compression-codec <codec>                                Compression
                                                              codec to use
                                                              for import
   --delete-target-dir                                        Imports data
                                                              in delete
                                                              mode
   --direct                                                   Use direct
                                                              import fast
                                                              path
   --direct-split-size <n>                                    Split the
                                                              input stream
                                                              every 'n'
                                                              bytes when
                                                              importing in
                                                              direct mode
-e,--query <statement>                                        Import
                                                              results of
                                                              SQL
                                                              'statement'
   --fetch-size <n>                                           Set number
                                                              'n' of rows
                                                              to fetch
                                                              from the
                                                              database
                                                              when more
                                                              rows are
                                                              needed
   --inline-lob-limit <n>                                     Set the
                                                              maximum size
                                                              for an
                                                              inline LOB
-m,--num-mappers <n>                                          Use 'n' map
                                                              tasks to
                                                              import in
                                                              parallel
   --mapreduce-job-name <name>                                Set name for
                                                              generated
                                                              mapreduce
                                                              job
   --merge-key <column>                                       Key column
                                                              to use to
                                                              join results
   --split-by <column-name>                                   Column of
                                                              the table
                                                              used to
                                                              split work
                                                              units
   --split-limit <size>                                       Upper Limit
                                                              of rows per
                                                              split for
                                                              split
                                                              columns of
                                                              Date/Time/Ti
                                                              mestamp and
                                                              integer
                                                              types. For
                                                              date or
                                                              timestamp
                                                              fields it is
                                                              calculated
                                                              in seconds.
                                                              split-limit
                                                              should be
                                                              greater than
                                                              0
   --table <table-name>                                       Table to
                                                              read
   --target-dir <dir>                                         HDFS plain
                                                              table
                                                              destination
   --validate                                                 Validate the
                                                              copy using
                                                              the
                                                              configured
                                                              validator
   --validation-failurehandler <validation-failurehandler>    Fully
                                                              qualified
                                                              class name
                                                              for
                                                              ValidationFa
                                                              ilureHandler
   --validation-threshold <validation-threshold>              Fully
                                                              qualified
                                                              class name
                                                              for
                                                              ValidationTh
                                                              reshold
   --validator <validator>                                    Fully
                                                              qualified
                                                              class name
                                                              for the
                                                              Validator
   --warehouse-dir <dir>                                      HDFS parent
                                                              for table
                                                              destination
   --where <where clause>                                     WHERE clause
                                                              to use
                                                              during
                                                              import
-z,--compress                                                 Enable
                                                              compression

Incremental import arguments:
   --check-column <column>        Source column to check for incremental
                                  change
   --incremental <import-type>    Define an incremental import of type
                                  'append' or 'lastmodified'
   --last-value <value>           Last imported value in the incremental
                                  check column

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
   --class-name <name>                        Sets the generated class
                                              name. This overrides
                                              --package-name. When
                                              combined with --jar-file,
                                              sets the input class.
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


At minimum, you must specify --connect and --table
Arguments to mysqldump and other subprograms may be supplied
after a '--' on the command line.
~~~

## Examples - Import data from table (primary key present in table)

### Pre-Requisite

* Create parent directory under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -mkdir -p /user/cloudera/sqoop_import/retail_db
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/cloudera/sqoop_import
drwxr-xr-x   - cloudera cloudera          0 2018-03-04 04:35 /user/cloudera/sqoop_import/retail_db
~~~

### Usage of --target-dir

* Notes:
  * --target-dir should work only in case of directory is not available in HDFS

* Import orders table from MySQL to HDFS

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

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0010/
~~~

* Verify orders data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/cloudera/sqoop_import/retail_db/orders

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 04:39 /user/cloudera/sqoop_import/retail_db/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera     741614 2018-03-04 04:39 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     753022 2018-03-04 04:39 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera     752368 2018-03-04 04:39 /user/cloudera/sqoop_import/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     752940 2018-03-04 04:39 /user/cloudera/sqoop_import/retail_db/orders/part-m-00003
~~~

### Usage of --target-dir with --delete-target-dir

* Notes:
  * --delete-target-dir make sure to delete --target-dir under HDFS before importing table data

* Import orders table from MySQL to HDFS

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

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0011/
~~~

* Verify orders data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/cloudera/sqoop_import/retail_db/orders

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 04:47 /user/cloudera/sqoop_import/retail_db/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera     741614 2018-03-04 04:47 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     753022 2018-03-04 04:47 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera     752368 2018-03-04 04:47 /user/cloudera/sqoop_import/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     752940 2018-03-04 04:47 /user/cloudera/sqoop_import/retail_db/orders/part-m-00003
~~~

### Usage of --warehouse-dir

* Notes:
  * --warehouse-dir make sure to create directory under HDFS with name provided in --table clause

* Remove orders directory from HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -rm -R /user/cloudera/sqoop_import/retail_db/orders
Deleted /user/cloudera/sqoop_import/retail_db/orders
~~~

* Import orders table from MySQL to HDFS

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

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0013/
~~~

* Verify orders data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/cloudera/sqoop_import/retail_db/orders

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 05:20 /user/cloudera/sqoop_import/retail_db/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera     741614 2018-03-04 05:20 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     753022 2018-03-04 05:20 /user/cloudera/sqoop_import/retail_db/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera     752368 2018-03-04 05:20 /user/cloudera/sqoop_import/retail_db/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     752940 2018-03-04 05:20 /user/cloudera/sqoop_import/retail_db/orders/part-m-00003
~~~

### Usage of --num-mappers

* Notes:
  * Import command generate 4 files under HDFS because default value of --num-mappers is 4, this can be override by specifying --num-mappers value in command 

* Remove orders directory from HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -rm -R /user/cloudera/sqoop_import/retail_db/orders
Deleted /user/cloudera/sqoop_import/retail_db/orders
~~~

* Import orders table from MySQL to HDFS

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

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0014/
~~~

* Verify orders data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/orders

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 05:39 /user/cloudera/sqoop_import/retail_db/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera      2.9 M 2018-03-04 05:39 /user/cloudera/sqoop_import/retail_db/orders/part-m-00000
~~~

## Examples - Import data from table (primary key NOT present in table)

### Pre-Requisite

* Login to mysql

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

* Create table without primary key

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

* Import order_items_nopk table from MySQL to HDFS

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

* Notes:
  * Above command is getting failed due to primary key is not present on table
  * Possible Solutions:
    * Use --num-mappers 1 in above command
    * Use --split-by argument 

### Usage of --split-by (for numeric field)

* Notes:
  * Value specified in --split-by column should be sequence generated OR evenly incremented
  * Value specified in --split-by column should not contain null values
  * Column specified in --split-by column should be indexed at DB level else importing data may have performance impact

* Import order_items_nopk table from MySQL to HDFS

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

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0016/
~~~

* Verify order_items_nopk data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items_nopk

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 06:36 /user/cloudera/sqoop_import/retail_db/order_items_nopk/_SUCCESS
-rw-r--r--   1 cloudera cloudera      1.2 M 2018-03-04 06:36 /user/cloudera/sqoop_import/retail_db/order_items_nopk/part-m-00000
-rw-r--r--   1 cloudera cloudera      1.3 M 2018-03-04 06:36 /user/cloudera/sqoop_import/retail_db/order_items_nopk/part-m-00001
-rw-r--r--   1 cloudera cloudera      1.3 M 2018-03-04 06:36 /user/cloudera/sqoop_import/retail_db/order_items_nopk/part-m-00002
-rw-r--r--   1 cloudera cloudera      1.3 M 2018-03-04 06:36 /user/cloudera/sqoop_import/retail_db/order_items_nopk/part-m-00003
~~~

### Usage of --split-by (for non-numeric field)

* Import orders table from MySQL to HDFS (using order_status column as --split-by field)

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

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0017/
~~~

* Verify orders_non_numeric data under HDFS

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

* Notes:
  * --autoreset-to-one-mapper is equal to --num-mappers 1 i.e. it will automatically reset number of mapper to 1, in case of table does not contain primary key 

* Import order_items_nopk table from MySQL to HDFS

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

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0018/
~~~

* Verify order_items_nopk data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items_nopk

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 07:13 /user/cloudera/sqoop_import/retail_db/order_items_nopk/_SUCCESS
-rw-r--r--   1 cloudera cloudera      5.2 M 2018-03-04 07:13 /user/cloudera/sqoop_import/retail_db/order_items_nopk/part-m-00000
~~~

## Examples - Import data from table (custom bondary query)

### Usage of --boundary-query (Approach -> 1)

* Notes:
  * --boundary-query is useful in case of filtering data from source table
  * --boundary-query must return two columns

* Import order_items table from MySQL to HDFS using custom bondary query

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

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0019/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items

-rw-r--r--   1 cloudera cloudera          0 2018-03-04 07:41 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    567.6 K 2018-03-04 07:41 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000
-rw-r--r--   1 cloudera cloudera    567.4 K 2018-03-04 07:41 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001
-rw-r--r--   1 cloudera cloudera    568.9 K 2018-03-04 07:41 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00002
-rw-r--r--   1 cloudera cloudera    569.9 K 2018-03-04 07:41 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00003
~~~

### Usage of --boundary-query (Approach -> 2)

* Import order_items table from MySQL to HDFS using custom bondary query

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

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0020/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items
-rw-r--r--   1 cloudera cloudera          0 2018-03-04 07:47 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    567.6 K 2018-03-04 07:47 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000
-rw-r--r--   1 cloudera cloudera    567.4 K 2018-03-04 07:47 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001
-rw-r--r--   1 cloudera cloudera    569.0 K 2018-03-04 07:47 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00002
-rw-r--r--   1 cloudera cloudera    569.8 K 2018-03-04 07:47 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00003
~~~

### Usage of --columns

* Import order_items table from MySQL to HDFS by specific columns

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

* Verify map reduce job

~~~
http://quickstart.cloudera:8088/proxy/application_1514521302404_0020/
~~~

* Verify order_items data under HDFS

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -h -R /user/cloudera/sqoop_import/retail_db/order_items 
-rw-r--r--   1 cloudera cloudera          0 2018-03-09 21:59 /user/cloudera/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    745.8 K 2018-03-09 21:59 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00000
-rw-r--r--   1 cloudera cloudera    783.7 K 2018-03-09 21:59 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00001
-rw-r--r--   1 cloudera cloudera    812.2 K 2018-03-09 21:59 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00002
-rw-r--r--   1 cloudera cloudera    827.5 K 2018-03-09 21:59 /user/cloudera/sqoop_import/retail_db/order_items/part-m-00003
~~~

## Examples - Import data (Using query instead of table)

### Usage of --query

* Notes:
  * --query is getting used while importing data from multiple tables by applying transformation & filtering logic 
  * --query is mutually exclusive with --table and/or --columns
  * --query usage with --split-by is mandatory in case of --num-mappers is > 1
  * --query must have a placefolder called \$CONDITIONS

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

## Examples - Import data (Using incremental load)

### Usage of --table with --where & --append (Approach - 1)

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

### Usage of --query with --split-by & --append (Approach - 2)

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

### Usage of --table with --check-column, --incremental & --last-value (Approach - 3)

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

## Examples - Import data (Using different file format)

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
