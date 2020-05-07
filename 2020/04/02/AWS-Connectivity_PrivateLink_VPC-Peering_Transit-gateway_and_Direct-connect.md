---
path: "./2020/04/02/AWS-Connectivity_PrivateLink_VPC-Peering_Transit-gateway_and_Direct-connect.md"
date: "2020-04-02"
title: "AWS Connectivity - PrivateLink, VPC-Peering, Transit-gateway and Direct-connect"
description: "Simplified - AWS PrivateLink, VPC Peering, Transit Gateway, Direct connect"
tags: ["Virtual Private Cloud (VPC)", "AWS Connectivity", "PrivateLink", "VPC Peering", "Transit Gateway", "Direct Connect", "Virtual Private Network (VPN)", "Network Address Transation (NAT)", "AWS Resource Manager"]
lang: "en-us"
---

### Acronyms ###

- AWS - Amazon Web Services
- NAT - Network Address Translation
- VPC - Virtual Private Cloud
- VPN - Virtual Private Network

AWS is about the cloud. Ergo, it is safe to say that Amazon Virtual Private
Cloud (VPC) is one of the most useful and central features of AWS. VPCs could
be connected via AWS Direct Connect (via Direct Connect Gateways), NAT Gateways,
Internet Gateways, Egress-Only Internet Gateways, VPC Peering, AWS Managed VPN
Connections, PrivateLink and Transit Gateways. In this article we will
elaborate on AWS Private link, VPC Peering, Transit Gateway and Direct connect.

### PrivateLink ###

__AWS PrivateLink__ allows you to privately access services hosted on the AWS
network in a highly available and scalable manner, without using public IPs and
without requiring the traffic to traverse the internet.

PrivateLink provides a convenient way to connect to applications/services
by name with added security. You configure your application/service in your
VPC as an AWS PrivateLink-powered service (referred to as an endpoint service).
AWS generates a specific DNS hostname for the service. Other AWS principals
can create a connection to your endpoint service after you grant them permission.

You can create your own application in your VPC and configure it as an
AWS PrivateLink-powered service (referred to as an endpoint service). Other AWS
principals can create a connection from their VPC to your endpoint service using
an interface [VPC Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html).
You are the _service provider_, and the AWS principals that create connections
to your service are _service consumers_. [More on VPC Endpoints and Endpoint services](/2020/03/16/AWS_VPC-endpoints-and-VPC-endpoint-services)

### VPC Peering ###

__VPC Peering__ offers point-to-point network connectivity between two VPCs.
You can use VPC peering to create a full mesh network that uses individual
connections between all networks.

With VPC peering you connect your VPC to another VPC. Both VPC owners are
involved in setting up this connection. When one VPC, (the visiting) wants
to access a resource on the other (the visited), the connection need not
go through the internet.

### PrivateLink `vs` VPC Peering ###

- PrivateLink - applies to Application/Service

- VPC Peering - applies to VPC

[Click here for more on the differences between VPC Peering and PrivateLink](/2020/04/02/AWS_VPC-peering_vs_PrivateLink)

### Transit Gateways ###

__TL:DR__ Transit gateway allows one-to-many network connections as opposed
to other AWS connectivity types which allow only on-to-one connections.

Transit Gateways solves some problems with VPC Peering. You can use VPC
peering to create a full `mesh network` that uses individual connections
between all networks. However, this can be very complex to manage as the
number of your VPCs grows.

Unlike other AWS connectivity options (which are peer-to-peer) AWS Transit
Gateway allows you to build a `hub-and-spoke network` topology. You can connect
your existing VPCs, data centers, remote offices, and remote gateways to a
managed Transit Gateway, with full control over network routing and security.
This is possible even if your VPCs, Active Directories, shared services, and
other resources span multiple AWS accounts.

### VPC Peering vs Transit Gateways ###

If you have a __VPC Peering connection__ between `VPC A` and `VPC B`, and one
between `VPC A` and `VPC C`, there is no VPC Peering connection
(_transitive peering_) between `VPC B` and `VPC C`. This means you cannot
route packets directly from `VPC B` to `VPC C` through `VPC A`.

This lack of _transitive peering_ in VPC peering is the reason AWS Transit
Gateway was introduced; thus the name __Transit Gateway__. Transitive networks
greatly simplify full, multi-VPC mesh networks where every node is connected
to every other node in the network.

### AWS Direct Connect ###

> __AWS Direct Connect__ is a cloud service solution that makes it easy to
> establish a   dedicated network connection from your premises to AWS. Using
> AWS Direct Connect, you   can establish private connectivity between AWS and
> your datacenter, office, or  colocation environment, which in many cases can
> reduce your network costs, increase  bandwidth throughput, and provide a
> more consistent network experience than Internet  based connections.

> AWS Direct Connect lets you establish a dedicated network connection between
> your network and one of the AWS Direct Connect locations. Using industry
> standard 802.1q VLANs, this dedicated connection can be partitioned into
> multiple virtual interfaces. This allows you to use the same connection to
> access public resources such as objects stored in Amazon S3 using public IP
> address space, and private resources such as Amazon EC2 instances running
> within an Amazon Virtual Private Cloud (VPC) using private IP space, while
> maintaining network separation between the public and private environments.
> Virtual interfaces can be reconfigured at any time to meet your changing needs.

### Notes ###

- AWS Resource Manager is an AWS service that makes it really easy to share
AWS resources across accounts

- AWS Transit Gateway makes use of AWS Resource Manager. An account that owns a
resource simply creates a Resource Share and specifies a list of other AWS
accounts that can access the resource. Transit Gateways were one of the first
resource types that you can share in this fashion.

### References ###

- [VPC Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html)

- [VPC Endpoints and Endpoint services](/2020/03/16/AWS_VPC-endpoints-and-VPC-endpoint-services)

- [AWS VPC Peering vs PrivateLink](/2020/04/02/AWS_VPC-peering_vs_PrivateLink)

- [Use AWS Transite Gateway to simplify your network architecture](https://aws.amazon.com/blogs/aws/new-use-an-aws-transit-gateway-to-simplify-your-network-architecture/)

- [VPC Sharing - A new approach to multiple accounts VPC management](https://aws.amazon.com/blogs/networking-and-content-delivery/vpc-sharing-a-new-approach-to-multiple-accounts-and-vpc-management/)

- [AWS Direct Connect](https://aws.amazon.com/directconnect/)
