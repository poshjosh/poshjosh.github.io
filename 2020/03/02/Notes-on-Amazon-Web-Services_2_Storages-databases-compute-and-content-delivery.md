---
path: "./2020/03/02/Notes-on-Amazon-Web-Services_2_Storages-databases-compute-and-content-delivery.md"
date: "2020-03-02T09:45:00"
title: "Notes on Amazon Web Services 2 - Storages databases compute and content delivery"
description: "Easy to understand tutorial: Notes on Amazon Web Services 2 - Storages databases compute and content delivery"
lang: "en-us"
---

### AWS Storages ###

- S3 - Store and access any type of data over the internet. Create a bucket and
upload objects to the bucket. The bucket grows as objects are added.. theoritically
unlimited size.

- Glacier - Cheapest storage, designed for archiving of data (so not readily
  accessible). One could set up lifecycle rule to auto migrate old data from S3
  to glacier for archiving.

- Elastic Block Store (EBS) - Highly available, low latency storage for attaching
to servers launched via EC2 service. Like attaching a hard drive to computer system.

- Elastic File System (EFS) - Network attached storage for EC2 service. Allows
multiple servers access same data source.

- Storage Gateway - Enables hybrid storage between on premises local and AWS
cloud. More frequently accessed data stored on premises while less frequently
accessed data is stored on the AWS cloud.

- Snowball - Portable peta byte scale data storage. Download your data to a Snow
ball device and send to AWS which will upload the data to you S3 bucket.

### AWS Clould Sample Usage ###

Create own VPC in AWS. Our VPC is a secure fortress.
Launch servers in the VPC
Attach EBS device, one for each server. Each server then has access to own EBS.
For both servers to share storage (i.e network), we use EFS with mount target to each server.

Automated solution which automaticaly migrates old data for archiving.

- Use S3 with lifecycle rule to auto migrate old data from S3 to glacier for archiving.
- S3 bucket is located in the AWS cloud not our VPC. So create a VPC endpoint to allow traffic flow in and out of our VPC.
- AWS storage gateway ensures data between local data centre and AWS cloud is synced. It will store copies of frequently accessed data in on site storage, whereas all data in amazon S3 buket.

### AWS Database Services ###
- Relational Database Service (RDS) - For relational database like MySQL, Oracle, PostgreSQL, Amazon Aurora (Amazons MySQL compatible database)
- Amazon Dynamo Db - serverless NoSQL database service.
- Amazon Redshit - Fast, fully managed petabyte database good for big data storage.
- ElasticCache - In memory datastore or cache, in the cloud. Faster than disk based database services like the one listed above.
- Database Migration Service (DMS) - for migrating from one amazon or other db to another.
- Amazon Neptune - Fast fully managed graph database service.

### Create VPC in AWS ###
- Migrate with DMS from local database to own VPC.
- Use ElasticCache for frequently accessed data. Check cache first then actual db.
- Goto: Management Console -> Services -> Database Services -> RDS and follow steps to setup a MySQL database.
- Under Backups: Set backup retention period to zero (0) to prevent backups which cost money.

### AWS Compute Services ###
- EC2 - Provide virtual servers in the AWS cloud.
- EC2 Autoscaling - Allows dynamic scaling of EC2 capacity based on conditions specified.
- Lightsail - Easiest way to launch virtual servers running applications.. provides dns management etc
- Elastic Container Service (ECS) - For docker images
- AWS Lambda -

### AWS Cloud ###
- Own VPC instance
- EC2 instances
- Elastic Load balancing receives traffic from end users and distribute the traffic to an available EC2 instance. It will balance the load accross multiple EC2 instances.
If an EC2 instance is unhealthy.. the EC2 instance will file a report with the Elastic load balance which will no-longer send traffic its way
- Autoscaling Service handles creating, terminating and performing health checks for EC2 instances.

### Networking & Content Delivery)

- CloudFront - CDN securely delivers content to various endpoints providing - protection against DDoS Attacks
- Virtual Private Cloud (VPC)
- AWS Direct Connect - High speed direct connection to AWS cloud.
- Elastic Load Balancing (ELB)
- Route 53 - Domain Name System (DNS)
- API Gateway - Fully managed serverless service which helps developers develop and deploy APIs.

