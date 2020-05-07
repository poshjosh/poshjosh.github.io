---
path: "./2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-8_Application-deployment.md"
date: "2020-03-09T16:39:00"
title: "AWS Certified Solutions Architect Associate - Part - 8 Application deployment"
description: "Easy to understand tutorial: AWS Certified Solutions Architect Associate - Part 8 - Application deployment"
lang: "en-us"
---

### Designing Cost-Optimized Compute ###

#### Application and Deployment Services ####

Application and deployment services allow you to implement application logic
in the cloud.

Service Categories
- Management console -> Services are listed
- Service categories can grow and change over time

#### Lamda ####

Lamda is an AWS compute service that allows code to run without placing it on
a launched EC2 instance.

Management Console -> Compute -> Lambda

- AWS Compute Service that runs code without (you specifying and managing the)
servers
- Runs code only when needed
- Scales automatically (up to thousands of requests per second)
- Billed by compute time

Automated Management
- Server maintenance
- Operating System maintenance
- Capacity scaling
- Code monitoring
- Logging

Languages Supported
- Node.js
- Java
- C#
- Go
- Python

Lamda Use Process

- Customer builds the code
- Customer launches the code as lamda function
- AWS selects the server
- Customer calls lamda function as needed from applications

#### API Gateway ####

Magagement Console -> Neworking & Content Delivery -> API Gateway

- APIs managementin the cloud:

  * Create
  * Publish
  * Maintain
  * Monitor
  * Secure

- APIs can interact with many targets
  * AWS services
  * Other web services
  * Data stored in AWS

- One API may use several services or other APIs etc

API Gateway uses a serverless architecture:

- Moves data in and out of the cloud without isntances
- Process functions without instances
- Two primary services
  * Lamda functions
  * API Gateway
- Example path way
Client device -> API Gateway -> Lamda Function -> AWS/Web Service etc (E.g database)
- Cross Origin Resource Sharing (CORS)
  * Can be enabled for API gateway
  * Allows receipt of request from other domains
  * Default is internal domain request only

#### Kinesis ####

Use to process streaming data.

Management Console -> Analytics

- Processing streaming data
- Real-time analytics
- Multi-tier enabler. Allows you to seperate your analytics from your database
- Very DevOps focused
- Conceptual importance for an architect.

Operating Modes

- Kinesis Data Streams
- Kinesis Data Firehose
- Kinesis Data Analytics
- Kinesis Video Streams

With the exception of Kinesis Data Analytics, the rest concern input data

Benefits

- Architecture fully managed
- No custom coding required
  * Configure producers
  * Configure consumers
  * Focus on analytics

#### Kinesis Data Streams and Firehose ####

- Kinesis Data Streams

Input -> Kinesis Data Streams -> Processing tools (e.g Kinesis Data Analytics,
Lamda functions, EC2 instances) -> Output to business intelligence tools

- Kinesis Data Firehose (Data streamed directly to data stores)

Input -> Kinesis Data Firehose -> Data stores -> Analytic tools

Data stores include: S3 buckets, Redshit, Elasticsearch and splunk

- Kinesis Video Streams (Recently introduced)

CCTV, Video Input etc -> Kinesis Data Firehose -> Output

With this you could use Amazon Rekognition to process the video data.

Kinesis Data Firehose streams directly to data stores unlike Kinesis Data streams
which streams to processing tools/services

#### Kinesis Data Analytics ####

Kinesis Data Analytics is for Analyzing real-time data in real-teim

- Analyze real-time data streams
- Based on standard SQL queries
- Supports many concurrent consumers
  * Redshift
  * S3
  * Elasticsearch
  * Lamda
  * Kinesis Data Streams

Input from Kinesis Data Firehose/Kinesis Data Streams -> Kinesis Data Analytics
-> Output

#### Reference Architectures ####

A reference architecture is a well - architected framework for a specific solution.
It is a template for implementing an entire network that complies with a set of
requirements. Design recommendations in AWS inorder to comply with standards etc

Example
- One AWS account
- One Production VPC
- 2 Availability Zones (e.g eu-west-2a and eu-west-2b)
- In each AZ
  * One private subnet
  * DMZs with proxies going out to the internet
  * Another private subet with relational databases
- S3 buckets
- S3 Policies
- AWS config
- CloudWatch alarms

For reference architectures browse quick starts by categories

- Browse to: aws.amazon.com/quickstarts
- Download the template for your own architecture case
- Management Console - Management Tools - CloudFormation
- Design a template
- Select file -> Open - Local file
- Select the template you got from the quick starts section of AWS

### Designing for Operational Excellence ###

#### CloudFront ####

All about improving performance for the end user experience using regional edge
caches.

Management Console -> Networking & Content Delivery -> CloudFront

Content Delivery Network

- Distributes content to localized regions
- Reduces latency
- Provides high data transfer speeds

