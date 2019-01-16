---
layout: post
title:  "AWS Notes - Database"
date:   2019-01-15 00:00:00 +1000
categories: tech blogs
---

### Aurora
* 5 times faster then MySQL 
* Start with 5 GB to 10 GB, 32vCpu, 244 GB memory
* 2 copies of data in one AZ, 3 AZs
* 2 types of replica, Aurora Replica, MySQL replica
* Up to 15 read replica
* Aurora PostgreSQL does not currently support cross-region replicas.

### RDS
* Automated backup is enabled by default
* Snapshots are created manually 
* Restore creates a new instance
* Multi AZs is for disaster recovery, read replica is to improve performance.
* Read replica is not available for SQL Server and Oracle database.
* Max size is 16TB for SQL Server.
* When RDS is doing a backup the IO may be briefly suspended.
* Changes to RDS backup window takes effect immediately.
* Up to 5 read replica 
* Encryption of existing RDB instance is not supported, you have to create a snapshot of the instance, encrypt the snapshot and restore from the encrypted copy. 
* Backup can be kept up to 35 days.
* Multi AZ use synchronous synchronisation for standby instance.
* Read replica use asynchronous synchronisation.
* Read replica can be in a different region.

### ElasticCache 
* Memcache 
* Redis 

### DynamoDB
* Value cannot exceeds 400kb.
* Spread 3 different data centers. It is automatically replicated across multiple AZs.
* By default it is eventual consistent read. It can support strongly consistent read with a higher cost.
* DynamoDB synchronously replicates data across three facilities in an AWS Region, giving you high availability and data durability.
* DynamoDB is designed to scale without limits. However, if you want to exceed throughput rates of 10,000 write capacity units or 10,000 read capacity units for an individual table, you must first  [contact Amazon](http://portal.aws.amazon.com/gp/aws/html-forms-controller/DynamoDB_Limit_Increase_Form) . If you want to provision more than 20,000 write capacity units or 20,000 read capacity units from a single subscriber account, you must first  [contact us](http://portal.aws.amazon.com/gp/aws/html-forms-controller/DynamoDB_Limit_Increase_Form)  to request a limit increase.

### Redshift
* A single node can have 160 GB data.
* Up to 128 compute node.
* Only support in one AZ.
* Columnar data storage. 