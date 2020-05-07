---
path: "./2020/04/14/AWS-Architect-6_Passing-the-Certification-Exam.md"
date: "2020-04-14T23:49:28"
title: "AWS Achritect - 6 - Passing the Certification Exam"
description: "A guide to help you pass the AWS Certified Solutions Architect Associate exam"
tags: ["AWS", "Certification", "Certified Solutions Architect", "Exam", "Architecting for Reliability", "Architecting for Performance Efficiency", "Architecting for Security", "Architecting for Cost Optimization", "Architecting for Operational Excellence"]
lang: "en-us"
---

### Acronyms ###

- DAX - DynamoDB Accelerator
- EBS - Elastic Block Store
- EC2 - Elastic Cloud Compute
- EFS - Elastic File System
- HDD - Hard Disk Drive
- IAM - Identity and Access Management
- RDS - Relational Database Service
- NFS - Network File System
- S3 - Simple Storage Service
- SSD - Solid State Drive

### Before we set off ###

__About this Article__

- Hi, this article is a guide to help you master AWS and pass the Certified
Solutions Architect - Associate exam.

- It is one of a series of articles.

- This is the sixth article in this series.

- [Click here for the previous article](????????????????????????????????????????).
To start at the beginning of the series, [click here](/2020/04/14/AWS-Architect-1_Architecting-for-Reliability).

- This article is an estimated 18 minute read.

__This Article is not an Introduction to AWS__

This series of articles is not an introduction to AWS or any of the core
concepts of the AWS Cloud. You need to be already familiar with some core
concepts of AWS Cloud to fully benefit from this article. In order words,
if you are not familiar with the AWS cloud, you should read the series of
articles beginning at:

- [Notes on Amazon Web Services - 1](/2020/03/02/Notes-on-Amazon-Web-Services_1_Introduction)

- [AWS Certified Solutions Architect Associate - Part 1](/2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-1_Key-services-relating-to-the-Exam)

### The Exam ###

- Initial pass rate for the AWS Certified Solutions Architect - Associate
exam is less than 25 percent.
- The exam has 65 questions
- No penalty for guessing
- Each question worth the same point
- 130 minutes
- You can skip and come back later
- Multiple choice with single answer as well as 2 answers.

__Hands on Experience__

- Hands on experience with AWS products is indispensable.

- qwiklabs.com is a good place to get hands on training.

### Areas of Concentration ###

__AWS Services to Concentrate on__

- Compute
- Storage
- Databases
- Network & Content Delivery
- Application Integration
- Desktop & App Streaming
- Management Tools
- Analytics
- Migration
- Security, Identity & Compliance

__Comparisons between various services__

- EBS vs EFS vs S3

- Systems Manager vs CloudWatchEvents + AWSConfig

System Manager enables you do a lot. However it is schedule based. This means
that we can set actions to take place at specific times. What happens when
we want an action to take place when, for example, a new EC2 instance is
launched? This is where `CloudWatch Events` and `AWS Config` come in.

__Service Limitations__

- Lamda limitations

__Service Limits__

- Subnets per VPC - 200
- Route tables per VPC - 200
- VPCs per region - 5
- IPV4 CIDR blocks per VPC - 5
- Elastic IP addresses per region - 5
- Egress-only internet gateways per Region - 5
- Internet gateways per Region - 5
- [View more AWS VPC Limits](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits/)

__Design architectures__

- Distributed Systems

- Decoupled architectures

- Sample architectures

- Sample templates

__Study Official Documentation__

- Service documentations - `Final word`
- FAQs
- AWS Whitepapers.

  * Start by reading the following:

    - Overview of Amazon Web Services
    - Architecting for the Cloud: AWS Best Practices
    - AWS Security Best Practices

  * Also read the following

    - AWS Storage Services Overview
    - AWS Well Architected Framework
    - Overview of Security Processes

