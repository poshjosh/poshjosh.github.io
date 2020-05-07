---
path: "./2020/03/16/AWS_VPC-endpoints-and-VPC-endpoint-services.md"
date: "2020-03-16"
title: "AWS VPC Endpoints and VPC Endpoint Services (AWS Private Link)"
description: "Easy to understand info on AWS: VPC Endpoints and Endpoint services (AWS Private Link)"
lang: "en-us"
---

### Acronyms ###

- VPC - Virtual Private Cloud

### VPC Endpoint Services (AWS Private Link) ###

You can create your own application in your VPC and configure it as an
AWS PrivateLink-powered service (referred to as an endpoint service). Other AWS
principals can create a connection from their VPC to your endpoint service using
an interface [VPC Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html).
You are the _service provider_, and the AWS principals that create connections
to your service are _service consumers_.

In the following diagram, the account owner of VPC B is a service provider, and has a service running on instances in subnet B. The owner of VPC B has a service endpoint (vpce-svc-1234) with an associated Network Load Balancer that points to the instances in subnet B as targets. Instances in subnet A of VPC A use an interface endpoint to access the services in subnet B.

__Illustration of VPC Endpoint Service__
<br/>
[VPC Endpoint Service](https://docs.aws.amazon.com/vpc/latest/userguide/images/vpc-endpoint-service.png)
<br/>
Illustration of VPC Endpoint Service. Source: _docs.aws.amazon.com_

For low latency and fault tolerance, we recommend using a Network Load Balancer with targets in every Availability Zone of the AWS Region. To help achieve high availability for service consumers that use zonal DNS hostnames to access the service, you can enable cross-zone load balancing. Cross-zone load balancing enables the load balancer to distribute traffic across the registered targets in all enabled Availability Zones. For more information, see
[Cross-Zone Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/network-load-balancers.html#cross-zone-load-balancing)

In the following diagram, the owner of VPC B is the service provider, and it has configured a Network Load Balancer with targets in two different Availability Zones. The service consumer (VPC A) has created interface endpoints in the same two Availability Zones in their VPC. Requests to the service from instances in VPC A can use either interface endpoint.

__Illustration of VPC Endpoint Service - Multi AZ__
<br/>
[AWS - VPC Endpoint Service - Multi AZ](https://docs.aws.amazon.com/vpc/latest/userguide/images/vpc-endpoint-service-multi-az.png)
<br/>
Illustration of VPC Endpoint Service - Multi AZ. Source: _docs.aws.amazon.com_

### Considerations ###

- Note that there is a charge for data transfer between Regions.

- __Note__ The AZ `us-east-1a` for your AWS account might not be the same
location as `us-east-1a` for another AWS account. This is because, AWS ensures
that resources are distributed across the AZs by _independently mapping AZs to
names for each AWS account_.

- To coordinate AZs across accounts, you must use the AZ ID, which is a unique
and consistent identifier for an Availability Zone. For example, `use1-az1` is
an AZ ID for the `us-east-1` Region and it has the same location in every AWS
account.

### References ###

- [VPC Endpoint Services](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-service.html)

- [VPC Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html)

- [Cross-Zone Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/network-load-balancers.html#cross-zone-load-balancing)
