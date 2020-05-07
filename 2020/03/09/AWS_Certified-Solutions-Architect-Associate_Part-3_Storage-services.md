---
path: "./2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-3_Storage-services.md"
date: "2020-03-09T10:43:00"
title: "AWS Certified Solutions Architect Associate - Part 3 - Storage services"
description: "Easy to understand tutorial: AWS Certified Solutions Architect Associate - Part 3 - Storage services"
lang: "en-us"
---

### Acronyms ###

- ACL - Access Control List
- ARN - Amazon Resource Name
- AZ - Availability Zone
- EBS - Elastic Block Store
- IA - Amazon S3 Infrequent Access
- MFA - Multi-Factor Authentication
- NAS - Network Attached Storage
- NFSv4 - Network File System version 4
- Provisioned IOPS - GUARANTEED Input/output operations per seconds
- REST - Representational State Transfer
- RRS - Reduced Redundancy Storage
- S3 - Simple Storage Service
- SNS - Simple Notification Service
- SSD - Solid State Drive
- URL - Uniform Resource Location

### Storage Services ###

- __Simple Storage Service__ (Object level access)

- __Glacier__ - For archive data

- __CloudFront__ - Data accessed frequently is cached at an edge location

- __Elastic Block Store__ - Very fast access (block level access)

- __Storage Gateway__ - An appliance you put on your local network that acts as
a VPN connection into the amazon clould so you can access your storage as if its
local storage.

- __Snow Family__ - A collection of primary products used to migrate large
amounts of data from local data stores into the cloud.

- __Databases__

### Characteristics of Storage ###

- __Block Storage__

  * Block Storage is used on local networks. Accessing the data in a similar way to local hard drives. e.g iSCSI, Fibre channel.

  * AWS can use block storage with virtual machines within the AWS cloud using EBS.

- __File Storage__

  * Here we are dealing with objects, or chunks of information.

  * AWS uses similar called object storage in S3

  * Used in Network Attached Storage (NAS) devices locally.

### Selecting Storage ###

- __Size__. How big are the objects and how much total storage is needed

- __Performance__. Speed of access (EBS gives better performance for instances) but don't forget cost.. which becomes obvious for large amounts of data.

- __Cost__

### Simple Storage Service (S3) ###

- `Object Storage`. S3 is about object storage. The object could be a file or any chunk of data.

- `High availability`. Automatically distributed across at least 3 availability
zones (AZ) (except One Zone IA) by default.

- `Encryption`. Objects are encrypted using server-side encryption with either Amazon S3-managed keys (SSE-S3) or customer master keys (CMKs) stored in AWS Key Management Service (AWS KMS).

- Automatic data classification

- Big data analytics could be done directly against the data stored in an S3 bucket. This means you don't have to put it in a database first.

- `Billing`. You pay for storing objects in your S3 buckets. The rate you’re
charged depends on your objects' size, how long you stored the objects during
the month, and the storage class. There are also per-request ingest fees when
using PUT, COPY, or lifecycle rules to move data into any S3 storage class.
When using Transfer Acceleration, additional data transfer charges may apply.
You pay for all bandwidth into and out of Amazon S3, except for the following:
  * Data transferred in from the internet
  * Data transferred out to an Amazon Elastic Compute Cloud (Amazon EC2) instance, when the instance is in the same AWS Region as the S3 bucket
  * Data transferred out to Amazon CloudFront (CloudFront)
  * Data transferred using Amazon S3 Transfer Acceleration.

- `Object Locking`. Amazon S3 does not currently support object locking. If two
PUT requests are simultaneously made to the same key, the request with the latest timestamp wins. If this is an issue, you will need to build an object-locking mechanism into your application.

- `IPv6`
  * Static website hosting from an S3 bucket is not supported over IPv6.
  * BitTorrent is not supported over IPv6

- `Transfer Acceleration` Enables fast, easy, and secure transfers of ﬁles over
long distances between your client and an S3 bucket. Transfer Acceleration takes advantage of Amazon CloudFront’s globally distributed edge locations. As the
data arrives at an edge location, data is routed to Amazon S3 over an optimized network path. When using Transfer Acceleration, additional data transfer charges may apply.   

#### S3 Consistency Model ####

| Action          | Consistency
|-----------------|-----------------
| PUT new object  | Read after write
| PUT overwrite   | Eventual
| DELETE          | Eventual

#### Getting Data into S3 ####

