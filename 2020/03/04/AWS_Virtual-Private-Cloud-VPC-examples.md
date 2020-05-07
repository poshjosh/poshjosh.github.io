---
path: "./2020/03/04/AWS_Virtual-Private-Cloud-VPC-examples.md"
date: "2020-03-04T22:42:10"
title: "AWS Virtual Private Cloud (VPC) Examples"
description: "Explains, with diagrams, scenarios for Virtual Private Clouds (VPC)"
tags: ["Virtual Private Cloud (VPC)", "private subnet", "public subnet", "AWS Resource Access Manager", "Network Address Translation", "Classless Inter-domain Routing", "sharing subnets", "control access to instances", "vpc security", "aws private link", "vpc peering"]
lang: "en-us"
---

__A 10 minute read, to depict various scenarios for Virtual Private Clouds__

### Acronyms ###

- CIDR - Classless Inter-Domain Routing
- CLI - Command Line Interface
- IP - Internet Protocol
- NAT - Network Address Translation
- SDK - Software Development Kit
- VPC - Virtual Private Cloud

### Sharing Public Subnets and Private Subnets ###

In this scenario, one account is responsible for the infrastructure, including
subnets, route tables, gateways, and CIDR ranges. This account is also
responsible for allowing other accounts in the same AWS Organization use the
subnets.

Account A (VPC Owner) uses `AWS Resource Access Manager` to create a Resource
Share for sharing private and public subnets.

- Private subnet shared with Accounts B and C.

- Public subnet shared with Account D

Account A manages the IP infrastructure, including the route tables for both the public
and private subnets. There is no additional configuration required for shared subnets,
so the route tables are the same as unshared subnet route tables.

Each account can only see the subnets that are shared with them, for example, Account D can only see the public subnet. Each of the accounts can control their resources, including instances, and security groups

AWS VPC - Share internet gateway example
<br/>
![AWS VPC - Share internet gateway example](https://docs.aws.amazon.com/vpc/latest/userguide/images/VPC-share-internet-gateway-example.png "AWS VPC - Share internet gateway example")
<br/>
AWS VPC - Share internet gateway example. Source: _docs.aws.amazon.com_

### Controlling Access to Instances in a Subnet ###

In this example, instances in your subnet can communicate with each other, and are
accessible from a trusted remote computer. The remote computer might be a computer in
your local network or an instance in a different subnet or VPC. You use it to connect
to your instances to perform administrative tasks. Your security group rules and
network ACL rules allow access from the IP address of your remote computer
(```172.31.1.2/32```). All other traffic from the internet or other networks is denied.

NACL Example
<br/>
![NACL Example](https://docs.aws.amazon.com/vpc/latest/userguide/images/nacl-example-diagram.png)
<br/>
NACL Example. Source: _docs.aws.amazon.com_

All instances use the same security group with the following rules:

Inbound Rules
-------------------------------------------------------------------------
Protocol Type |	Protocol      |	Port Range    |	Source 	      |	Comments
-------------------------------------------------------------------------
All traffic   |	All 	      |	All 	      |	sg-1a2b3c4d   |	Enables instances that are associated with the same security group to communicate with each other.
SSH 	      |	TCP 	      |	22 	      |	172.31.1.2/32 |	Allows inbound SSH access from the remote computer. If the instance is a Windows computer, this rule must use the RDP protocol for port 3389 instead.
-------------------------------------------------------------------------

Outbound Rules
-------------------------------------------------------------------------
Protocol Type |	Protocol      |	Port Range    |	Destination   |	Comments
-------------------------------------------------------------------------
All traffic   |	All 	      |	All           |	sg-1a2b3c4d   |	Enables instances that are associated with the same security group to communicate with each other. Security groups are stateful. Therefore you don't need a rule that allows response traffic for inbound requests.
-------------------------------------------------------------------------

The subnet is associated with a network ACL that has the following rules:

Inbound Rules
------------------------------------------------------------------------------------------
Rule # 	Type 	Protocol 	Port Range 	Source 		Allow/Deny 	Comments
------------------------------------------------------------------------------------------
100 	SSH 	TCP 		22 		172.31.1.2/32 	ALLOW 		Allows inbound traffic from the remote computer. If the instance is a Windows computer, this rule must use the RDP protocol for port 3389 instead.
* 	All 	All 		All 		0.0.0.0/0 	DENY 		Denies all other inbound traffic that does not match the previous rule.
------------------------------------------------------------------------------------------

Outbound Rules
---------------------------------------------------------------------------------------------------
Rule # 	Type 		Protocol 	Port Range 	Destination 	Allow/Deny 	Comments
---------------------------------------------------------------------------------------------------
100 	Custom TCP 	TCP 		1024-65535 	172.31.1.2/32 	ALLOW 		Allows outbound responses to the remote computer. Network ACLs are stateless. Therefore this rule is required to allow response traffic for inbound requests.
* 	All traffic 	All 		All 		0.0.0.0/0 	DENY 		Denies all other outbound traffic that does not match the previous rule.
---------------------------------------------------------------------------------------------------

The above scenario gives you the flexibility to change the security groups or security
group rules for your instances, and have the network ACL as the backup layer of defense.
The network ACL rules apply to all instances in the subnet. If you accidentally make
your security group rules too permissive, the network ACL rules continue to permit
access only from the single IP address.

To view VPC security best practices as well as recommended rules for various scenarios
[click here](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-best-practices/ "VPC security best practices")

### Services Using AWS PrivateLink and VPC Peering ###

An AWS PrivateLink service provider configures instances running services in their
VPC, with a Network Load Balancer as the front end. Use intra-region VPC Peering
(VPCs are in the same Region) and inter-region VPC Peering (VPCs are in different
Regions) with AWS PrivateLink to allow private access to consumers across VPC
peering connections.

Consumers in remote VPCs cannot use [Private DNS Names for Endpoint Services](https://docs.aws.amazon.com/vpc/latest/userguide/verify-domains/)
names across peering connections. They can however create their own private hosted
zones on Route 53, and attach it to their VPCs to use the same Private DNS name.
For information about using transit gateway with Amazon Route 53 Resolver, to
share PrivateLink interface endpoints between multiple connected VPCs and an
on-premises environment, see
[Integrating AWS Transit Gateway with AWS PrivateLink and Amazon Route 53 Resolver](https://aws.amazon.com/blogs/networking-and-content-delivery/integrating-aws-transit-gateway-with-aws-privatelink-and-amazon-route-53-resolver/ "Integrating AWS Transit Gateway with AWS PrivateLink and Amazon Route 53 Resolver")

Examples

_@TODO Explode these examples_

- [Example: Service Provider Configures the Service](https://docs.aws.amazon.com/vpc/latest/userguide/vpc--region-peering-provider-side/)

- [Example: Service Consumer Configures Access](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-region-peering-consumer-side/)

- [Example: Service Provider Configures a Service to Span Regions](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-inter-region-peering-provider-side/)

- [Example: Service Consumer Configures Access Across Regions](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-inter-region-peering-consumer-side/)

### References ###

- [AWS - VPC share](https://docs.aws.amazon.com/vpc/latest/userguide/example-vpc-share/ "AWS - VPC Share")

- [VPC security best practices](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-best-practices/ "VPC security best practices")

- [AWS Privatelink and VPC peering](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-peer-region-example/ "AWS Privatelink and VPC peering")
