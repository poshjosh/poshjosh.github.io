---
path: "./2020/03/02/Notes-on-Amazon-Web-Services_5_Security-identity-and-compliance.md"
date: "2020-03-02T14:33:00"
title: "Notes on Amazon Web Services 5 - Security, Identity and Compliance"
description: "Easy to understand tutorial: Notes on Amazon Web Services 5 - Security identity and compliance"
lang: "en-us"
---

### Security ###

- Artifact - Online protal for access to AWS security and compliance documentation.  
- Cerificate Manager - Issues free SSL certificates. Integrates with Route 53 and Cloud Front.
- Cloud Directory - Cloud based directory service that can have heirarchies of data in multiple dimensions.
- Directory Service - Fully managed Microsoft active directory service in AWS cloud
- Could Hardware Security Module (HSM) - Dedicated hardware security module in the AWS cloud.
- Amazon Cognito - Sign in and sign up capabilities for web apps, Oauth2 and SAML2 supported.
- Identity and Access Management (IAM) - Allows you to manage user access to AWS services and resources with permissions.
- AWS Organizations - Provides policy based management for multiple AWS accounts.
- AWS Inspector - Automated security accessment service. Can identify vulnerabilities and areas needing improvement.
- Key Management Service (KMS) - Create and control encryption keys for encrypted data.. uses hardware security module to protect your keys.
- AWS Shield - Provides protection against DDoS. Standard version of Shield implemented automatically on all AWS accounts.
- Web Application Firewall - Sits in front of your website to provide additional protection against common attacks such as SQL injection and XSS.

### Usage of IAM ###
- Login with your email and password  .. implies root user with all permissions.
- Make sure your root account is very secure.
  * Signin as root user
  * Goto: Your account -> My security credentials
  * Use a very good password for your root user
  * Use multi factor authentication for root user
- Create IAM users with assigned permissions and use that user.
- [Click here for steps to create IAM user](/2020/02/02/Amazon-Web-Services-Create-IAM-User/)

### Notes ###

- After creating IAM user, login url will change from ```https://aws.amazon.com/console``` to ```https://<your_aws_account_id>.signin.aws.amazon.com/console/```

### Acronyms ###
- HSM - Hardware Security Module
- IAM - Identity and Access Management
- KMS -Key Management Service
- WAF - Web Application Firewall

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