### Example Usage CDN with Multiple Availability Zones ###
- ELS can distribute traffic across multiple availability zones
- Use CDN  for content which does not change often which needs to be delivery with high speed and low latency e.g images, videos.
- Use CloudFront distribution to handle the above.
- For other requests, CloudFront forwards to ELB which forwards to EC2 instance.
- The CloudFront distribution will have its own DNS name by which we can browse to the content. DNS name will be quite user unfriendly (long unweidly URI).
- Route 53 will is a DNS which provide user friendly names and translates requests to that name to the actual unfriendly URI which will be used to connect to CloudFront.

### Quick Deployment of Wordpress ###
- Management Console -> Services -> Compute Services -> EC2
- Click on Instances -> Launch and Instance
- Choose Amazon Machine Image (AMI)
- Go to AWS Market place to select Wordpress AMI
- Enable public IP
- To connect to the linux OS of the instance use a key. So create a key pair.
- Follow instructions to Review and Launch the instance
- Wordpress password is in the logs.
- To find your password: Management console -> Actions - Settings - Logs

### Acronyms ###

- VPC - Virtual Private Cloud
- S3 - Simple Storage Service
- EBS - Elastic Block Store
- RDS - Relational Database Service
- ELB - Elastic Beanstalk
- ECS - Elastic Container Service
- DNS - Domain Name Service

### Notes ###

S3 Bucket names must be unique across the whole AWS buckets.

### Links ###

- [Notes on AWS - Part 1 - Introduction](/2020/03/02/Notes-on-Amazon-Web-Services_1_Introduction/)

- [Notes on AWS - Part 2 - Storages, Databases, Compute and Content Delivery](/2020/03/02/Notes-on-Amazon-Web-Services_2_Storages-databases-compute-and-content-delivery/)

- [Notes on AWS - Part 3 - Management Tools, App Integration and Customer Engagement](/2020/03/02/Notes-on-Amazon-Web-Services_3_Managment-tools-app-integration-and-customer-engagement/)

- [Notes on AWS - Part 4 - Analytics and Machine Learning](/2020/03/02/Notes-on-Amazon-Web-Services_4_Analytics-and-machine-learning/)

- [Notes on AWS - Part 5 - Security, Identity and Compliance](/2020/03/02/Notes-on-Amazon-Web-Services_5_Security-identity-and-compliance/)

- [Notes on AWS - Part 6 - Developer, Media, Mobile, Migration, Productivity, IoT and Gaming](/2020/03/02/Notes-on-Amazon-Web-Services_6_Developer-media-migration-productivity-iot-and-gaming/)

- [Notes on AWS - Part 7 - Elastic Beanstalk](/2020/03/02/Notes-on-Amazon-Web-Services_7_Elastic-beanstalk/)

- [Notes on AWS - Part 8 - Command Line Interface](/2020/03/02/Notes-on-Amazon-Web-Services_8_Command-line-interface/)

### References ###

- [Backspace Academy AWS Certification Course](http://cdn.backspace.academy/courses/aws-certification/01/010/references-01-01.pdf)

- [Amazon Simple Storage Service (S3) - Getting Started Guide](https://docs.aws.amazon.com/AmazonS3/latest/gsg/s3-gsg.pdf)

- [Amazon Simple Storage Service (S3) - Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-dg.pdf)

- [Amazon Relational Database Service (RDS) - User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-ug.pdf)

- [Amazon Elastic Cloud Compute (EC2) - User Guide for Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-ug.pdf)

- [Amazon Simple Email Service (SES) - Developer Guide](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/ses-dg.pdf)

- [Amazon CloudWatch - User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/acw-ug.pdf)

- [AWS Elastic Beanstalk - Developer Guide](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/awseb-dg.pdf)

- [AWS Command Line Interface (CLI) - User Guide](https://docs.aws.amazon.com/cli/latest/userguide/aws-cli.pdf)

- [AWS Command Line Interface (CLI) - Reference](https://docs.aws.amazon.com/cli/latest/reference/)

- [AWS Billing and Cost Management - User Guide Version 2.0](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/awsaccountbilling-aboutv2.pdf)
