## Introduction

### Typical Life Cycle of data processing in Hadoop
* Data Ingestion - Few most popular tools used to ingest data from different sources to hadoop file system (HDFS) are:
  * Sqoop
  * Flume
  * Kafka
* Data Processing - Followings are few popular framework/tools/technology used to process data in batch/stream fashion:
  * Map Reduce
  * Spark
  * Flink
  * Storm
* Data Visulization - Below are few most popular BI/Reporting tools use to get insight on processed data are:
  * Tableau
  * Qlikview
* Data Exporting - Many times processed data gets exported to RDBMS or No-SQL database
  * Sqoop 

### What is Sqoop?
* Sqoop is a tool designed to transfer data between Hadoop and relational database servers. 
* Sqoop is used to import data from RDBMS such as MySQL, Oracle to Hadoop HDFS, and export from Hadoop file system to RDBMS.
* Sqoop uses MapReduce to import and export the data, which provides parallel operation as well as fault tolerance.
* Sqoop named from "**SQ**L to Had**oop** & Had**oop** to **SQ**L"

### How Sqoop Works?
* Below diagram describes the working model of Sqoop.
    
  ![Alt text](_images/sqoop_work.jpg?raw=true "Sqoop Working Model")  

### Features
* Connectors for all major RDBMS Databases
* Full Load
* Incremental Load
* Import results of SQL query
* Load data directly into HIVE/HBase
* Parallel import/export
* Compression

## Pre-Requisite
* Cloudera QuickStart VM should be up & running (Click [here](https://github.com/124938/learning-hadoop-vendors/tree/master/cloudera/_1_quickstart_vm/README.md) to know more details on it)

## Verification
 
### Step-1: Login to Quick Start VM OR Gateway Node of hadoop cluster

~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Wed Jan  3 02:59:27 2018 from 192.168.211.1

[cloudera@quickstart ~]$
~~~

### Step-2: Verification of sqoop binaries

~~~
[cloudera@quickstart conf]$ cd /usr/lib/sqoop/bin/
[cloudera@quickstart bin]$ ls -ltr
total 88
-rwxr-xr-x 1 root root 1564 Jun 29  2017 stop-metastore.sh
-rwxr-xr-x 1 root root 3987 Jun 29  2017 start-metastore.sh
-rwxr-xr-x 1 root root 3060 Jun 29  2017 sqoop-version
-rwxr-xr-x 1 root root 3062 Jun 29  2017 sqoop-metastore
-rwxr-xr-x 1 root root 3058 Jun 29  2017 sqoop-merge
-rwxr-xr-x 1 root root 3064 Jun 29  2017 sqoop-list-tables
-rwxr-xr-x 1 root root 3067 Jun 29  2017 sqoop-list-databases
-rwxr-xr-x 1 root root 3056 Jun 29  2017 sqoop-job
-rwxr-xr-x 1 root root 3069 Jun 29  2017 sqoop-import-mainframe
-rwxr-xr-x 1 root root 3070 Jun 29  2017 sqoop-import-all-tables
-rwxr-xr-x 1 root root 3059 Jun 29  2017 sqoop-import
-rwxr-xr-x 1 root root 3057 Jun 29  2017 sqoop-help
-rwxr-xr-x 1 root root 3059 Jun 29  2017 sqoop-export
-rwxr-xr-x 1 root root 3057 Jun 29  2017 sqoop-eval
-rwxr-xr-x 1 root root 3070 Jun 29  2017 sqoop-create-hive-table
-rwxr-xr-x 1 root root 3060 Jun 29  2017 sqoop-codegen
-rwxr-xr-x 1 root root 1055 Jun 29  2017 sqoop.cmd
-rwxr-xr-x 1 root root 3133 Jun 29  2017 sqoop
-rwxr-xr-x 1 root root 6533 Jun 29  2017 configure-sqoop.cmd
-rwxr-xr-x 1 root root 7486 Jun 29  2017 configure-sqoop
~~~

### Step-3: Verification of sqoop configurations

~~~
[cloudera@quickstart ~]$ cd /etc/sqoop/conf
[cloudera@quickstart conf]$ ls -ltr
total 28
-rwxr-xr-x 1 root root 6044 Jun 29  2017 sqoop-site-template.xml
-rwxr-xr-x 1 root root 1345 Jun 29  2017 sqoop-env-template.sh
-rwxr-xr-x 1 root root 1404 Jun 29  2017 sqoop-env-template.cmd
-rwxr-xr-x 1 root root 3895 Jun 29  2017 oraoop-site-template.xml
-rwxr-xr-x 1 root root 6044 Jun 29  2017 sqoop-site.xml
~~~

### Step-4: Verification of sqoop command

~~~
[cloudera@quickstart ~]$ sqoop help
Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/02 07:15:56 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
usage: sqoop COMMAND [ARGS]

Available commands:
  codegen            Generate code to interact with database records
  create-hive-table  Import a table definition into Hive
  eval               Evaluate a SQL statement and display the results
  export             Export an HDFS directory to a database table
  help               List available commands
  import             Import a table from a database to HDFS
  import-all-tables  Import tables from a database to HDFS
  import-mainframe   Import datasets from a mainframe server to HDFS
  job                Work with saved jobs
  list-databases     List available databases on a server
  list-tables        List available tables in a database
  merge              Merge results of incremental imports
  metastore          Run a standalone Sqoop metastore
  version            Display version information

See 'sqoop help COMMAND' for information on a specific command.
~~~

~~~
[cloudera@quickstart ~]$ sqoop version
Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
18/03/02 08:28:57 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
Sqoop 1.4.6-cdh5.12.0
git commit id 
Compiled by jenkins on Thu Jun 29 04:30:40 PDT 2017
~~~

### Step-5: Verification of MySQL

~~~
[cloudera@quickstart ~]$ mysql -h quickstart.cloudera -P 3306 -u retail_dba -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 472
Server version: 5.1.73 Source distribution

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| retail_db          |
+--------------------+
2 rows in set (0.00 sec)

mysql> use retail_db;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
~~~


