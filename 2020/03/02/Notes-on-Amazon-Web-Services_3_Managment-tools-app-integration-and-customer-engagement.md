---
path: "./2020/03/02/Notes-on-Amazon-Web-Services_3_Managment-tools-app-integration-and-customer-engagement.md"
date: "2020-03-02T10:55:00"
title: "Notes on Amazon Web Services 3 - Managment Tools, App Integration and Customer Engagement"
description: "Easy to understand tutorial: Notes on Amazon Web Services 3 - Managment tools app integration and customer engagement"
lang: "en-us"
---

# Notes on Amazon Web Services (AWS) - Part 3 - Management Tools, App Integration and Customer Engagement #
_{author: [Chinomso Ikwuagwu](https://github.com/poshjosh), dated: 2020-03-02T04:00:00+01:00}_
<br/>
________________________________________________________________________________

### AWS Management Tools ###
Clound Formation - Allows for infra as code via text file
AWS Service Catalog - Allows common governance and compliance for IT resources by clearly defining what is allowed to be deployed to AWS
ClounWatch - For monitoring, could be used for triggering auto scaling ops or providing insight.
AWS Systems Manager - Unified UI for viewing ops data from various AWS services and to automate tasks.
CloudTrail - Monitors and logs activities .. management console, developer kits etc.
AWS Config - Access, evaluate the configuration of AWS configs. Change management.
OpsWorks - Provides managed instances of Chef and Puppet. Both could be used to configure and automate the deployment of AWS resources
TrustedAdvisor - Online expert sys analyze AWS account and advise how to achieve high security and best performance.

### Billing and Cost Management Console - Simple Notification Service ###

- Click your account (top right) -> My Billing Dashboard
- Select Receive Billing Alerts
- Click on Services (Top Left) -> Cloud Watch
- Alarms -> Metric -> Billing to set up billing alarm
- AFter creating an alarm, goto Actions and create an SNS topic for the email alert.
- Give your alarm a name and/description

### Application Integration

- AWS Step Functions - Makes it easy to coordinate the components of distributed apps and microservices via a visual workflow. Parallel or sequential functions may be defined visually as a series of setps
- Simple Worflow Service (SWF) - Similar to step functions.
- Simple Notification Service (SNS) - Flexible, fully managed pub-sub messaging service. Could be used for push notifications for mobile devices
- Fully managed message queueing service. Enable to decouple apps from demand as message build up in a queue till the app can catch up with demand

### Example Usage for Decoupling App from Demand ###
- Application e.g a process server (not a webapp)
- Autoscaling EC2 group. Add more instances as demand increases
Autoscaling group would not be able to scale quick enough when a very large spike in demand occurs over a short period like 1 second because it takes 5 - 100 minutes to autoscale.
- High demand could lead to continous queue growth. In such case we set up cloud watch notification to let use know.
- We use the cloud watch to notify the autoscaling group to increase the number of instances or otherwise.

### Customer Engagement Services ###
- Cloud Connect - Self service contact centre which is pay as you go and UI based such that you use a UI to create process flows which define customer interactions.
- Amazon Pinpoint - Allows us to send email, sms and mobile push message for mobile marketing as well as direct messages such as email confirmation.
- Simple Email Service (SES) - Cloud based, bulk email sending service.

### Sending and Email with SES ###
- Services -> Messaging Services -> SES
Enter and verify your email address
- Request Sending Limit Increase. To prevent spam... there is a process you have to go through to use the SES for high volume of emails.

### Notes ###

- US east north virginia billing is the only region you can be in to set up billing on your account.
- New apps are recommended to use Step Functions

### Acronyms ###

- SNS - Simple Notification Service
- SWF - Simple Workflow Service
- SNS - Simple Notification Service
- SQS - Simple Queue Service
- SES - Simple Email Service

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
