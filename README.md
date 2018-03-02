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

## Installation/Verification

### Pre-Requisite
* Cloudera QuickStart VM should be up & running (Click [here](https://github.com/124938/learning-hadoop-vendors/tree/master/cloudera/_1_quickstart_vm/README.md) to know more details on it)
    
### Login to Quick Start VM or Gateway Node of hadoop cluster using `ssh`

~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Wed Jan  3 02:59:27 2018 from 192.168.211.1

[cloudera@quickstart ~]$
~~~
