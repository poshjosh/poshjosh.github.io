---
path: "./2020/03/02/Notes-on-Amazon-Web-Services_7_Elastic-beanstalk.md"
date: "2020-03-02T17:10:00"
title: "Notes on Amazon Web Services 7 - Elastic Beanstalk"
description: "Easy to understand tutorial: Notes on Amazon Web Services 7 - Elastic beanstalk"
lang: "en-us"
---

### Elastic Beanstalk ###

Elastic Beanstalk is a deployment service which allows you to deploy your application to complex artchitectures and environments on AWS. It can create highly available and fault tolerant achitectures.  It automatically handles capacity provisioning, load balancing, scaling and application health and monitoring.

New versions of an app could be uploaded through the console or CLI. Complete envirions could also be re-deployed. Applications could be:

 - Docker Containers
 - NodeJS, Java, .NET, PHP, Ruby, Python & Go.
 - On servers like Apache, Nginx, Passenger, IIS

You supply your code and elastic beanstalk will do the rest automatically.

- Create application  
- Upload a version  
- Launch environment (EC2 instances)
- Manage environment

Highly Available and Fault Tolerance.

- AWS Cloud comprises of regions.
- Regions comprises of Availability Zones (AZ)
- Availability Zones (AZ)
- Our VPC launched across multiple AZs
- Load balancing + auto scaling group

### Deployment options ###

- All at once. Remember your app will be down during deployment. Hence if you have 20 EC2 instances all at once could take a while and your app will be down during thi time.

- Rolling (a batch at a tie), Rolling with additional batch. Deploy in batches  to minimize downtime. Additional batch (e.g 2 environments if a batch is 2) created during deployment to prevent downtime.

- Immutable (2 environments temporarily). If you have 10 EC2 instances, it will create addtional 10 EC2 instances during deployment to prevent down-time.

- Blue - Green (2 environments) - One environ could be a developing while the other could be production. Deployment makes the new environ production and the old development environ. Domain name for both environs are switched automatically for you to prevent downtime.

### Highly Available and Fault Tolerant NodeJS App ###

Management Console -> Services -> Elastic Beanstalk -> Get Started

Create a new app -> Select type - > Configure more options
- Select high availability
- After creation, the endpoint URL will be at the top

### Acronyms ###

- HA - Highly Available
- FT - Fault Tolerance

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
