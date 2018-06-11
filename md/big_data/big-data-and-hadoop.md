# Big Data and Hadoop

-

## Concepts

* What is Hadoop
* The Hadoop Ecosystem
* HDFS
* Resource Negotiation
* MapReduce

-

### Why Hadoop

* Search for data amongst large datasets
* Scale systems linerally
* Allow for safe failure

-

### Why Hadoop

Larger Datasets take space. Do we:

1. Keep getting larger and larger harddrives to store the data?
2. Add additional drives to the server?
3. Make more servers with additional drives?

-

### Why Hadoop - Larger Drives

Pros:

* Allows you to predict when upgrades need to happen
* Easy to maintain system

Cons:

* Does not scale linearly
* Disk read times become slower as drives scales
* Creating a single point of failure

-

### Why Hadoop - Additional Drives

Pros:

* Create space without increasing disk read time
* Drives can be swapped out and data can be copied

Cons:

* Drives have to be sequentially searched
* One CPU will handle traffic

-

### Why Hadoop - Distributed Servers

Pros:

* Servers can be generated cheaply
* Data can be copied between servers
* No single point of failure possible

Cons:

* Data must be searched for through each server
* Access from multiple servers can cause collision if multiple writes occur simultaniously
* System is difficult to maintain and code would have to be modified in the middle tier to service the data tier

-

### The Solution

Hadoop - An ecosystem of technology that distributes data and naturally scales out using distributed nodes. Hadoop will handle resource negotiation so a middle tier service wont have to be modified to search through larger data sets. 

-
-

## The Ecosystem

-

### HDFS

Hadoop Distributed File System

* How Hadoop stores data
* Connects data from multiple sources in one access point
* Allows for data replication within one cluster of systems

-

### YARN 

Yet Another Resource Negotiater

* Searches for data across multiple nodes
* Allows distribution of server load across available nodes
* Confirms system meets ACID compliance standards
* Balances load of backend system

-

### MapReduce

* Allows for quick scanning for data
* Sorts and organizes data for viewing
* Reduces the amount of data that travels over a network

-

### The Basic System

The distributed file system holds data and keeps track of the sources of that data and allows for data being backedup

YARN solves how data should be processed most efficiently

MapReduce scans for data and figures out the most effective way to compress and send data

-
-

## Hadoop Community

-

### Building on Hadoop

Hadoop works as a technology but can be hard to work with. Certain steps (such as resource negotiation) might have a better tool for your specific use case 

You may also decide to work with a technology to interact with Hadoop indirectly. Some of these technologies might handle query requests to Hadoop or they could even handle things like failover

-

### Mesos

![logo](https://raw.githubusercontent.com/Zipcoder/reveal-slides/master/img/ApacheMesosLogo.png)

* Apache project for Resource Negotiation
* Works on top of HDFS

-

### TEZ

![logo](https://raw.githubusercontent.com/Zipcoder/reveal-slides/master/img/ApacheTezLogo_lowres.jpg)

* Apache project that uses its own special mapping different from MapReduce
* Works on top of both Yarn and Mesos

-

### Spark

![logo](https://raw.githubusercontent.com/Zipcoder/reveal-slides/master/img/spark.jpg)

* Another Apache Product that replaces MapReduce
* Key features designed for handling distributed Machine Learning

-

### Pig

![logo](https://raw.githubusercontent.com/Zipcoder/reveal-slides/master/img/pig.png)

* Allows for query like scripts against Yarn
* Uses a SQL like language that gets translated before being sent to Hadoop

-

### Hive

<img src="https://raw.githubusercontent.com/Zipcoder/reveal-slides/master/img/Hive.png" alt="logo" width="300">

* Allows for SQL queries against plain text files in Hadoop
* SQL syntax that can treat flat files like database tables

-

### Many More! 

There are managers for failover like Zookeeper or Oozie. There are Web UI's for interacting with your cluster like Apache Ambari. There are Data Ingestion tools like Kafka.

You can also integrate standard data stores like MySQL and MongoDB. The limits of Hadoop have not been found yet!