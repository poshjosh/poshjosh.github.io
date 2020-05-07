---
path: "./2020/03/02/Notes-on-Amazon-Web-Services_8_Command-line-interface.md"
date: "2020-03-02T18:28:00"
title: "Notes on Amazon Web Services 8 - Command Line Interface (CLI)"
description: "Easy to understand tutorial: Notes on Amazon Web Services 8 - Command line interface"
lang: "en-us"
---

### Command Line Interface (CLI) ###

- AWS API provides for communication with AWS throw HTTP/s calls

- AWS API documentation available for many services

- Utiilized by:

  * AWS Management Console
  * AWS Command Line Interface (CLI)
  * AWS Software Development Kits (SDKs)
  * Other AWS services.

- API calls can only be made by valid security credentials:

  * Console access - Account username and password
  * CLI - IAM user access key ID and secret. IAM user downloads access key ID and secret to be able to issue CLI commands.
  * SDKs - IAM temporary credentials. Your app may use google, facebook etc to authenticate external users e.g via Oauth2. Temporary Oauth2 credentails are thus issued by google, facebook etc. With such credentials you could have limited and temporary access to AWS cloud.

- API calls can be logged cusing CloudTrail service.

### AWS CLI Application ###

- AWS CLI application is available for Windows, Mac, Linux. It allows API Commands to be sent to AWS using command line for Windows, terminal for Linux?Mac.

- AWS Shell cross platform standalone integrated shell environment written in Python.

- AWS Tools for Windows PowerShell

### AWS Cloud9 IDE ###

- An Integrated Development Environment (IDE) running on EC2 accessed throw the AWS Management Console.

- AWS CLI application pre-installed

- Increased security as IAM credentials are not saved on computer.

- When used in conjuction with multi-factor authentication (MFA), account cannot be access with username and password only.

### AWS CLI Usage ###

- Management Console -> Services -> Cloud9 -> Create Environment

- Add at least a name

- Select: Create a new instance

  * Select the type of instance
  * Select the platform
  * Select cost saving

- The enviroment is created in a VPC

- Settings -> Preferences -> Themes

### Sample CLI commands via Cloud9 IDE ###

- Check if AWS CLI is installed:

```
$aws --version
```

- Create an s3 bucket named mybucket

```
$aws s3 mb s3://mybucket
```

- Copy a document named Notes.pdf to an s3 bucket named mybucket
_Note: First upload the document named Notes.pdf to the Cloud9 directory_

```
$aws s3 cp Notes.pdf s3://mybucket
```

- Delete the document named Notes.pdf from the s3 bucket named mybucket

```					
$aws s3 rm s3://mybucket/Notes.pdf
```

### Notes ###

- Using AWS Cloud9 IDE service for sending CLI commands provides increased security because you don't have to download credentials, thus reducing exposure. You may ask, what about the username and password used to connect to managment console... well you could enable multi-factor authentication on those.

- Select cost saving when creating a Cloud9 environment EC2 instance. This saves cost by going into hibernation when the EC2 instance is not being used.

- To view cli reference, browse to: aws.amazon.com/cli -> click CLI Reference

- S3 api vs S3 commands - S3 api commands are far more in number, the commands are more verbose and powerful.

- To delete a Cloud9 environment, Click on Cloud9 -> Select your environment -> Click delete.

### Acronyms ###

- CLI - Command Line Interface
- SDK - Software Development Kits   
- IDE - Integrated Development Environment
- MFA - Multi-factor Authentication

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
