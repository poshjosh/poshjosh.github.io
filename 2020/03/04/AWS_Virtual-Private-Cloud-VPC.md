---
path: "./2020/03/04/AWS_Virtual-Private-Cloud-VPC.md"
date: "2020-03-04T22:20:40"
title: "Curated info on AWS Virtual Private Cloud (VPC)"
description: "Curated info on AWS Virtual Private Cloud (VPC)"
tags: ["Virtual Private Cloud (VPC)", "vpc pricing", "vpc limits", "vpc security", "security group", "network access control list (NACL)", "private subnet", "public subnet", "Network Address Translation", "Classless Inter-domain Routing", "aws private link"]
lang: "en-us"
---

__A 15 minute read, aimed at exposing you to the Amazon cloud network.
Within the amazon cloud network, each private network is designated as a
Virtual Private Cloud (VPC)__

### Acronyms ###

- API - Application Programming Interface
- CIDR - Classless Inter-Domain Routing
- CLI - Command Line Interface
- IP - Internet Protocol
- NAT - Network Address Translation
- SDK - Software Development Kit
- VPC - Virtual Private Cloud

### What is AWS Virtual Private Cloud (VPC)? ###

Amazon Virtual Private Cloud (Amazon VPC) is a virtual network within AWS cloud
which you can launch AWS resources into. This virtual network closely resembles a
traditional network that you'd operate in your own data center, with the benefits of
using the scalable infrastructure of AWS.

The following are the key concepts for VPCs:

- A virtual private cloud (VPC) is a virtual network dedicated to your AWS account.

- A subnet is a range of IP addresses in your VPC.

- A route table contains a set of rules, called routes, that are used to determine
where network traffic is directed.

- An internet gateway is a horizontally scaled, redundant, and highly available VPC
component that allows communication between instances in your VPC and the internet. It
therefore imposes no availability risks or bandwidth constraints on your network traffic.

- A VPC endpoint enables you to privately connect your VPC to supported AWS services
and VPC endpoint services powered by PrivateLink without requiring an internet gateway,
NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do
not require public IP addresses to communicate with resources in the service. Traffic
between your VPC and the other service does not leave the Amazon network.

#### Accessing VPCs ####

You can create, access, and manage your VPCs using any of the following interfaces:

- __AWS Management Console__ — Provides a web interface that you can use to access your VPCs.

- __AWS Command Line Interface (AWS CLI)__ — Provides a command line interface and commands
to access VPCs as well as a broad set of AWS services.

- __AWS SDKs__ — Provides language-specific APIs for accessing VPCs and other AWS services.

- __Query API__ — Provides low-level API actions that you call using HTTPS requests.

#### VPC Pricing ####

There's no additional charge for using Amazon VPC. You pay the standard rates for the
instances and other Amazon EC2 features that you use. There are charges for using an
Site-to-Site VPN connection and using a NAT gateway.

### How Amazon VPCs Work ###

__VPC Limits__

- Subnets per VPC - 200
- Route tables per VPC - 200
- VPCs per region - 5
- IPV4 CIDR blocks per VPC - 5
- Elastic IP addresses per region - 5
- Egress-only internet gateways per Region - 5
- Internet gateways per Region - 5
- [View more AWS VPC Limits](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits/)

__VPCs and Subnets__

A virtual private cloud (VPC) is a virtual network dedicated to your AWS account. It is
logically isolated from other virtual networks in the AWS Cloud.

