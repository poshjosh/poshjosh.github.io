---
path: "./2020/03/02/Notes-on-Amazon-Web-Services_4_Analytics-and-machine-learning.md"
date: "2020-03-02T12:15:00"
title: "Notes on Amazon Web Services 4 - Analytics and Machine Learning"
description: "Easy to understand tutorial: Notes on Amazon Web Services 4 - Analytics and machine learning"
lang: "en-us"
---

### Anaytics ###


- Elastic Map Reduce (EMR) - Hadoop framework as a service. Data can be analyzed by EMR in AWS data stores like S3 and DynamoDb etc.
- Athena - Allows you to analyze data stored in an amazon S3 bucket using standard SQL queries
- Elasticsearch Service - Managed service for elasticsearch framework.
- Kinesis - Process and analyze real time streaming data
- QuickSight - Business intelligence tool

### Maching Learning ###

- DeepLens - Deep learning enabled video camera which has a deep learning SDK that allows you to create advanced vision system apps.
- SageMaker - Flagship machine learning product. Enables you build your own machine learning apps and deploy it to the AWS cloud.
- AmazonRekognition - Provides deep learning based analysis of videos and images.
- Lex - Allows you to build conversational chatbots.
- Polly - Natural sounding tex to speech
- Comprehend - Use deep learning analyze text for insights and relationships. Usable for customer analysis and searching of documents etc
- Translate - Use machine learning to acurately translate to various languages
- Transcribe - Automatic speech recognition service that can analyze audio files stored in amazon s3 and return the transcribed text.

### Amazon Rekognition Use Case ###

- Management Console -> Services -> Rekognition
- Send a request to the Amazon Rekognition api though the SDK e.g javascript SDK or java SDK.
- Receive a response with all the labels and the percentage probability of occurrence.


### Note ###

- Amazon Rekognition could be used for image moderation.. i.e to ensure safe content.

### Acronyms ###

- EMR - Elastic Map Reduce

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
