---
path: "./2020/03/02/Notes-on-Amazon-Web-Services_1_Introduction.md"
date: "2020-03-02T09:00:00"
title: "Notes on Amazon Web Services 1 - Introduction"
description: "Easy to understand tutorial: Notes on Amazon Web Services 1 - Introduction"
lang: "en-us"
---

### On Billing ###

It is useful to create CloudWatch billing alarms, this way you can be notified when your AWS usage exceeds a dollar amount you specify. I often run load tests with a lot of EC2 instances, which if left unattended can result in a VERY high AWS bill. Billing alarms have saved me many times from getting nasty surprises at the end of the month.

### On Adding Elastic Cloud Compute (EC2) Instance ###

- Terminate your instances once you are done running, to avoid costs

- Auto Scaling Groups
Help specify how to scale up e.g at 90% capacity add n number of instances, or at 40% remove n instances

- Start with 2-4 instances for redundancy then add auto scaling group

- Spot instance. Bids we place which we win, when our bid price is competitive. Add spot instances during maybe periods of high load.

- Use tags to identify instances

E.g name=QA System, Manager = John Doe, Type = QA

### On Adding Volumes (EBS volume) ###
- t2.micro image does not have some volume options e.g throughput optimized volume which uses a minimum of 500G

- Ensure encryption of volumes to secure data

- You can create and attach additional volumes to existing EC2 instances

- Create snapshot volumes as kind of template volumes which does not get used directly but as kind of base image for future volumes - CONFIRM

### On Adding Security Group ###

- Inbound
  * HTTP - Anywhere  - For public users
  * HTTPS - Anywhere - For public users
  * SSH - My IP  - For me

- Outbound
  * All traffic - Anywhere - Your app should be able to do all

### On Adding RDS ###

- Use Aurora MySQL

- Use Multi - AZ deployment to spread your database across multiple availability zones.
The above in additon to proper backup... is necessary for recovery capability

- Leave the db to be publicly available then use a security group to secure access

- Enable backup - do not leave the default of 7 days (every 7 days)

- Be careful allowing automatic upgrades... it could break your system.

### On AWS Web Hosting ###

AWS Home page -> Properties -> Static Website Hosting

AWS Home page -> Logging
Logging enables us track changes occuring in our S3 bucket

Create a bucket specifically for logging
And enable logging on existing buckets referencing the bucket created for logging

### On EC2 volumes vs S3 Buckets ###
Why we put data, even for EC2 on S3 buckets
- EC2 volumes are for computations
- EC2 volumes are more expensive than S3 buckets
- EC3 volumes are not persistent?????????????

### On Adding S3 Bucket ###
- Create S3 Bucket
- Add Permissions
- Create folders
- Upload files
- Grant permissions to individual files

- Welcome Page
You could give an S3 bucket a welcome page to act as a welcome page (usually with links to s3 bucket content)

- Versioning for S3 buckets
Versioning allows us store different versions of a file over time

### Acronyms ###
- EC2 - Elastic Cloud Compute
- IOPs - Input Output Per Seconds
- SSD - Solid State Disk
- RDP - Remote Desktop Protocol
- EBS - Elastic Block Storage
- RDS - Relational Database Service
- S3 - Simple Storage Service

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
