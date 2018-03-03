## Sqoop Eval

### Objective
* Sqoop eval tool executes provided query against the database server.

### Login to Quick Start VM OR Gateway Node of hadoop cluster

~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Wed Jan  3 02:59:27 2018 from 192.168.211.1

[cloudera@quickstart ~]$
~~~

### Syntax

~~~
[cloudera@quickstart ~]$ sqoop eval --help
Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/03 01:30:40 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
usage: sqoop eval [GENERIC-ARGS] [TOOL-ARGS]

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

SQL evaluation arguments:
-e,--query <statement>    Execute 'statement' in SQL and exit

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
~~~

## Examples

### (1) Execute `SELECT` query:**

~~~
asus@asus-GL553VD:~$ sqoop-eval \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--query "SELECT * FROM departments"

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/03 01:34:13 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/03 01:34:13 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/03 01:34:13 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
--------------------------------------
| department_id | department_name      | 
--------------------------------------
| 2           | Fitness              | 
| 3           | Footwear             | 
| 4           | Apparel              | 
| 5           | Golf                 | 
| 6           | Outdoors             | 
| 7           | Fan Shop             | 
--------------------------------------
~~~

### (2) Execute `CREATE TABLE` query (DDL statement):**

~~~
asus@asus-GL553VD:~$ sqoop-eval \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--query "CREATE TABLE TMP(i INT)"

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/03 01:39:25 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/03 01:39:25 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/03 01:39:25 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/03 01:39:26 INFO tool.EvalSqlTool: 0 row(s) updated.
~~~

~~~
asus@asus-GL553VD:~$ sqoop-eval \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--query "SHOW TABLES"

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/03 03:43:27 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/03 03:43:27 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/03 03:43:27 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
------------------------
| Tables_in_retail_db  | 
------------------------
| TMP                  | 
| categories           | 
| customers            | 
| departments          | 
| order_items          | 
| orders               | 
| products             | 
------------------------
~~~

### (3) Execute `INSERT` query (DML statement):**

~~~
asus@asus-GL553VD:~$ sqoop-eval \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--query "INSERT INTO TMP VALUES(1)"

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/03 01:41:11 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/03 01:41:11 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/03 01:41:11 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/03 01:41:11 INFO tool.EvalSqlTool: 1 row(s) updated.
~~~

~~~
asus@asus-GL553VD:~$ sqoop-eval \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--query "SELECT * FROM TMP"

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/03 01:42:09 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/03 01:42:09 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/03 01:42:09 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
---------------
| i           | 
---------------
| 1           | 
---------------
~~~

### (4) Execute `DROP TABLE` query (DDL statement):**

~~~
asus@asus-GL553VD:~$ sqoop-eval \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--query "DROP TABLE TMP"

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/03 01:42:41 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/03 01:42:41 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/03 01:42:41 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
18/03/03 01:42:42 INFO tool.EvalSqlTool: 0 row(s) updated.
~~~

~~~
asus@asus-GL553VD:~$ sqoop-eval \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--query "SHOW TABLES"

Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/03 01:43:28 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
18/03/03 01:43:28 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/03/03 01:43:28 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
------------------------
| Tables_in_retail_db  | 
------------------------
| categories           | 
| customers            | 
| departments          | 
| order_items          | 
| orders               | 
| products             | 
------------------------
~~~
