---
path: "./2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-9_Databases.md"
date: "2020-03-09T18:02:00"
title: "AWS Certified Solutions Architect Associate - Part 9 - Databases"
description: "Easy to understand tutorial: AWS Certified Solutions Architect Associate - Part 9 - Databases"
lang: "en-us"
---

### Database Design ###

#### Database types ####

- Hosted services. Services hosted and managed by AWS
  * Relational
  * Non-relational (NoSQL) only DynamoDB

- Custom instance installs - Bring your own license (BYOL)

Hosted Services

- AWS Relational Database Service (RDS)
  * Aurora MySQL  
  * Aurora PostgreSQL  
  * Oracle  
  * SQL Server
  * MySQL  
  * PostgreSQL  
  * MariaDB  

Custom Instance
- Start an instance with the required OS and install the database service:
  * Install via executable.
  * Install via ISO image
- Start an instance with the required OS and database service

Flat File vs Relational Databases
- Flat file databases (Think of an excel spreadsheet with just one worksheet)
  * One line per record
  * Doesn't contain multiple tables
- Relational databases
  * Store portions of data in tables
  * Tables are related based on unique identifier

NoSQL
- Not based on SQL or relational design theory
- Design supports fast transactions
- DynamoDB is a NoSQL service

Data Warehouses
- Large, central repository for data
- Data aggregated from one or more sources
- Used for Online Analytical Processing (OLAP)
- Redshit used within AWS

#### Relational Databases ####

Relational Database Terminology
- Rows - tuples
- Columns - attributes
- Tables - relations, entities, objects
- Views/results could be created (A view is a saved set of results)

Relationships
- Primary key (A column in the current table where every entry is unique so we can uniquely identify the associated row)
- Foreign key (A column in the related table representing the primary key)
- Join

Normalization
- Process for evaluating and correcting structures in a database
  * Determines the best assignments of attributes to entities
- Works through a series of stages called normal forms
  * 1NF, 2NF, 3NF, 4NF (optional)
- The higher the normal form (closer to 4NF) the slower the reads and faster the writes

#### Database hosting methods ####

EC2 instance based hosting - Total control, manual performance mgt and updates
- Launch the instance
- Install the database
- Create the database and connect to it

AWS Service Based - Less control, automatic performance mgt and updates
- Create the database
- Connect to the database

Considerations when choosing hosting methods:
- control - performance management - update procedure

#### High availability solutions ####

Clustering
- Multiple servers (instances) in a group we call a cluster
- One database with replication
- Increases availability
- Automatic failover
- Load balancing + clustering involves (called Active Active Cluster where both
servers are equally active) and helps increase both performance and availability.

Standby Instances
Standby instances only become active in event of failure of primary instance
- Multiple servers (instances) in a group we call a cluster
- One database with replication
- Increases recoverability
- No automatic failover
- Reduced costs

Sinle AZ deployment
- One instance in one AZ in one region

Multi AZ deployment
- Multiple instances in m AZs in one region
- Replicated storage
  * Increased availability
  * Increased performance
- Cost

#### Scaling solutions ####

Scalability e.g by increasing the class of your EC2 instance
- Increase capacity
  * Storage
  * Processing
  * Networking (e.g throughput)

Scaling the Instance
- Change the instance type/class
- Auto scaling is not supported in RDS but supported when using own EC2 instances
- In RDS auto scaling could be achieved by scripting with CLI commands

Read Replica
- Read - only copy of the database
- Offloads read-only traffic from the main database
- Multiple instances could be in different regions

#### Database securty ####

Encryption
- RDS databases support 'at rest' encryption (Encrypt the data while in storage)
- Must be enabled at creation time
- Can be enabled on recovery (manually)

Permissions
- AWS Administration access based on IAM
- Data access based on database capabilities
- Database admin (not AWS admin)  

Creating a database admin user

- Browse to AWS Management Console -> IAM console
- Click add user
- Give a name e.g DbAdmin
- Give Management console access
- Set permissions
- Click on attach existing policies
- Search for RDS
- Select RDS full access
- Review and create user

#### Aurora ####

- Relational DB
- Optimized for Online Transaction Processing (OLTP) mean very fast writes
- MySQL compatible DB system
- Good performance (up to 5 times better than MySQL)
- Inital 10GB size and scale by 10GB increments up to max of 64TB
- Compute resources of max 32CPUs and 244BiB RAM