- AWS Event Videos

  * https://www.youtube.com/user/AmazonWebServices
  * Level 300 (intermediate) and 400 (advanced) vides
  * Good video: Another Day, Another Billion Packets

- AWS Architecture Centre

  * https://aws.amazon.com/architecture
  * CloudFormation templates

- AWS Answers

  * https://aws.amazon.com/answers

- qwiklabs.com -> Very good practice site

- Personal projects

  * Build a WordPress site using EC2/RDS
  * Create a custom VPC with a NAT Gateway
  * Deploy a website using Amazon S3 and CloudFront
  * Create a Lambda function to backup EBS volumes
  * Use Elastic Beanstalk to deploy a PHP website
  * Create a CloudFormation template to automate one or more of the above
  projects

__Exam Blue Print__

### Design Resilient Architecture ###

__Storage Types__

- EC2 instance store is only supported by a limited EC2 instance types.
  It has a limited capacity
  It is extremely fast
  It is temporary (aka ephemeral) i.e only exists for the life of the instance

- Elastic Block Store
  It is network attach storage

- Elastic File System

  * EFS is a managed service
  * Elastic, so you don't have to define storage allocation at creation
  * Can be attached to multiple EC2 instances
  * Supports both NFS 4.0 and 4.1
  * As of 1 May 2020. [Using Amazon EFS with Microsoft Windows–based Amazon EC2 instances is not supported](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html).

- Amazon S3

__Decoupling Applications using AWS__

- Decouple for health/resilience using queues

- Decouple for scalability using queues. Use CloudWatch combined with SQL to
monitor the queue and make auto scaling changes

- Decouple for scalability using Elastic Load Balancers.

__CloudFormation__

__Auto Scaling__

__Lambda__

### Defining Performant Architectures ###

__Choose performant storage and databases__

- SSD dominates for random reads

- Multiple users download different files = random e.g web apps

- HDD provides much better performance for sequential reads

- Single user accessing large content = sequential e.g big data

- RDS - managed relational database

  * Know all 6 supported database engines.
  * Understand multi-AZ deployments with RDS
  * Understand how read replicas are used with RDS
  * Replication from master to standby is done syncronously.

- DynamoDB - managed NoSQL database

  * Strongly consistent read  - 1 read per second
  * Eventually consistent read  - 2 reads per second
  * Writes - 1 read per second

- Enabling DynamoDB Accelerator (DAX) is a better choice when working with DynamoDB

- `Amazon DynamoDB Accelerator (DAX)`: is a fully managed, highly available,
in-memory cache for DynamoDB that delivers up to a 10x performance improvement
– from milliseconds to microseconds – even at millions of requests per second.
[Click here for more on DAX](/2020/04/04/Amazon-DynamoDB-Accelerator-DAX)

- Redshift - managed big data (for e.g data warehouse)

__Caching__

- Cache dynamic and static content at the edge

- Application layer caching

- Redis - better for NoSQL databases
  * Vertical scaling
  * Atomic counters
  * Supports read replicas - If you loose a node, you don't loose all your
  cached data unlike Memcached
  * Leaderboards/Counters

- Elasticache (Memcached) - better for relational db
  * Horizontal scaling
  * Low maintenace
  * Storing session state

__A word on Billing for S3__

You pay for what you use in GB per month
You pay for transferring data out of the region where the data resides
You pay for `PUT`, `copy`, `POST`, `list` and `GET` request. `GET` the
most expensive.

__CloudFront__

- Use SSL and Origin Access Identity (OAI) to protect CloudFront content.

__Design Solutions for Elasticity and Scalability__

Horizontal scaling - Auto scaling

Use Elastic Load Balancer (ELB)

3 components of auto scaling

- `Launch configuration` can only be copied or replaced but not modified.

- `Auto Scaling Groups`. these reference the launch config, specify the min
max and desired size of the auto scaling group. You can reference the elastic
load balancer from the auto scaling group. Health checks can be set up
for auto scaling groups.

