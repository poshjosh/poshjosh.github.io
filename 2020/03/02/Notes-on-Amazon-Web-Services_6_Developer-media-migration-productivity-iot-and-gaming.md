---
path: "./2020/03/02/Notes-on-Amazon-Web-Services_6_Developer-media-migration-productivity-iot-and-gaming.md"
date: "2020-03-02T15:49:00"
title: "Notes on Amazon Web Services 6 - Developer, Media, Migration, Productivity, IoT and Gaming"
description: "Easy to understand tutorial: Notes on Amazon Web Services 6 - Developer, media, migration, productivity, IoT and gaming"
lang: "en-us"
---

### Developer Tools ###

- Cloud9 - IDE running in AWS Cloud.
- CodeStart - Makes easy to develop and deploy applications to AWS. It can manage the entire CI/CD pipeline for you. It has a project management dashboard including issue management by Jira.
- X-Ray - Analyze and debug.. provides insight into apps peformance.
- CodeCommit - Git repo (like GitHub) in the AWS cloud.
- CodePipeline - CI/CD service
- CodeBuild - Compiles your source code, runs test then produces software packages that are ready to deploy on AWS.
- CodeDeploy - Automates software deployment to a variety of compute services including Amazon EC2, Lamda and local (on premises) instances

### AWS Media Services ###

- Elemental MediaConvert - Managed video transcoding service for converting video format for video on demand content.
- Elemental MeidaPackage - Prepares video content for delivery, protects against piracy by using Digital Rights Management (DRM)
- Elemental MediaTailor - Inserts individually targeted adverts into video stream.
- Elemental MediaLive - For creating video streams for delivery to TVs and internet streaming devices
- Elemental MediaStore - Storage service in the AWS cloud that is optimized for media.
- Kineses Video Streams - Streams from connected devices through the AWS cloud for analytics, machine learning and other processing applications.

### Mobile Services ###

- Mobile Hub - Easily configure your AWS services for mobile apps in one place.
- Device Farm - App testing service for android, ios and web apps. Test your app against a large collection of physical devices in the AWS cloud.
- AppSync - GraphQL backend for mobile and web apps.

### Migration Services ###

- Application Discovery Service - Gathers information about on premises data cen to help plan migration over to AWS.
- Database Migration Service - Ochestrates migration of databases to AWS cloud. Migrate from Oracle to AWS aurora.
- Server Migration Service - Migrate workloads to AWS cloud.
- Snowball - Portable petabyte scale data storage device for migrating data from on premise environment to AWS cloud. Amazon sends you the physical device where you store your data and send back to amazon for uploading to their cloud.

### Business Productivity & App Streaming ###

- WorkDocs - Secure fully managed file collaboration and management service in the AWS cloud. 35 different file types supported.
- WorkMail - Business email and calender service
- Chime - Online meeting service in AWS cloud
- Workspaces - Fully managed, secure desktop as a service. E.g stream a windows desktop to endusers.
- AppStream - Fully managed, secure application streaming service that allows you to stream desktop apps from AWS to web browser. Allows access to your application from anywhere.

### Internet of Things (IoT) ###

- IoT - Managed cloud platform that allows embedded devices (e.g rasberry pi) securely interact with cloud apps and other devices
- FreeRTOS - Operation system for micro controllers. Small low cost and power devices to connect to AWS IoT.
- Greengrass - Allows you run local AWS lamda functions, messaging, data caching, sync on AWS IoT connected devices. Extends AWS services to devices so they can act locally on the data they generate while still using cloud based AWS IoT capabilities

### Game Development ###

- Gamelift - Deploy and manage dedicated game server in the AWS cloud.
- Lumberyard - Game dev environ and cross platform tripple game engine.

### Notes ###

- Avoid creating workspaces, unless really needed bacause creating a Workspace is quick and easy but cleaning it up is not and could risk you getting an AWS bill.

### Acronyms ###

- DRM - Digital Rights Management
- IDE - Integrated Developer Environment
- IoT - Internet of Things

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
