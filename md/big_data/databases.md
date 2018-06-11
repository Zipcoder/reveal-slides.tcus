# Databases

-

# Differences

* Governance - who controls their future
* Platform - where can it be run
* Support - what tools do you need to work with
* [ACID compliance](https://en.wikipedia.org/wiki/ACID)

-

### ACID compliance

* Atomicity
	- A transaction is all or nothing
* Consistency
	- Any transaction will bring a database from one valid state to another
* Isolation
	- Two concurrent transactions will be the same as if they were sequential
* Durability
	- once a transaction has been committed it is final no matter what

-
-

## Relational Databases

-

### MySQL 

- Open source but controlled by a single company
- Can run on all major operating systems
- Partions data with proprietary tech called MySQL Cluster
- Leader-Leader replication
- Not ACID compliant. Does not support Consistency or Durability 

-

### Postgres

- Open source and controlled by the PostgreSQL Global Development Group 
- Can run on all major operating systems
- Does not support true partitioning but can "replicate" the behavior
- Leader-Follower replication but third party plugins support all kinds of replication methods
- ACID compliant

-

### MariaDB 

- Fork of MySQL created by the original MySQL developers
- Governed by a small group of devlopers but uses the community governance model
- Can run on all major operating systems
- Supports partitioning via multiple methods through community plugins
- As of 5.3, MariaDB is ACID compliant. Earlier version are not.

-

### SQL Server

- Proprietary database engine from Microsoft
- Closed source and managed by Microsoft
- Can run on Windows Servers and home computers
- ACID compliant
- Supports partitioning via proprietary technology

-

### Redshift

- Proprietary database engine from AWS
- Managed by Amazon, closed source ish?
- Does not run on any OS. Data is stored by Amazon internally
- Architecture based around columnar storage
- Not ACID compliant, consistency as referential and unique constraints are not enforced

-
-

## Non-Relational Solutions

-

### Why NoSQL Solutions?

- Limitting structure allows for more fluid data
- In memory data store can be read incredibly fast
- Related data is stored together making joins unneeded
- Data isn't predefined, it can pivot with your app needs
- New features do not require new endpoints nor migration of any data
- Can be interfaced directly in code rather than with some Query Languange

-

### MongoDB 

- Free and open-source governed by the MongoDB organization
- Managed by the open source community
- Runs on all major operating systems
- Not ACID compliant. 
	* Cannot guarantee consistency because no references
	* File based or memory based store is not all or nothing
	* Data can be out of sync if transactions are run simultaneously
	* MongoDB doesn't run with commits so saves aren't guaranteed in the event of power outage or computer crashes

-

### CassandraDB

- Free and open-source by the Apache foundation
- Runs on all major operating systems
- Can be interfaced with a SLQ like language
- Not ACID compliant
	* Atomicity and Isolation are available on a row level but not on a transactional level

-

### Redis

- Open source in-memory data structure store
- Governed by the Redis organization
- Single threaded
- ACID compliant*
- Supports datatypes that can directly translate back to an object in code

