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

## Examples

### Sample import

* Create parent directory under HDFS

~~~
hadoop fs -mkdir /user/cloudera/sqoop_import
hadoop fs -ls /user/cloudera/sqoop_import
~~~

* Import orders table from MySQL to HDFS

~~~
asus@asus-GL553VD:~$ sqoop-import \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--table orders \
--target-dir /user/cloudera/sqoop_import/orders

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/03 07:52:36 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/03 07:52:36 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/03 07:52:36 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/03 07:52:37 INFO tool.CodeGenTool: Beginning code generation
18/03/03 07:52:37 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/03 07:52:37 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `orders` AS t LIMIT 1
18/03/03 07:52:37 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/07397d0853c95eb338bdbb005d6b7282/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
18/03/03 07:52:38 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/07397d0853c95eb338bdbb005d6b7282/orders.jar
18/03/03 07:52:38 WARN manager.MySQLManager: It looks like you are importing from mysql.
18/03/03 07:52:38 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
18/03/03 07:52:38 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
18/03/03 07:52:38 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
18/03/03 07:52:38 INFO mapreduce.ImportJobBase: Beginning import of orders
18/03/03 07:52:38 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
18/03/03 07:52:38 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
18/03/03 07:52:39 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
18/03/03 07:52:39 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
18/03/03 07:52:40 INFO db.DBInputFormat: Using read commited transaction isolation
18/03/03 07:52:40 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`order_id`), MAX(`order_id`) FROM `orders`
18/03/03 07:52:40 INFO db.IntegerSplitter: Split size: 17220; Num splits: 4 from: 1 to: 68883
18/03/03 07:52:40 INFO mapreduce.JobSubmitter: number of splits:4
18/03/03 07:52:40 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1514521302404_0009
18/03/03 07:52:40 INFO impl.YarnClientImpl: Submitted application application_1514521302404_0009
18/03/03 07:52:40 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1514521302404_0009/
18/03/03 07:52:40 INFO mapreduce.Job: Running job: job_1514521302404_0009
18/03/03 07:52:45 INFO mapreduce.Job: Job job_1514521302404_0009 running in uber mode : false
18/03/03 07:52:45 INFO mapreduce.Job:  map 0% reduce 0%
18/03/03 07:52:52 INFO mapreduce.Job:  map 25% reduce 0%
18/03/03 07:52:53 INFO mapreduce.Job:  map 50% reduce 0%
18/03/03 07:52:54 INFO mapreduce.Job:  map 75% reduce 0%
18/03/03 07:52:55 INFO mapreduce.Job:  map 100% reduce 0%
18/03/03 07:52:55 INFO mapreduce.Job: Job job_1514521302404_0009 completed successfully
18/03/03 07:52:55 INFO mapreduce.Job: Counters: 30
  File System Counters
    FILE: Number of bytes read=0
    FILE: Number of bytes written=606596
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
    Total time spent by all maps in occupied slots (ms)=18158
    Total time spent by all reduces in occupied slots (ms)=0
    Total time spent by all map tasks (ms)=18158
    Total vcore-milliseconds taken by all map tasks=18158
    Total megabyte-milliseconds taken by all map tasks=18593792
  Map-Reduce Framework
    Map input records=68883
    Map output records=68883
    Input split bytes=469
    Spilled Records=0
    Failed Shuffles=0
    Merged Map outputs=0
    GC time elapsed (ms)=206
    CPU time spent (ms)=9920
    Physical memory (bytes) snapshot=875040768
    Virtual memory (bytes) snapshot=6306291712
    Total committed heap usage (bytes)=916455424
  File Input Format Counters 
    Bytes Read=0
  File Output Format Counters 
    Bytes Written=2999944
18/03/03 07:52:55 INFO mapreduce.ImportJobBase: Transferred 2.861 MB in 16.6491 seconds (175.9633 KB/sec)
18/03/03 07:52:55 INFO mapreduce.ImportJobBase: Retrieved 68883 records.
~~~

* Verify orders data under HDFS

~~~
hadoop fs -ls /user/cloudera/sqoop_import/orders

Found 5 items
-rw-r--r--   1 cloudera cloudera          0 2018-03-03 07:52 /user/cloudera/sqoop_import/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera     741614 2018-03-03 07:52 /user/cloudera/sqoop_import/orders/part-m-00000
-rw-r--r--   1 cloudera cloudera     753022 2018-03-03 07:52 /user/cloudera/sqoop_import/orders/part-m-00001
-rw-r--r--   1 cloudera cloudera     752368 2018-03-03 07:52 /user/cloudera/sqoop_import/orders/part-m-00002
-rw-r--r--   1 cloudera cloudera     752940 2018-03-03 07:52 /user/cloudera/sqoop_import/orders/part-m-00003
~~~

