---
path: "./2020/04/02/AWS_VPC-peering_vs_PrivateLink.md"
date: "2020-04-02"
title: "AWS - VPC peering vs PrivateLink"
description: "Easily understand - AWS VPC peering vs PrivateLink"
tags: ["Virtual Private Cloud (VPC)", "VPC Peering", "PrivateLInk", "Virtual Private Network (VPN)", "VPC Endpoint Service"]
lang: "en-us"
---

### Acronyms ###

- AWS - Amazon Web Services
- DNS - Domain Name Service
- IAM - Identity and Access Management
- S3 - Simple Storage Service
- VPC - Virtual Private Cloud
- VPN - Virtual Private Network

### VPC Peering `vs` PrivateLink ###

These 2 developed separately, but have more recently found themselves intertwined.

- VPC Peering - applies to VPC

- PrivateLink - applies to Application/Service

### VPC Peering ###

With __VPC Peering__ you connect your VPC to another VPC. Both VPC owners are
involved in setting up this connection. When one VPC, (the visiting) wants
to access a resource on the other (the visited), the connection need not
go through the internet.

### PrivateLink ###

__PrivateLink__ provides a convenient way to connect to applications/services
by name with added security. You configure your application/service in your
VPC as an AWS PrivateLink-powered service (referred to as an endpoint service).
AWS generates a specific DNS hostname for the service. Other AWS principals
can create a connection to your endpoint service after you grant them permission.

### Notes on VPC Peering ###

> __VPC peering__ allows VPC resources including ... to communicate with each
> other using private IP addresses, without requiring gateways, VPN connections,
> or separate network appliances. ...Traffic always stays on the global AWS
> backbone, and never traverses the public internet

> Inter-Region VPC Peering provides a simple and cost-effective way to share
> resources between regions or replicate data for geographic redundancy.

### Notes on Endpoint Services ###

> When you create a __VPC endpoint service__, AWS generates endpoint-specific DNS
> hostnames that you can use to communicate with the service. These names
> include the VPC endpoint ID, the Availability Zone name and Region Name, for
> example, `vpce-1234-abcdev-us-east-1.vpce-svc-123345.us-east-1.vpce.amazonaws.com`.
> By default, your consumers access the service with that DNS name

> When you create an endpoint, you can attach an endpoint policy to it that
> controls access to the _related_ service

> An endpoint policy does not override or replace IAM user policies or
> service-specific policies (such as S3 bucket policies). It is a separate
> policy for controlling access from the endpoint to the specified service.

### VPC Peering + PrivateLink ###

__As of March 7, 2019, applications in a VPC can now securely access AWS
PrivateLink endpoints across VPC peering connections__. AWS PrivateLink
endpoints can now be accessed across both intra- and inter-region VPC peering
connections. [More on this](https://aws.amazon.com/about-aws/whats-new/2019/03/aws-privatelink-now-supports-access-over-vpc-peering/)

### References ###

- [AWS PrivateLink access over VPC peering](https://aws.amazon.com/about-aws/whats-new/2019/03/aws-privatelink-now-supports-access-over-vpc-peering/)

- [AWS - What is VPC Peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)

- [AWS Endpoint access](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-service.html)

- [AWS VPC Endpoint access](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html)
