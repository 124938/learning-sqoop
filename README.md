## Introduction

### What is Sqoop?
* Sqoop is a tool designed to transfer data between Hadoop and relational database servers. 
* Sqoop is used to import data from RDBMS such as MySQL, Oracle to Hadoop HDFS, and export from Hadoop file system to RDBMS.
* Sqoop uses MapReduce to import and export the data, which provides parallel operation as well as fault tolerance.

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

## Configuration/Verification

### Step-1: Pre-Requisite
* Cloudera QuickStart VM should be up & running (Click [here](https://github.com/124938/learning-hadoop-vendors/tree/master/cloudera/_1_quickstart_vm/README.md) to know more details on it)
    
### Step-2: Login to Quick Start VM OR Gateway Node of hadoop cluster

~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Wed Jan  3 02:59:27 2018 from 192.168.211.1

[cloudera@quickstart ~]$
~~~

### Step-3: Important configuration files of Sqoop

~~~
[cloudera@quickstart ~]$ cd /etc/sqoop/conf
[cloudera@quickstart conf]$ ls -ltr
total 28
-rwxr-xr-x 1 root root 6044 Jun 29  2017 sqoop-site-template.xml
-rwxr-xr-x 1 root root 1345 Jun 29  2017 sqoop-env-template.sh
-rwxr-xr-x 1 root root 1404 Jun 29  2017 sqoop-env-template.cmd
-rwxr-xr-x 1 root root 3895 Jun 29  2017 oraoop-site-template.xml
-rwxr-xr-x 1 root root 6044 Jun 29  2017 sqoop-site.xml

[cloudera@quickstart conf]$ 
~~~

### Step-4: Verification of Sqoop

~~~
[cloudera@quickstart conf]$ sqoop help
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

[cloudera@quickstart conf]$ 
~~~