Aurora Availability

- Minimum of 6 copies and maximum of 15
  * 2 DB copies in each AZ
  * Minimum of 3 AZs
- Write capability continues with up to 2 copies lost
- Read capability continues with up to 3 copies lost

Aurora Replicas
- Up to 15 Aurora replicas with automatic failover
- Up to 5 MySQL read replicas (No automatic failover

#### Redshift ####

An Online Analytical Processing (OLAP) database.

OLAP DBs give very fast read capabilities

- AWS managed
- Data warehouse DB
- Optimized for OLAP
- Pricing
  * Entry point of $0.25/hr
  * $1,000 per TB/yr
- Single or Multinode
- Single node - 160 GB
- Multi node
  * Leader node - connections and queries submitted
  * Compute node - store data and execute queries and calculations

Redshit speed
- Columnar data stores
  * Sequential reads
  * Very fast reads
- Data compression
- Massively Parallel Processing (MPP)

Reshift Security
- SSL transit encryption
- AES - 256 storage encryption
- Keys for both encryption above managed through AWS Key Management

Redshit Availability
- Operates in one AZ
- Snapshots can be restored to new AZs

Remember that data warehouse DBs are usually built from different data sources.
This means that the data ware house DBs will not critically need back up and
restore like OLTP DBs do. Also, data ware house DBs could be rebuilt from the
data sources.

#### DynamoDB ####

DynamoDB is a fully managed DB service. We don't even create database only tables etc

- NoSQL database service
- Provides special features
  * Millisecond latency at any scale
    . Very fast read/write
  * Stored on SSD
  * Spread accross 3 distinct data centers   

Read consistency types
DynamoDB uses eventual consistency
- Eventual consistent reads - lower latency, possibly stale data
- Strongly consistent reads - higher latency, up to date data

DynamoDB Pricing
- Storage - $0.25/GB per month
- Throughput
  * Write billed per hour for every 10 units
  * Read billed per hour for every 50 units
  * One unit = 1 transaction per second
  * Round up to the next 10 e.g 11 transactions per second will be billed 20 units

### Database Deployment ###

#### DynamoDB tables ####

DynamoDB is a fully managed DB service. We don't even create database only tables etc

Browse to AWS Management Console -> Databases -> DynamoDB
- Create DynamoDB table
  * Table name
  * Primary key (partition key) enter the name and select the type e.g number
  * Add sort key if needed
  * If you de-select ```use default settings``` you get much configuration options
  e.g encryption at rest, autoscaling etc
After creating the table, go into the table and begin adding elements
- Create item
- Set the user id
- Click the plus ```+``` sign to add fields/columns to the table

DynamoDB - access control through polices used to control permissions within the database

#### MySQL ####

MySQL database can be implemented for free

- AWS AWS Management console -> Database -> RDS
- Create Database
- Select type - e.g MySQL
- Sepcify DB details
  * License model
  * Database versions
  * Choose instance class (Not applicable free tier)
  * Multi AZ deployment (Not applicable to free tier
  * Storage type
  * Allocate storage
  * Database identifier
  * Master username
  * Master password
- Cofigure advanced settings
  * Select VPC
  * Select subnet groups
  * Specify whether the DB will be publicly accessible
    . A web server in the same VPC - No
    . A web server in another location e.g outside AWS - Yes
  * Create a new VPC security group or select existing. Recommended to let it
  create a new one because it creates the rules needed for the particular DB
  system you selected.
  * Port - 3306
  * Leave default DB parameter group
  * Enable encryption (Not applicable to free tier)
Backup (This section refers to automated, not manual backups you create yourself)
  * Retention period - 7 days default
  * Backup window (Better to select periods of expected low usage)
Enable deletion protection - Recommended to enable this for databases

#### Configuration ####

Remember that each database in RDS has an EC2 instance. The instance is however
not displayed in the EC2 dashboard. The instance is fully managed by AWS RDS.

All configurations specified during creation could be modified after creation???
To modify an existing database
- Browse the AWS Management Console -> Databases -> RDS
- Click on instances
- Click the instance to be modified
- Click ```Instance actions``` -> Choose ```Modify```
- You could change everything??? you configured at creation time, e.g:
  * AZ deployment - single/multiple
  * Storage size
  * Db instance identifier
  * Master password
- Click modify DB instance - the change may take a while

To create a read replica of an existing database
- Browse the AWS Management Console -> Databases -> RDS
- Click on instances
- Click the instance to be replicated
- Click ```Instance actions``` -> Choose ```Create read replica```

- Many actions like update configuration or create read replica cannot be carried
out while the database is undergoing maintenance (e.g when read replica being
created)

- For databases use dns connection parameters as opposed to IP address because dns
name applies to all copies of the database.

#### Backups ####

Automated backups are created by modifying configuration

- Browse the AWS Management Console -> Databases -> RDS
- Click on instances
- Click the instance to be automatically backed up
- Click ```Instance actions``` -> Choose ```Modify```
- Under the backup section set backup criteria
  * Retention period
    . Minimum = 0, default = 7, maximum = 35
    . Zero disables automatic backups
    . If the db is a temporary db for analytics then you could set zero

#### Restore ####

Two types of restoration:
- From automated backup
- From snapshots you created

When you restore, you are restoring to a new database instance

To restore form an automated backup
- Browse the AWS Management Console -> Databases -> RDS
- Click on instances
- Click the instance to be restored
- Click ```Instance actions``` -> Choose ```Restore to point in time```

#### Snapshot ####

Manual backups are created through snapshots

To create a snapshot of an existing database
- Browse the AWS Management Console -> Databases -> RDS
- Click on instances
- Click the instance
- Click ```Instance actions``` -> Choose ```Take snapshot```
- Specify name (recommended to include name with full date and time)

To share a snapshot
- Browse the AWS Management Console -> Databases -> RDS
- Click on instances
- Click the instance to be shared
- Click ```Instance actions``` -> Choose ```Share snapshot```

Clean up snapshots when no-longer needed

#### Monitoring ####

- Performance
- That we are not over utilizing a single instance
- Decide if we need read replicas because performance is reducing

To enable enhanced performance/performance insights
- Browse the AWS Management Console -> Databases -> RDS
- Click on instances
- Click the instance
- Click ```Instance actions``` -> Choose ```Modify```
- Enable enhanced performance/peformance insights

To cluster a single database, restore a backup of the database to a cluster of
databases

### Notes ###

- The higher the normal form (closer to 4NF) the slower the reads and faster the writes
- Tables in a relational database are linked based on primary and foreign keys
- Considerations when choosing hosting methods: control, performance management, update procedure
- Auto scaling is not supported in RDS but supported when using own EC2 instances
- In RDS auto scaling could be achieved by scripting with CLI commands
- RDS databases supports (at rest) encryption only at creation time or during recovery
- 6 - 15 Aurora replicas with automatic failover
- Up to 5 MySQL read replicas (No automatic failover
- Aurora performance (up to 5 times better than MySQL)
- Aurora inital size of 10GB and scale by 10GB increments up to max of 64TB
- Aurora compute resources of max 32CPUs and 244BiB RAM
- Redshift single node can store max of 160GB
- DynamoDB is a fully managed DB service. We don't even create database only tables etc
- DynamoDb is a NoSQL database which uses eventual consistency model
- DynamoDB - access control through polices used to control permissions within the database
- DynamoDB can be installed outside AWS - [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal/ "DynamoDB Local")
- Eventual consistent reads - lower latency, possibly stale data
- Strongly consistent reads - higher latency, up to date data
- MySQL database can be implemented for free
- Aurora DB is the only relational DB on AWS that isn't free tier
- For databases use dns connection parameters as opposed to IP address because dns
name applies to all copies of the database.
- Database retention period - minimum = 0, default = 7, maximum = 35
- When you restore, you are restoring to a new database instance
- Automated backups are created by modifying configuration
- Manual backups are created through snapshots
- Snapshot - copy and Snapshot - shares to transfer a database to another location
- To cluster a single database, restore a backup of the database to a cluster of
databases

### Acronyms ###

- BYOL - Bring Your Own License
- RDS - Relational Database Service
- SQL - Structured Query Language
- OLAP - Online Analytical Processing
- OLTP - Online Transaction Processing
- MPP - Massively Parallel Processing