Use Cases

- Accelerate static website content delivery
- Serve on demand or live streaming video
- Encrypt specific fields throughout system processing
- Customize at the edge
- Serve private content by using lambda@edge customizations

Primary Considerations

- Content source

  * S3 bucket
  * MediaPackage channel - helps get media in right format for web, mobile as needed
  * HTTP server

- Content access

  * Public
  * Restricted

- Content constraints

  * HTTPS required
  * Geo-restrictions

AWS Cloud -> Edge cache -> Edge server -> end user

#### Web Application Firewall (WAF) ####

- Controls access to HTTP and HTTPS servers

  * Based on requests
  * Based on source IPs

- Works with CloudFront and/Elastic Load Balancer

WAF Behaviours

- Allow all requests except the ones specified (Open to closed)
- Block all request except the ones specified (Closed to open)
- Monitoring
  * Requests that match specified parameters
  * Integratable with Simple Notification Service

WAF Operations

- Error handling
  * HTTP 403 error (forbidden)

- Configurable default behaviour
  * What happens when the request doesn't match andy rules?
    . Allow
    . Deny  

WAF especially useful for public facing websites.

#### Simple Queue Service (SQS) ####

Management Console -> Application Integration -> SQS

- Used to decouple applications
  * Break app into seperate processing tasks
  * Allows many smaller processes to form a complete solution

SQS Messages

- Outputs from other processes
- Inputs to other processes
- Queued and processed asynchronously
  * Non - linear
- Up to 256kb of data

SQS Participants

- Message producers
- Message consumers
- Messaging Service (i.e SQS in this case)

SQL Features

- Redundant accross multiple AZs
  * Queued until process
  * Rentention up to 14 days

- Automatically scales

SQL Queue Types

- Standard
  * Default
  * Does not guarantee sequential delivery of messages
- First-in-First-Out (FIFO)
  * Guarantees sequentlial delivery of messages
  * Supports fewer transactions per second.

Standard (performance + async) vs FIFO (less performance + sequential)

#### Simple Notification Service (SNS) ####

Management Console -> Application Integration -> SNS

- Notifications in the cloud

- Uses the publish-subscribe mechanism based on topics
  * Called pub-sub messaging

- Publishers push messages to topics
  * Topic examples - admin alerts, performance alerts etc
  * Publisher examples - cost watch etc

- Subscribers

  * Clients receiving notifications
  * Receive all messages broadcasted to the topic

- Publishers and Subscribers aren't aware of each other

SNS Features

- Stored across multiple AZs
- Several delivery options supported e.g HTTP/HTTPS
- E-mail
- Short Message Service (SMS)
- Lamda function target
- SQS target

SNS Message Limits

- Up to 256KB of data
- Special SMS constraints
  * 140 bytes is max size of single SMS
  * Larger messages are sent as multiple transmissions
  * Aggregate SMS size is 1600 bytes

#### Simple Workflow (SWF) ####

Workflow process management tool. Useful when you want to make sure decoupled
applications workflow are performed in coordination to achieve desired objective.

SWF

- Defines the sequence of events required to achieve a workflow
- Used in decoupled applications. We want to understand what each component does
in relation to each other
- Activities that result in a desired objective
- Logic that controls the activies
  * Logic begins with a decider function which determines best workflow
- Operates in a domain which is a create logical boundary within SWF to constrain
the scope of the activies

SWF Activity Tasks

- One invocation of an activity
  * For example processing an order
- May be invoked multiple times

SWS Activity Workers

- The applications that receives and process activity tasks.

#### Step Functions ####

Management Console -> Application Integration -> Step Functions

- AWS recommended practice. Eventually replacing SWF
- Simplar to SWF in functionality
  * Uses state machines comprising: Decider, Activity Tasks, Worker tasks
- Task is a single unit of work
- Choice provides brancing logic
- Support parallel processing

### Designing for Elasticity and Scalability ###

#### OpsWorks ####

Management Console -> Management Tools

- Configuration management service
  * Configure (code based)
    . Instance deployment
    . Service deployment
    . Application deployment
  * Operate
    . Application update
    . Infrastructure update

- Automated deployment

OpsWorks Stacks

- Inital OpsWork mode
- Stacks contain layers
- Collection of layers
  * Any AWS Service
  * Any runtime environment
  * E.g of layers linux, MySQL, Node.js
  * When you combine various layers, you get a stack

OpsWorks Chef Automate

- Cookbooks contain recipes
- Recipes equivalent to layers
  * Defined configuration settings
    . Admin defined
    . AWS defined
    . Third-party defined

OpsWorks Puppet

- Master servers
  * Pre-configured modules
  * Modules are configured to layers

Prebuilt Layers

- Ruby
- PHP
- Node.js
- Java
- Amazon RDS
- HA proxy
- MySQL