- __AWS APIs__. API calls from your applications

- __Amazon Direct Connect__. A VPN from enterprise/business network to AWS

- __Storage Gateway__. With storage gateway, in addition to VPN connection,
there could also be data stored locally and synchronized/replicated into the S3.

- __Kinesis Firehose__. A way to get a large amount of analytical data into the S3 bucket.

- __Transfer Acceleration__. Works based on CloudFront tech. Optimized route
using edge location for uploading data to S3 bucket... Costs more.

- __Snow Family__. These are actual physical hardware.

  * Snowball. Petabyte scale.

  * Snowball Edge. 100 TB local storage. Bring the device into your facility and it has ability to run instances on itself. The device gets sent back to AWS for transfer to the cloud.

  * Snowmobile. Exabytes of data. A large trailer, each of which can store 100 petabytes of info. AWS staff come with the trailer and help with setup.

#### S3 Features ####

- __Prefixes and Delimiters__. S3 doesn't actually have folder hierarchy.
Organization and identification is achieved by prefixes and delimiters. With
prefixes and delimiters give   the impression of folders.

- __S3 Storage Classes__. From most expensive at the top to least expensive.

| Class                       | Retrieval fee | Availability (%) | Min capacity charge per object (KB) | Min storage duration charge (days)
|-----------------------------|---------------|------------------|-------------------------------------|-----------------------------------
| S3 Standard                 | N/A           | 99.99            | N/A   | N/A
| S3 Intelligent-Tiering      | N/A           | 99.9             | N/A   | 30
| S3 Standard - IA            | per GB        | 99.9             | 128KB | 30
| S3 One Zone - IA            | per GB        | 99.5             | 128KB | 30
| S3 Glacier                  | per GB        | 99.9             | 40KB  | 90
| S3 Glacier - Deep Archive   | per GB        | 99.9             | 40KB  | 180
| Reduced Redundancy Storage  |               |                  |       |

- All storage classes have 99.999999999 percent (11 9s) durability.

- `S3 Intelligent-Tiering`. The S3 Intelligent-Tiering storage class is designed
to optimize costs by automatically moving data to the most cost-effective
access tier, without performance impact or operational overhead.

_Though glacier is the least expensive, if accessed frequently then the price
shoots up due to the access charge_.

- __Object Lifecycle Management__

The standard is to add info to S3 Standard, migrate to S3 IA after some months
and further move the data to Glacier after some months.

- __Encryption__

Server side and Client side.

  * Server side. AWS does the 256 bit encryption on the server. Note it is   storage security not transit security.

  * Client side. You encrypt the file before uploading to S3.

- __Versioning__

Maintains multiple versions of objects. Turned off by default. Once you enable
it, you can't disable it. But you can suspend it so that new versions are no
longer created.

- __Multi-Factor Authentication (MFA)__

- __Multi-part upload__. Giga bytes of data upload in parts

- __Range GETs__. E.g get the range from 10kb to 20kb.

- __Cross region Replication__. Replicate data accross S3 buckets in different
regions. When enabled it doesn't replicate existing data but newly added data.

- __Logging__. Log addition, deletion, changes etc

- __Event Notifications__

#### S3 Bucket Usage ####

__Enabling Encryption (Server side encryption)__

Encryption Options are:

- AES Option - Managed by Amazon. Easier.
- KMS Option - Managed by you. More Flexible. You have the ability to archive or restore your key.

__Permissions__

- Bucket level permission
- Object level permission. Will override bucket level per object.

__Manage Lifecycle__

Tags or prefixes could be used to apply rules to a group of objects in a bucket.

- Storage class Transiton

  * To IA after 30 days
  * To Glacier after 90 days
  * You could also set for deletion after a set amount of time (It is advisable to have a backup)

__Bucket Policy__

- JSON policies could be done, but we have visual editors to help build JSON policies

- CORs - Cross Origin Resource sharing for web apps

- Others Management, Analytics, Metrics, Inventory

__Adding objects__

- Object duration is specified at creation time.

- Minimum size of an s3 bucket object is 0 bytes. You can add a zero byte file.

#### S3 Terminology ####

- Regions.
- Buckets.
- Objects.
- Keys. Objects have keys, like filenames for files
- Object URLs
- `Eventual consistency`. S3 objects have eventual consistency while Elastic
Block Store (EBS) objects are consistent. Eventual consistency means there is a
lag between when a new state is introduced on one location (originating location)
and when the new state becomes consistent with redundant locations.

