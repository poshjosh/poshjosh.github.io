---
path: "./2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-10_Services_and_design_scenarios.md"
date: "2020-03-09T21:29:00"
title: "AWS Certified Solutions Architect Associate - Part 10 - Services and design scenarios"
description: "Easy to understand tutorial: AWS Certified Solutions Architect Associate - Part 10 - Services and design scenarios"
lang: "en-us"
---

### Amazon Services ###

#### Media Content Delivery ####

Elastic Transcoder is used to provide videos to mutliple locations (windows,linux,ios,android etc)
and formats (mp4,m4a,3gpp etc), for example from one format.

- Media transcoding (i.e converting media from one format to another)
  * Jobs do the transcoding
  * Pipelines - Queues which to the jobs
  * Presets - Settings to convert media (e.g from which format)
  * Notifications - SNS used to notify you of job status
- The transcoded files gotten from S3 bucket and placed in the original bucket
with a different file name after transcoding

Translate is used to translate content into different languages. Be mindful, for
each language has source to destination pairs that are actually allowed.

AWS Management Console -> Machine Learning -> Translate

- Can integrate into apps for localization
- On demand language translation
- Encoder reads the source text
- Decoder outputs the translated text

Elemental Media Store

AWS Management Console -> Media Services -> Media Store

Elemental MediaStore.

- Video origination and storage services.
  * Containers, Folders, Endpoints, Objects, Policies

- Video Considerations
  * Live video streams. Elemental MediaStore, origination endpoint
  * Storage based video. S3 buckets.

Transcribe

AWS Management Console > Machine Learning -> Transcribe

- Speech to text for audio and video (Generate close captioned files for video)
- Integrates with Translate - Transcribe the speech to text in the S3 bucket
then translate from there.

Rekognition

AWS Management Console > Machine Learning -> Rekognition.

- Image and video analysis to find people, speech, objects etc in videos
- Can be run against S3 buckets
  * Enhanced search based on analysis results

#### Desktop and App Streaming ####

Workspaces = AMIs that already exist that may be used in AWS as virtual desktop.

AWS Management Console -> Desktop and App Streaming -> Workspaces

- Virtual Desktop in AWS
  * Linux
  * Windows 7 (plain or with MsOffice 2010/2016)
  * Windows 10
- Persistent storage in Virtual D: drive. All users D: drive are backed up
automatically and regularly.
- Based on windows servers virtualization

AppStream 2.0 gives us virtual applications in AWS i.e it looks like the app is
running on your local machine.

AWS Management Console -> Desktop and App Streaming -> AppStream 2.0

Commonly used for developed apps which you want to test on many different
computers.

#### ElasticCache ####

- In-memory caching for databases
  * Memcached - Simplest model for implementation in AWS (more performant than Redis)
- Redis. HIPAA or PCI-DSS compliant

AWS Management Console -> Databases -> ElasticCache
- Click get started now to build a cluster of caching servers
- Picking the right node (instance) type ensures you have the right amount of
memory you need.

#### Lab: Security Services ####

Key Management Services - Used to manage (rotate, delete etc) encryption keys.

AWS Management Console -> Security Identity & Compliance -> IAM
Click on Encryption keys to view your keys
- Click on create key to create an new key
- Advanced Options
  * Click on KMS for Key Material Origin (KMS/External)
- Key Administration - Who can administer this key
- Key Usage Permission - Who can use this key
- Preview Key Policy

Cloud Hardware Securit Module (HSM)

AWS Management Console -> Security Identity & Compliance -> Cloud HSM
- Create a cluster on which the HSM will run
- Thereafter create a HSM
- Applications call the HSM to offload encryption processing to better optimize
the app. The HSM could also be called from an on-premises server

Directory Services are tools that allow you to have a directory of all the resources
on your network; Resource like user, groups, roles, organizational units, devices
(including computers, printers etc) policies amongst others

AWS Management Console -> Security Identity & Compliance -> Directory Services

- Directory types
  * AWS Managed Microsoft Active Directory (AD)
  * Simple AD
  * AD Connector which could be used to an existing directory

Take advantage of existing security enabled AMIs
AWS Management Console -> EC2 -> Launch and instance
- Choose AMI
- Select AMI interface
- Select the security category to view available products

#### Analytics Engines ####

AWS Management Console -> Analytics -> CloudSearch

CloudSearch is useful when you have a lot of offline data that you want to
make searchable. This is achieved by bringing it into a central repository.

AWS Management Console -> Analytics -> ElasticSearch
Create a new domain
Generally we create an elasticsearch cluster

AWS Management Console -> Analytics -> AWS Data Pipeline
Ochestration for data driven workflows

AWS Management Console -> Analytics -> AWS Glue
Extract transform and Load (ETL) tool.