Use Cases

- In the cloud
  * Chef
  * Puppet

- On-premises (local)
  * Stacks

#### Cognito ####

Management Console -> Security & Identity -> Congnito

- User identity and data sync service a.k.a SSO
- Public identity providers
  * Google
  * Facebook
  * Amazon
- Private identity providers
  * Active directory with SAML

Identity Management

- Based on open standards
  * OAuth 2.0
  * SAML 2.0
  * OpenID connect

- Profile management (Manage users with out them having to have IAM accounts)
- Scales to millions of users

AWS Integration

- Congnito controls access to AWS resources
  * Define roles
  * Map users to roles

#### Elastic MapReduce (EMR) ####

Reduce by mapping to another. E.g reduce the workload of this process by mapping
to another process. E.g reduce the scope of data by mapping the data to its ID value.

- Distributes processing accross clusters
  * Implements a managed Hadoop framework
- Pulls data from S3 buckets
- Uses EC2 instances for compute power
- User defines the number of needed cluster

EMR Cluster Nodes

- Master Node - Coordinates job distribution across core and task nodes

- Core Node
  * Run tasks assigned by master node
  * Store data in the cluster

- Task Node
  * Runs only tasks that do not store data

#### CloudFormation ####

Management Console -> Management Tools -> CloudFormation

Uses templates to automate building entire solutions like:
- EC2 instances
- Security groups
- Subnets
- IAM users and roles

Notes

- Templates are used to build stacks.
- Stacks are implementations of templates.
- Changesets are used to modify stacks.
- Templates may deploy many different things.
- A template in deployment is called a stack.

Why Use It?

- Rapid deployment
- Mirror existing internal achritecture
- Take advantage of templates created by others
- Easily rebuild you entire AWS architecture

Examples of templates:

Template to:

- Launch EC2 instance running webserver with wordpress, having some specific plugin
- Launch wordpress in ELB cluster and stores all images in S3 bucket with CloudFront

Usage

- Management Console -> Management Tools -> CloudFormation
- Click on Design Template
- Note you could choose File -> Open, to open an existing template

#### CloudWatch ####

AWS Monitoring solution. E.g health, security, cost, performance monitory

Management Console -> Management Tools -> CloudWatch

- Monitors cloud and on-premises systems
- Dashboards. You could build a billing/performance dashborad
- Based on logs
- Events
- Alarms. An alarm could send an SNS notification or launch an EC2 instance.

Why Use?

- Monitor critical systems
- REceive notificatios related to performance and security etc
- Push on-premises logs into the cloud
- Take automatic actions based on alarms

#### TrustedAdvisor ####

Advise about your AWS implementation.
- Cost Optimization
- Preformance
- Security (Free tier)
- Fault Tolerance
- Service Limits

Management Console -> Management Tools -> TrustedAdvisor

#### Organizations ####

Management Console -> Services -> Security, Identity & Compliance -> AWS Organization

- A collection of AWS accounts
- Centralized
  * Management interface
  * Billing
  * Account management
- No additonal charge for use

Organizational Units (OUs)

Simply a container you can place things in. You could create one unit called
accounting and another called IT

- Hierarchical account managemnt
- Nested OUs up to five level deep
- Policies attached at the OU level for permissions

### Notes ###

- What AWS component allows you to interact with Lambda functions and other
custom code running in the cloud? API Gateway
- For API Gateways, Cross Origin Resource Sharing (CORS) need to be enabled for
request originating outside current domain.
- Kinesis Data Firehose streams directly to data stores unlike Kinesis Data streams
which streams to processing tools/services
- WAF can limit access based on request types and source IP addresses
- Standard (performance + async) vs FIFO (less performance + sequential)
- SQS messages could be up to 256KB
- SNS Message Limit - 256KB
- Special SMS constraints
  * 140 bytes is max size of single SMS
  * Larger messages are sent as multiple transmissions
  * Aggregate SMS size is 1600 bytes
- SWF allows for the definition of sequenced events and is used in decoupled applications
- A workflow includes the activities and logic required to complete a process
- Ops work can be used for on-premises configuration management
- EMR core nodes process tasks with IO read/write (store data)
- EMR task nodes process tasks data does not store data
- CloudFormation can deploy entire solution sets automatically including EC2 instances
with security groups, subnets, IAM users, roles and more
- Y/N CloudWatch can monitor on-premises systems -> Y
- TrustedAdvisor security improvements is included in Free Tier
- Using AWS Organizations incurs no additonal charge
- AWS Organizations can manage user, groups and roles together
- Organizational Units could be nested up to five (5) level deep

### Acronyms ###

- WAF - Web Application Firewall
- SQS - Simple Queue Service
- SNS - Simple Notification Service
- SWF - Simple Workflow
- EMR - Elastic MapReduce
- CORS - Cross Origin Resource Sharing