#### Common S3 Operations ####

- Creating and deleting buckets
- Write, read, delete objects
- Manage object properties
- List keys in buckets

#### REST Interface ####

Representational State Transfer (REST) uses HTTP methods.

- Create - HTTP PUT or POST
- Read - HTTP GET
- Update - HTTP POST or PUT
- Delete - HTTP DELETE

### Glacier ###

- Glacier is for achiving data

- You could automatially provision for a glacier by adding a lifecycle rule to a bucket e.g move to glacier after 90 days

- If you want to manually put stuff in Glacier, you have to create your own vault.

  * Management Console -> Glaciers -> Create vault

  * You could enable notifications and create an SNS topic or use existing SNS topic. A topic is just that a topic... used to identify a queue e.g football, marketing etc

  * A topic has an Amazon Resource Name (ARN)

  * You may set permissions for a Vault. (You could use tags like in buckets)

  * You could use your vault with your storage gateway, if you have implemented one.

  * Settings

### Elastic Block Store (EBS) ###

- Each EC2 instance uses EBS. EBS used for durable/persistent storage in the instance.

- EBS is block level storage from one AWS service to another

- __EBS Volume Types__:

- SSD-backed volumes optimized for transactional workloads involving frequent
read/write operations with small I/O size, where the dominant performance
attribute is IOPS

- HDD-backed volumes optimized for large streaming workloads where throughput
(measured in MiB/s) is a better performance measure than IOPS

  * Magnetic, slowest, cheapest
    - Standard (54 rpm like in computer hard drive)
    - Throughput optimized (10k rpm +)
  * SSD
    - General Purpose
    - Provisioned IOPS

- Generally
  * Cold Hard Disk Drive - (Really large and really slow)
  * Throughput Optimized - (Potentially large, really fast) e.g 10000+ rpm
  * Magnetic Standard - (Middle of the road) like a typical 5400rmp hard drive found in local computers
  * If you use SSD, to take advantage of the additional performance, you need an EBS optimized instance.
  * EBS volume type: Magnetic Standard SSD is free tier.

__Protecting EBS Data__

- `Snapshots`.
  * Take a snapshot, then restore to that point in time later.
  * Take a snapshot, create another EBS volume from the snapshot. Create an
  instance and used the newly created EBS volume.

- `Volume recovery`
  * Attach volumes from one instance to another.

- Encryption methods

__Creating EBS Volumes__

_No link to EBS volumes under storages in AWS Services. EBS links are under EC2._

Management Console -> Compute -> EC2 -> Volumes -> Create Volume

- Range: 1 gigabyte - 1 terrabyte (1024 gigabytes)
- You want your EBS to be in the AZ that your instance is.
- Encrypt volume uses ASW EBS master key by default

After creating volume

Browse to: Actions -> Attach to instance

or

During instance creation attach the newly created EBS volume.

__EBS Volume Types__