AWS Management Console -> Analytics -> QuckSight
QuickSight is advanced business analytics
QuickSight does not come with standard AWS subscription. You need to sign up for it.

AWS Management Console -> Analytics -> Athena
Athena is a query service that makes it easy to analyze data in S3 buckets

#### Development and Operations (DevOps) ####

AWS Management Console -> Developer Tools

CodeCommit, CodeBuild, CodeDeploy and CodePipeline - If you click on any one
of them, you get all of them.

CodeDeploy can deploy code across required instances automatically as it is updated

Not all languages are supported.

CodeStar also allows you to develop, build and deploy applications in the cloud

AWS Cloud 9 is a cloud base IDE
- AWS Cloud 9 is not free
- After 30 minutes of in-activity the server will be suspended to keep costs down

AWS X-Ray is used for analyzing and debugging applications

### Operational Excellence with AWS ###

Well-Architected Framework
- Operational Excellence
- Security
- Reliability
- Performance
- Cost Optimization

#### The Operational Excellence Process ####
- Prepare
  * Understand workloads and expected behaviours,
  * Considerations
    . Operational priorities
    . Design for operations
    . Operational readiness
- Operate
  * Monitor
    . Environmental health
    . Discover business and technical insights
  * Respond to security, reliability, performance and cost
- Evolve
  * Learn from experience
  * Share learning
  * Improve
  * Scale - out or in as required

The most prioritized service could have multiple redundancies etc

#### Widget Makers Scenario ####

What are your priorities? Eg:
Order processing - Web Server, SQL server 50 - 70 users
Inventory Management - MySQL
Payroll - TimeClock connection, SQL server, Managers - r/w, Accounting - r
User data - Windows server share 700MB per user 150GB total
Web site - 3500 daily visits during the week, 600 per day weekends
Wordpress with custom plugins.

#### Resilient Design ####

- Provides reliability
- Automation
  * Recovery, Scaling and backups (not as resilient if manual)
- aws-reliability-pillar.pdf
  * Test recovery procedure - E.g test your CloudFormation launch template
  * Auto recover from failure
  * Scale horizonatally to increase aggregate system availability. Break up a
  monolith system.
  * Stop guessing capacity
  * Manage change via automation (Automate scaling out, in of servers etc)

#### Resilient Design Scenario ####

Widget Makers

Order Processing -> RDS with SQL Server (same db used earlier)
Reliability with mutli AZ DB deployment

Inventory Mgt - RDS with MySQL (same db used earlier)
No clustering due to fewer users
Reliability with mutli AZ DB deployment

Payroll System -> RDS with SQL Server (same db used earlier)
Reliability with mutli AZ DB deployment
Implement a read replica - There is usually a short period where the database
receives much transaction

User data stored on shared folders on local system... We move these to S3 buckets
Use third party tools to map drive letter on local system to S3 buckets.

Web site via WordPress - Elastic load balanced deployment with 2 EC2 instances
running WordPress

#### Performant Design ####

- Democratize advanced technologies ... don't try to re-invent the wheel
Rather than putting text data in an s3 bucket and writting and reading much
data from it, use a DynamoDB table
- Go global in minutes ... Deploy to multiple regions (closer to users) in minutes
- Use serverless architecture ... Serverless architectures scale better than server
based archtitecture
- Experiment often, game the system, test often, try different configurations
- Have mechanical sysmpathy - Use tech that aligns best to objectives. For example
consider data access patterns when selecting DB.

Auto Scaling
- Key to performant design in the cloud
- EC2 instances can be scaled auto
  * Logging of scale actions should be in place
- Make sure DB services can be scaled quickly by monitoring them
- Make sure to use the right storage for your performance needs

| Storage |	 Services              |  Latency          |  Throughput         Shareable
|---------|------------------------|-------------------|-------------------|-----------
| Block	  | EBS, EC2 instance store| Lowest consistency| Single instance   | Mounted on single instance, copies via snapshot
| File    | EFS  			             | Low consistency   | Multiple instances| Many clients
| Object  | S3			               | Low latency	     | Web-scale	       | Many clients
| Archival| Glacier		           	 | Min to Hrs        | High              | No

#### Performant Design Scenario ####

- Order Processing -> Instance type optimized for memory and processing

- Inventory Mgt -> Instance type optimized for memory and processing
Automate inventory mgt using SNS service
Automate adding inventory with AWS CloudWatch monitoring and trigger restock action

- Payroll	->  Instance type optimized for memory and processing
Perform payroll processing only from the read replica

- User Data -> Implement departmental S3 buckets for improved performance and managemnt
Configure alarms to notify admin of users exceeding 700MB of storage

- Web site -> Instance type optimized for memory and processing
Use ELB volumes (using the write drive type)