A subnet is a range of IP addresses in your VPC. You can launch AWS resources into a
specified subnet. Use a public subnet for resources that must be connected to the internet,
and a private subnet for resources that won't be connected to the internet. For more
information about public and private subnets, see [VPC and Subnet Basics](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets/#vpc-subnet-basics).

__Default VPCs and Subnets__

Each amazon account has a default VPC that has a default subnet in each Availability Zone.
A default VPC has the benefits of the advanced features provided by EC2-VPC, and is ready
for you to use. If you have a default VPC and don't specify a subnet when you launch an
instance, the instance is launched into your default VPC.

### VPC Security ###

To protect the AWS resources in each subnet, you can use multiple layers of security,
including security groups and network access control lists (ACL). For more information,
see [Internetwork Traffic Privacy in Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security/).

__Difference between Security Groups and NACLs__

__TL;DR__: _Security group is the firewall of EC2 Instances whereas Network ACL is the firewall of the Subnet._

Difference between Security Groups and NACLs
<br/>
![Difference between Security Groups and NACLs](https://miro.medium.com/max/944/1*pwAjuZMHsDJV6XckZGARxA.png "Difference between Security Groups and NACLs")
<br/>
Difference between Security Groups and NACLs. Source: _miro.medium.com_

- __Scope__: _Subnet or EC2 Instance_

When you apply an NACL to a subnet, it's rules apply to all instances in the
subnet. On the other hand, security groups have to be applied to each instance.
If you have many instances, managing the firewalls using NACL could be very useful

- __State__: _Stateful vs Stateless_

Security groups are stateful. This means any changes applied to an incoming rule
will be automatically applied to the outgoing rule. e.g. If you allow an incoming
port 80, the outgoing port 80 will be automatically opened.

Network ACLs are stateless. This means any changes applied to an incoming rule
will not be applied to the outgoing rule. e.g. If you allow an incoming port 80,
you would also need to apply the rule for outgoing traffic.

- __Rules__: _Allow or Deny_

Security group support allow rules only (by default all rules are denied). e.g.
You cannot deny a certain IP address from establishing a connection.

Network ACL support allow and deny rules. By deny rules, you could explicitly deny
a certain IP address to establish a connection example: Block IP address
`123.201.57.39` from establishing a connection to an EC2 Instance.

- __Rules__: _Process order_

All rules in a security group are applied whereas rules are applied in their order
(the rule with the lower number gets processed first) in Network ACL.i.e. Security
groups evaluate all the rules in them before allowing a traffic whereas NACLs do it
in the number order, from top to bottom.

__Example Security Group Configuration__

The following example shows the security group rules for allowing both IPv4 and IPv6
traffic on port 80 and 443:

__Inbound rules__

| Type   	 	  | Protocol  | Port Range | Source     |
|-------------|-----------|------------|------------|
| HTTP (80) 	| TCP (6)   | 80   	     | 0.0.0.0/0  |
| HTTP (80) 	| TCP (6)   |	80   	     | ::/0       |
| HTTPS (443) |	TCP (6)   |	443  	     | 0.0.0.0/0  |
| HTTPS (443) |	TCP (6)   |	443  	     | ::/0       |

For an NACL example, [click here](https://aws.amazon.com/premiumsupport/knowledge-center/connect-http-https-ec2/)

Because security groups are stateful, the return traffic from the instance to users
is allowed automatically, so you don't need to modify the security group's outbound
rules.

#### More on Security Groups ####

By default, a security group includes an outbound rule that allows all outbound traffic.
You can remove the rule and add outbound rules that allow specific outbound traffic
only. If your security group has no outbound rules, no outbound traffic originating
from your instance is allowed.

Security groups are associated with network interfaces. After you launch an instance,
you can change the security groups that are associated with the instance, which
changes the security groups associated with the primary network interface ```(eth0)```.
You can also specify or change the security groups associated with any other network
interface. By default, when you create a network interface, it's associated with the
default security group for the VPC, unless you specify a different security group.
For more information about network interfaces, see
[Elastic Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni/ "Elastic Network Interfaces").

__Security Group Names__

When you create a security group, you must provide it with a name and a description.

The following rules apply:

- Names and descriptions can be up to 255 characters in length.

- Names and descriptions are limited to the following characters:
```
a-z, A-Z, 0-9, spaces, and ._-:/()#,@[]+=&;{}!$*.
```

- A security group name cannot start with sg-.

- A security group name must be unique within the VPC.

#### More on Network ACLs ####

You can associate a network ACL with multiple subnets. However, a subnet can be
associated with only one network ACL at a time. When you associate a network ACL
with a subnet, the previous association is removed.

The client that initiates the request chooses the ephemeral port range. The range
varies depending on the client's operating system.

- Many Linux kernels (including the Amazon Linux kernel) use ports 32768-61000.

- Requests originating from Elastic Load Balancing use ports 1024-65535.

- Windows operating systems through Windows Server 2003 use ports 1025-5000.

- Windows Server 2008 and later versions use ports 49152-65535.

- A NAT gateway uses ports 1024-65535.

- AWS Lambda functions use ports 1024-65535.

In practice, to cover the different types of clients that might initiate traffic to
public-facing instances in your VPC, you can open ephemeral ports ```1024-65535```.
However, you can also add rules to the ACL to deny traffic on any malicious ports
within that range. Ensure that you place the deny rules earlier in the table than the
allow rules that open the wide range of ephemeral ports.

### Accessing the Internet ###

The default VPC includes an internet gateway, and each default subnet is a public subnet.
Each instance that you launch into a default subnet has a private IPv4 address and a
public IPv4 address. These instances can communicate with the internet through the internet
gateway via the Amazon EC2 network edge.

Default VPC
<br/>
![Default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/images/default-vpc-diagram.png "Default VPC")
<br/>
Default VPC. Source: _docs.aws.amazon.com_

By default, each instance that you launch into a nondefault subnet has a private IPv4 address,
but no public IPv4 address, unless you specifically assign one at launch, or you modify the
subnet's public IP address attribute. These instances can communicate with each other, but
can't access the internet.

Non-Default VPC
<br/>
![Non Default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/images/nondefault-vpc-diagram.png "Non-Default VPC")
<br/>
Non Default VPC. Source: _docs.aws.amazon.com_

On the other hand, to allow an instance in your VPC to initiate outbound connections to the
internet but prevent unsolicited inbound connections from the internet, you can use a
network address translation (NAT) device for IPv4 traffic. NAT maps multiple private IPv4
addresses to a single public IPv4 address. A NAT device has an Elastic IP address and is
connected to the internet through an internet gateway. You can connect an instance in a
private subnet to the internet through the NAT device, which routes traffic from the
instance to the internet gateway, and routes any responses to the instance. For more
information, see [NAT](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat/ "NAT").

You can optionally associate an IPv6 CIDR block with your VPC and assign IPv6 addresses to
your instances. Instances can connect to the internet over IPv6 through an internet gateway.
Alternatively, instances can initiate outbound connections to the internet over IPv6 using an
egress-only internet gateway. For more information, see Egress-Only Internet Gateways. IPv6
traffic is separate from IPv4 traffic; your route tables must include separate routes for
IPv6 traffic.

__CIDR Notation__

Classless Inter-Domain Routing (CIDR) is a method for allocating IP addresses and IP routing.
CIDR notation is a compact representation of an IP address and its associated routing prefix.
The notation is constructed from an IP address, a slash ('/') character, and an integer.

An IPv4 address has 4 parts (e.g `192.168.100.0`), each of which is 8 bits. Hence an
IPv4 address is 32 bits in total. On the other hand, a CIDR block may be represented thus:
`192.168.100.14/24` The integer `24` after the slash `/` tells us that `24` out
of `32` bits of the ip address identify the network, wile `8` out of `32` bits identify the host.
[Click here for more on CIDR](/2020/03/09/CIDR-Blocks)

__Accessing a Corporate or Home Network__

You can optionally connect your VPC to your own data center using an IPsec AWS Site-to-Site
VPN connection, making the AWS Cloud an extension of your data center. A Site-to-Site VPN
connection consists of a virtual private gateway attached to your VPC and a customer gateway
device located in your data center.

__Accessing Services through AWS PrivateLink__

AWS PrivateLink enables you to privately connect your VPC to supported AWS services, services
hosted by other AWS accounts (VPC endpoint services), and supported AWS Marketplace partner
services. You do not require an internet gateway, NAT device, public IP address, AWS Direct
Connect connection, or AWS Site-to-Site VPN connection to communicate with the service. Traffic
between your VPC and the service does not leave the Amazon network.

To use AWS PrivateLink, create an interface VPC endpoint for a service in your VPC. This
creates an elastic network interface in your subnet with a private IP address that serves as
an entry point for traffic destined to the service. For more info see
[VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints/ "VPC Endpoints")

VPC Endpoint Privatelink
<br/>
![VPC Endpoint Privatelink](https://docs.aws.amazon.com/vpc/latest/userguide/images/vpc-endpoint-privatelink-diagram.png "VPC Endpoint Privatelink")
VPC Endpoint Privatelink. Source: _docs.aws.amazon.com_

You can create your own AWS PrivateLink-powered service (endpoint service) and enable other AWS
customers to access your service. For more information, see
[VPC Endpoint Services - AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-service/).

A private link could be used on a global scale, bearing the following
considerations in mind:

- Traffic that is in an Availability Zone, or between Availability Zones in all Regions,
routes over the AWS private global network.

- Traffic that is between Regions always routes over the AWS private global network, except
for China Regions.

- AWS backbone network targets a p99 of a hourly PLR of less than 0.0001%.

### Notes ###

- A virtual private cloud (VPC) is a virtual network dedicated to your AWS account.

- A subnet is a range of IP addresses in your VPC.

- A route table contains a set of rules, called routes, that are used to determine
where network traffic is directed.

- An internet gateway is a VPC component that allows communication between instances
in your VPC and the internet.

- A VPC endpoint enables you to privately connect your VPC to supported AWS services
and VPC endpoint services powered by PrivateLink within the Amazon network.

- Four ways to manage VPCs: Management Console, CLI, SDKs, Query API.

- There's no additional charge for using Amazon VPC. However, there are charges for
using an Site-to-Site VPN connection and using a NAT gateway.

- Each amazon account has a default VPC that has a default subnet in each Availability Zone.

- The default VPC includes an internet gateway, and each default subnet is a public subnet.

- To allow an instance in your VPC to initiate outbound connections to the
internet but prevent unsolicited inbound connections from the internet, you can use a
network address translation (NAT) device for IPv4 traffic.

- NAT maps multiple private IPv4 addresses to a single public IPv4 address.

### References ###

- [How AWS VPC Works](https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works/ "How AWS VPC Works")

- [Medium.com - Difference between Security Group and NACL](https://medium.com/awesome-cloud/aws-difference-between-security-groups-and-network-acls-adc632ea29ae)

- [AWS - Connect to EC2 via HTTP and HTTPS](https://aws.amazon.com/premiumsupport/knowledge-center/connect-http-https-ec2/)

- [CIDR Blocks](/2020/03/09/CIDR-Blocks)

- [Wikipedia - Class Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing "Wikipedia - Class Inter-Domain Routing")