![EBS Volume Types](https://secureservercdn.net/160.153.138.177/3d9.249.myftpupload.com/wp-content/uploads/2016/03/EBS_Volume_Types.png "EBS Volume Types")

### Elastic File System ###

- EFS is different from other storage because it is sharable. This means that
multiple devices can access it at the same time.
- EFS is heirarchical in nature, unlike S3 which uses prefix and delimiters
- EFS can be accessed through NFSv4 (Network File System version 4)
- EFS could be used by EC2 instances
- EFS not supported on windows instances
  For windows, create an EBS volume and use shared folders from the windows instance itself.

| Type                             | EFS                    	    | S3                  		       | EBS
|----------------------------------|------------------------------|--------------------------------|----------------------------------------
| `Performance per ops`            | low, consistent		          | low for mixed req & CloudFront | lowest, consistent 	
| `Thoughput scale`                | Multiple Gigabytes/sec	      | Multiple Gigabytes/sec	       | Single Gigabytes/sec
| `Data Availability & Durability` | Multiple AZ redundancy	      | Multiple AZ redundancy         | Single AZ & hardware based redundancy
| `Access`		                     | Thousands from multiple AZs  | Millions over the web		       | Single EC2 in single AZ
| `Use cases`		                   | Web serving, content mgt, enterprise apps, media & entertainment, home dirs, db backup, dev tools, container storage, big data analysis | Static website, content mgt, media & entertainment, backups, big data analytics, data lake | Boot volumes, transactional & NoSQL db, data warehouse & ETL

__Create EFS File System__

- Management Console -> Storage -> EFS -> Create EFS File System
  * Select VPC
  * Performance Mode
    - General Purpose
    - Max IO - Good to use when large number of access.
  * Throughput mode:
    - Bursting - Increase or decrease with demand.
    - Provisioned - fixed at a certain amount.

### Integrating Cloud with On-premises Storage ###

Storage Gateway - Software appliance that creates a gateway on the customers location. Storage gateway provides 3 types of storage solutions

- File based (uses NFS, unlike EFS it supports windows systems)
- Volume based (iSCSI protocol i.e SCSI over internet protocol)  
- Tape based

File gateway provides interface to S3 buckets.

__Must Read: AWS Storage Gateway -> Planning your storage gateway deployment__

### Storage Access Security ###

- Browse to: Management Console -> S3
- Click on an existing S3 bucket -> Permissions

To add permissions you need the person's email address or canonical id. Some users a created using only username. Also getting their canonical id may not be immediate. For this reason most use JSON based policy for security.

- Click on an existing S3 bucket -> Bucket Policy
- Take advantage of the policy generator if you don't want to type JSON directly.
- Set the following amongst others:
  * Type of Policy
  * Allow/Deny or both
  * Principal (the user's ARN). To get the ARN of the user:
    - Management Console -> Services -> IAM  -> Users -> Select paticulary user
    - The user's ARN will be displayed at the top.
  * Amazon Resource Name (ARN of the current resource)
- Copy the JSON code generated, go back to the S3 management console and paste the copied JSON policy.

Command Line Parameters/Tools Avaialable for S3 Buckets

- Browse to: S3 API Command

- You will find: Get Bucket ACL, Put Bucket ACL. These set permission on a bucket using Access Control Lists (ACL)

### AWS Documentation  and Notes ###

Storage Performance Management is about selecting the right type and class of storage.

__SSD vs HDD (Magnetic)__

- Amazon EBS Volume Type Performance. _Any question above 10k IOPs use provisioned SSD._

  * __SSD__
    - GP2 - General purpose - max 10k IOPs
    - Provisioned - max 32k IOPs

  * __HDD (Magnetic)__ - Various types have 250 - 500 IOPs (not high)

- Volume Size Constraints

  * __SDD__
    - General - 1GiB - 16TiB
    - Provisioned IOPs - 4GiB - 16TiB

  * __HDD (Magnetic)__ - Various types have  500GiB - 16TiB

The default of General Purpose SSD is usually adequate.

- __Difference between MB and MiB__

  * KB = 1000B, KiB =1024B
  * MB = 1000k, MiB = 1024k
  * GB = 1000B, KiB =1024B

- __Storage Class Differences__

  * S3 standard
    - Durability   - 99.999999999%
    - Availability - 99.99%
  * S3 standard IA
    - Durability   - 99.999999999%
    - Availability - 99.9%
  * S3 One Zone IA
    - Durability   - 99.999999999%
    - Availability - 99.5%

### Notes ###

- S3 is about object storage not just file storage

- There are different classes of S3 storage.

- Many other services can store data in S3 e.g Kinesis, Firehose.

- Once versioning is turned on you can only suspend it for new S3 objects.

- Replication does not apply to existing data in an S3 bucket.

- Use DNS compliant names for buckets

- S3 bucket names must be globally unique

- All Amazon S3 resources and sub-resources are private by default, but you can
configure security features, such as access control lists (ACLs) and bucket
policies, to allow public access to your buckets or objects.

- When you see 1 grantee under permission, the obviously it is the owner who
has permission.

- Default max amount of bucket is 100

- Default max amount of objects in bucket is infinity (theoritically)

- S3 good for static website hosting.

- Max number of tags for S3 object is 10.

- Files in vault are called archives from AWS Glacier perspective.

- What is the maximum number of vaults an AWS account can create in a region? 1000

- What is the expected recovery window for a Glacier restore with standard
access?
S3 Glacier provides three retrieval options that range from a few minutes to hours.

- To guarantee IOPS use SSD type provisioned IOPS

- If you use SSD, to take advantage of the additional performance, you need
and EBS optimzed instance.

- EBS volume type: Magnetic Standard SSD is free tier

- EFS shares can be limited to a VPC.

- Storage security could be managed through Management Console / CLI
