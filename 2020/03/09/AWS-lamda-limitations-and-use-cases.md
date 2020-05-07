---
path: "./2020/03/09/AWS-lamda-limitations-and-use-cases.md"
date: "2020-03-09T21:59:59"
title: "AWS Lamda - Limitations and Use Cases"
description: "Easy to understand tutorial: AWS lamda limitations and use cases"
lang: "en-us"
---

### AWS Lambda Limitations ###

Runtime environment limitations as of 21/01/2020:

-    The disk space is limited to 512 MB.
-    The default deployment package size is 50 MB.
-    Memory range is from 128 to 3008 MB. Previously, the maximum amount of memory available to your functions was 1536 MB.
-    Maximum execution timeout for a function is 15 minutes.
-    Request limitations by Lambda: Request and response body payload size are maximized to 6 MB.
-    The event request body can be up to 128 KB.
-    Lambda Cold Start. Takes some time for the Lambda function to handle the first request, because Lambda has to start a new instance of the function. This means infrequently-used serverless code may suffer from greater response latency. (This can be mitigated by periodically pinging your function.)
-    Application dependencies can be troublesome, especially if third-party libraries link to external packages like C programs for Python code. This becomes a problem with the 50 MB package size limitation.
-    Monitoring Lambda via cloud watch logs can get costly if you need that sort of thing.

Depending on how you look at this limitation, it’s not all doom and gloom. These
limitations can be looked at from a different perspective as a design choice that
AWS made.

In order to have a serverless model and architecture, it’s imperative to have a
system that’s fast and scalable. Meaning restrictions have to be put in place
in order to strike an optimal balance i.e. trade-offs.

Thus, if you decide to use Lambda, you need to be sure about your use cases that
they fall within the bounds of the limitations.

There are workarounds like persisting your data or state machines that address the limitations but it’s not the objective of this article to address the limitations or provide solutions.

### Use cases ###

- In combination with other AWS products

Lambda integrates very well with most other AWS services.

Manipulating objects in an S3 bucket, processing events from a Kinesis Stream, database
items from a DynamoDB table or messages in an SQS queue, responding to REST API requests, etc.
These are all examples of services that work seamlessly with AWS Lambda. For a full list
of integrations, please refer to the official documentation

- Serverless Website or Mobile App Backend

While static content can be stored in S3 and CloudFront, dynamic API requests can be served
by AWS Lambda in combination with API Gateway or AppSync. With this configuration, it is
possible to have an entire website in serverless platforms.

- Unpredictable, high-variance load

Lambda is usually a good fit for workloads whose demand is unpredicable and highly variable,
due to its highly scalable performance. Traditional infrastructures are provisioned accounting
for peak demand. This leads to waste on idle resources. Although auto-scaling can mitigate,
it may not be able to cope with rapid spikes in demand.

- File Manipulation

A Lambda function can provide a quick and stable way to manipulate files. Images stored in an
S3 bucket, for example, could be automatically converted from JPG to PNG, or have its size and
quality setting reduced for optimized web navigation. This could work for virtually any kind
of file: text, video, compressed, etc.

- Artificial intelligence

Implementing and maintaining an infrastructure to run AI systems on a large scale can be
difficult. Some machine learning frameworks and libraries, such as Scikit Learn, SciPy,
NumPy, spaCy, etc. can run smoothly on AWS Lambda. Models that are too big to deploy with
the Lambda package can be stored in S3 and retrieved on demand. It’s possible to keep the
model in memory for a warm start in the next invocations served by the same Lambda container.

- Disaster recovery

AWS Lambda can be used to automate tasks such as EBS snapshot and AMI creation to backup
resources when configuring EC2 instances. Backup images can be stored in S3, for example.
Lambda can also be used to restore backup images and run CloudFormation templates.

- Extract, Transform, Load (ETL)

ETL jobs can be easily automated and scaled with AWS Lambda. A JSON object can be stored in
S3, normalized in Lambda to insert in an RDS database, or validated to save in a DynamoDB table,
for example. Later, Lambda could optimize the JSON using a columnar format to store back in S3
and serve fast and efficient analytical queries with AWS Athena.

### References ###

- [Medium.com Use Cases for AWS Lamda](https://medium.com/better-programming/7-out-of-the-box-use-cases-for-aws-lambda-30d7dc5c99f7)

- [Dashbird.io - Use Cases for AWS Lambda](https://dashbird.io/knowledge-base/aws-lambda/use-cases/)