- `Auto Scaling Policy`. Specify how much we should scale in and out. Multiple
policies can be attached to an auto scaling group.

CloudWatch Metrics

- Hypervisor level. E.g CPU utilization and network bandwidth

- Those that require a CloudWatch agent to be installed on the EC2 instance. E.g disk
space utilization.

- Default CloudWatch monitoring at 5 minute intervals.

- Detailed CloudWatch monitoring at 1 minute interval.

### Specify Secure Applications and Infrastructure ###

- Shared Responsibility Model.

Understand the difference between security in the cloud and security of the cloud.

- Principle of least privilege.

- Identity and Access Management (IAM)

- Prefer assigning roles to resources over user account with access keys.

- You need to know how to create a VPC from scratch, rather than using the wizard.

- Difference between security groups and NACLs

- Know difference between public and private subnet and when to use them.
For a subnet to be public, it must contain a route table route to the
internet gateway.

- VPC Peering, NAT Gateways, Internet Gateways, Virtual Private Gateways,
Direct Connect etc

__Securing Data__

- In transit - SSL certificates for web traffic. IPSec for VPN traffic or
Direct Connect (put an IPSec tunnel inside Direct Connect)

- At rest
  * Four ways that we can encrypt data in amazon S3
    - Amazon S3 managed keys - SSE - S3  - Uses AES 256
    - SSE - KMS - Key management service
    - SSE - C - Customer Managed Keys
    - Client side encryption - Upload data you already encrypted.
  * Amazon EBS support encryption of data with KMS
  * KMS can integrate with many other AWS services.
  * KMS FIPS 140-2 level 2 compliance
  * For FIPS Level 3 you use an AWS CloudHSM - a hardware device.
  AWS takes care of the backups and replication between nodes if in a cluster.
  You should have a cluster because if you loose these keys -- too bad!

### Design Cost Optimized Architectures ###

__Billing Tips__

- Pay as you go
- Pay less when you reserve Resources
- Pay even less when you use much resources

- You typically pay for: capacity, storage and data transfer
- You can use DynamoDB to hold metadata for S3 objects.

__Optimizing S3 costs__

- Storage Class
- Storage consumption
- Requests
- Data transfer

__Optimizing EBS volume costs__

- Volume Type
- IOPS
- Snapshots
- Data Transfer

__Serverless__

Serverless is more cost effective in most cases. Look for every opportunity
to incorporate serverless options.

- AWS Lamda
- S3
- API Gateway
- Fargate

__Guidance for Compute Cost Optimization__

- Shutdown a server when you don't need.
- Replace with serverless if possible.
- Instance configuration e.g t3 is less expensive than C5 of similar size.
- Type of instance you purchase. E.g a reserved instance saves money than an
on demand instance. Spot instance can be very cost effective.
- Number of instances
- CloudWatch monitoring (detailed) as well custom Cloud metrics costs money
- Auto Scaling.
- OS and Software. e.g Licences software like Oracle.
- Tenancy e.g shared tenancy vs dedicated servers
Some compliance requirements require dedicated servers.

### Define Operationally Excellent Architecture  ###

- Peform operations with code e.g via CloudFormation
- Evolve
  * Continously
  * Learn from failures

- AWS CloudTrail - Tracks all API calls
- AWS Config - Monitor resources and changes to those resources
- AWS CloudFormation - Create and Deploy
- AWS Inspector - Automated security assessment tool (has an agent) scans
- EC2 instances for
- AWS Trusted Advisor
- VPC Flow logs - Allows you to capture information about ip traffic.
- VPC flow logs do not capture packet level info. It only captures meta data
e.g source ip, source port, dest ip and port, tcp protocol, successful or failed.

### Takeaways ###

- Design for infrastructure failure scenarios

- Understand the difference between fault tolerance and high - availability.

- Managed services are preferred

- For High - availability a single AZ is always incorrect.

- S3 is a good storage storage for unstructured data.

- Look for caching options to improve performance.