Good internet connection neccessary for your users to experience the best of
what you have to offer

#### Secure Design ####

Design principles

- Implement a strong identity foundation. E.g abide by least privilege principle
- Enable traceability ... via monitoring with CloudTrail
- Apply security at all layers e.g edge network, vpc, subnet, load balancer,
instance level, OS level and application level.
- Automate security best practices. Create secure architecture by implementing
controls that are defined and managed as code in version control
- Protect data in transit and at rest. Classify data into sensitivity levels and
use mechanisms such as encryption, tokenization and access control where appropriate
- Keep people away from data. Create mechanisms and tools to reduce or eliminate
access to data
- Prepare for security events. Prepare for an incident by having incident management
process

Security in the Cloud
- IAM
- Detective controls .. monitoring
- Infrastructure protection .. least privilege access
- Data protection (encryption, backup, recovery)
- Incident response

Share Responsibility Model

Edge network, vpc, subnet, load balancer, instance level, OS level and application level

AWS -> Security of the Cloud
You -> Security in the Cloud (generally from the OS level up)

#### Secure Design Scenario ####

Widget Makers Scenario

- Order Processing -> SQLServer
IAM groups and policies
Only approved people can access RDS instances
Update DB permissons to secure individual access
Secure the client application locally

- Inventory Mgt
IAM groups and policies
Only approved people can access RDS instances
Update D permissons to secure individual access

- Payroll
IAM groups and policies
Only approved people can access RDS instances
Update DB permissons to secure individual access
Read replica - Only accounting employees can access this replica

- User Data
IAM groups and policies
Only approved people can access buckets
Enable at rest encryption
Enable SSL for data transfer accross the internet

- Web site
Run web server instances with appropriate roles
Configure security groups for network interfaces
Configure proper security groups for the VPC

#### Cost Optimization ####

Design principles

- Adopt a consumption model. Use what you need. E.g some instances be brought down
during less transactions. Also e.g employee servers could be brougt down outside
working hours
- Measure overall efficiency. Measure the business output and the cost associated
with it.
- Stop spending money on data-center operations
- Anayze and attribute expenditure. Evaluate cost - value/benefit -ratio to know
what areas/departments/services bring more money.
- Use managed services to reduce cost of ownership.

Four Pillars of Cost Optimization
- Use cost-effective resources. Some times it is less expensive to run a more
powerful instance which does more stuff per hour
- Match supply with demand (auto scaling config)
- Be aware of your expenditures
- Optimizing over time

#### Cost Optimization Scenario ####

- Order Processing -> SQLServer
Use a managed database

- Inventory Mgt
Use a managed database

- Payroll
Use a managed database
Use the read replica as needed (payroll on certain days, times and frequencies)
Remember to give time for data to load

- User Data
Monitor use

- Web site
Use the right instance class
Monitor access
Address improper access

#### AWS general best practices ####

Design for failure
- Use clustering to have failover instances
- Availability Zones
- Backups
- Alternate AWS accounts. Another AWS account with everything you are using in
the main account. The instances are not up and running etc
- CloudFormation templates

Implement Elasticity
- Auto Scaling
- Elastic Load Balancing
- Decoupled Applications
- Run tasks in parallel

Learn
- Use the AWS free tier account
- Practice
  * Build entire solutions
  * Configure every option
  * Tear it down
  * Start again
- Try different solutions

### To Read ###

AWS White Documents

- AWS Reliability Pillar
- AWS Performance Efficiency Pillar
- AWS Security Pillar
- AWS Cost Optimization Pillar

### Notes ###

- Amazon translate - Each language has source to destination pairs that are
actually allowed.
- Use Elemental MediaStore for live video streaming and S3 buckets for storing
videos normally
- Use AppStreams for developed apps which you want to test on many different
computers.
- Redis is HIPAA and PCI-DSS compliant
- PKIs are used to centrally manage keys for asymmetric cryptography
- Developer Tools - CodeCommit, CodeBuild, CodeDeploy and CodePipeline - If you
click on any one of them, you get all of them.
- Developer Tools - CodeStar also allows you to develop, build and deploy applications
in the cloud.
- AWS Cloud 9 is a cloud base IDE which is not free
- After 30 minutes of in-activity Cloud 9 will be suspended the serverto keep costs down
- The process of a well-architected framework includes:
  * Preparation, operation and evolution
- Resilient/Reliable Design = automated recovery, scaling and backups (not as
resilient if manual)
- Implementing IAM properly is the foundation of AWS security
- Using managed services like RDS is usually less expensive than managing EC2
instances yourself

### Acronyms ###

- HSM - Hardware Securit Module
- AD - Active Directory
- PKI - Publick Key Infrastructure
- ETL - Extract transform and Load
