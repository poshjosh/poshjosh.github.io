---
path: "./2020/04/04/Amazon-DynamoDB-Accelerator-DAX.md"
date: "2020-04-04T23:49:28"
title: "Amazon DynamoDB Accelerator (DAX)"
description: "Curated information on Amazon DynamoDB Accelerator (DAX)"
tags: ["DynamoDB Accelerator (DAX)", "DynamoDB", "enable DAX", "use DAX", "deploy application with DAX client", "benefits DAX", "DAX client"]
lang: "en-us"
---

### Acronyms ###

DAX - DynamoDB Accelerator
IAM - Identity and Access Management
VPC - Virtual Private Cloud
VPN - Virtual Private Network

### TL;DR ###

Scroll down to the end of the article and read the `takeaways`.

### What is DynamoDB Accelerator (DAX) ###

Amazon DynamoDB Accelerator (DAX) is a fully managed, highly available,
in-memory cache for DynamoDB that delivers up to a 10x performance improvement
(from milliseconds to microseconds) even at millions of requests per second.

DAX does all the heavy lifting required to add in-memory acceleration to your
DynamoDB tables, without requiring developers to manage cache invalidation,
data population, or cluster management.

You do not need to modify application logic, since DAX is compatible with
existing DynamoDB API calls.

Enable DAX via the AWS Management Console or SDK

### Benefits of Enabling DAX ###

- Extreme Performance
  * DynamoDB = consistent single-digit millisecond latency
  * DynamoDB + DAX = response times in microseconds for millions of requests per second for read-heavy workloads.

- Highly Scalable. You can start with a three-node DAX cluster and scale up to 10 nodes
giving you millions of requests per second.

- Fully Managed. DAX is fully managed by AWS just like DynamoDB

- Ease of Use. DAX is tightly integrated with Amazon DynamoDB – you simply
provision a DAX cluster, use the DAX client SDK to point your existing DynamoDB
API calls at the DAX cluster, and let DAX handle the rest. Because DAX is
API-compatible with DynamoDB, there is no need to make any functional
application code changes.

- Cost Savings. You may be able to reduce the provisioned read capacity of
DynamoDB and lower overall operational cost, because the retrieval of cached
data reduces the read load on existing DynamoDB tables.

- Flexible. DAX enables you to provision one DAX cluster for multiple DynamoDB
tables, multiple DAX clusters for a single DynamoDB table or somewhere in
between giving you maximal flexibility.


> - Secure. DAX fully integrates with AWS services to enhance security. You can
> use Identity and Access Management (IAM) to assign unique security credentials
> to each user and control each user's access to services and resources. Amazon
> CloudWatch enables you to gain system-wide visibility into resource utilization,
> application performance, and operational health. Integration with AWS CloudTrail
> enables you to easily log and audit changes to your cluster configuration. DAX
> supports Amazon Virtual Private Cloud (VPC) for secure and easy access from your
> existing applications. Tagging provides you additional visibility to help you
> manage your DAX clusters.

### How DAX Works ###

Amazon DynamoDB Accelerator (DAX) is designed to run within an Amazon Virtual
Private Cloud (Amazon VPC) environment.

An Amazon VPC service defines a virtual network that closely resembles a
traditional data center. With a VPC, you have control over its IP address range,
subnets, routing tables, network gateways, and security settings.

You can launch a DAX cluster in your virtual network (VPC) and control access
to the cluster by using Amazon VPC security groups.

Diagram showing a high-level overview of DAX
<br/>
![](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/images/dax_high_level.png)
<br/>
Diagram shows a high-level overview of DAX. Source: _docs.aws.amazon.com_

- Enable DAX via the AWS Management Console or SDK

- Deploy your application with the DAX client.

- At runtime, the DAX client directs all of your application's DynamoDB API
requests to the DAX cluster. If DAX can process one of these API requests
directly, it does so. Otherwise, it passes the request through to DynamoDB.

### Takeaways ###

- Amazon DynamoDB Accelerator (DAX) is a fully managed service.

- DAX delivers up to a 10x performance improvement (from milliseconds to
microseconds) even at millions of requests per second.

- You do not need to modify application logic, since DAX is compatible with
existing DynamoDB API calls.

- You can launch a DAX cluster in your virtual network (VPC) and control access
to the cluster by using Amazon VPC security groups.

- Steps to enable and use DAX:

  * Enable DAX via the AWS Management Console or SDK

  * Deploy your application with the DAX client.

  * At runtime, the DAX client directs all of your application's DynamoDB API
  requests to the DAX cluster. If DAX can process one of these API requests
  directly, it does so. Otherwise, it passes the request through to DynamoDB.

### References ###

- [AWS DAX](https://aws.amazon.com/dynamodb/dax/)

- [AWS DAX Concepts](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.concepts.html)
