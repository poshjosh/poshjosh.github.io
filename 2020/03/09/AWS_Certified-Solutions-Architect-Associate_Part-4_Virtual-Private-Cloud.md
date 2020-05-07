---
path: "./2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-4_Virtual-Private-Cloud.md"
date: "2020-03-09T11:20:00"
title: "AWS Certified Solutions Architect Associate - Part 4 - Virtual Private Cloud"
description: "Easy to understand tutorial: AWS Certified Solutions Architect Associate - Part 4 - Virtual Private Cloud"
lang: "en-us"
---

### Virtual Private Could (VPC) ###

- Virtual Private Could (VPC) - Not Microsoft Virtual PC (VPC)
- The privacy is virtual. It is actually a public cloud with security.
- There is a default VPC, one in each region.
- Amazon recommends never deleting the default VPC
- Personal data center in the cloud
- VPN connections can be made to the VPC.
- Apps run in the VPC or on-premises
- Subnets could be created in the VPC
  * Public subnets - public facing
  * Private subnets -
  E.g web server on the public side needs secure connection to the database on the private side.
- Direct Connect can provide VPN connection. The connection could be between VPCs or between a local networks and VPCs.
- Multiple VPCs can be interconnected, using VPC peering.
- Endpoints could be created within the VPC to connect to Amazon resources
- Endpoints a controlled by policies
- VPC is about instances and EBS volumes - So we use endpoints will policies to connect to resources Amazon outside the VPC like S3 buckets, Glacier store.
- VPC features:
  * Dynamic private IP
  * Dynamic public IP
  * AWS-provisioned DNS names (long, cryptic - many prefer to use own DNS names which may be public/private)
  * Private DNS name
  * Public DNS name

#### Creating VPCs ####

- Management Console -> Networking & Content Delivery -> VPC Console
- Click the Create VPC button which is at the top
- Name
- IPv4 Classless Interdomain Routing (CIDR) block (To learn more, search for private IP address/IP address spaces on the internet)
- Tenancy
  * Default (shared tenancy)
  * Dedicated - Not shared, more expensive

VPCs cannot directly talk to each other. Communications between VPCs must be facilitated.

Create a subnet for the VPC

- In the VPC console, Click on Subnets -> Create new subnet
  * Name
  * Select a VPC for the subnet
  * VPC CIDR
  * Availability Zones
  * IPv4 CIDR block (this is for the subnet)

Must Read - AWS Direct Connect

Go to Amazon Web Services Documentation and search for What is AWS Direct Connect.

#### Configuring a DHCP options ####

Any device that receives configuration from DHCP gets an IP address, subnet mask, default gateway and when defined, the DHCP options.

- A DHCP option is a configuration parameter relating to the IP address protocol
(the protocol that we use to get ip addresses and the rest of our configuration)
on the device that receives that configuration set.

- Range of addresses is defined during subnet creation

- DHCP is used to provide dynamic IP addresses

VPC console -> Click a VPC -> DHCP Options
- Name
- Domain name - e.g sales.mycompany.something
- DNS - (You could use 8.8.8.8 -> google public DNS)
- NTP Server - time server ip address
- NetBIOS name servers - Optional, not used as much these days
- NetBIOS node type - Optional, not used as much these days

You could have multiple DHCP Options set if you have multiple subnets

### Elastic IP addresses ###

Public IP addresses, routable on the internet, are created via Elastic IP (EIP)
addresses. EIPs are IP addresses within your VPC associated with instances.

- Elastic IP (EIP) - A public IP address from within the VPC region.

- Amazon has huge blocks of IP addresses from all over the world. Amazon allocates
an EIP to your instances. They are elastic because they can change or moved from
one instance to another.

- Permanently allocated to your account until released.

- Account is charged until the IP is released.

- Managing your EIPs is very important to keep your cost down.

- EIPs associated with Elastic Network Interface, the network interface is the
point where the public IP address could be used.

- EIPs can be moved between instances within the same Region only, because VPCs
are region bound.

#### Create Elastic IP ####

VPC Management Console -> Elastic IPs -> Allocate new address

Note when you browse to VPC Console -> Elastic IPs, if you see that you have EIPs
but you do not have any public IP address on any of instances, then you are
spending money for nothing.

When you don't need an elastic IP, release it to avoid costs. To release an EIP:

- Browse to: VPC Management Console -> Elastic IPs
- Select an EIPs
- Click on actions -> Release address


### Elastic Network Interfaces (ENIs) ###

An Elastic Network Interface is basically

- Virtual network interface (like a network card/adaptor) attached to an EC2 instance.
- Only available within a VPC
- Each ENI is associated with a subnet within the VPC just as a physical network
interface would be associated with a subnet within a local network

Benefits

- Allows dual-homing (Multiple ENIs attached to a single EC2 instance allows
for dual-homing)
- One public address and multiple private addresses associated to the ENI

Once you associate an ENI with an instance you can then give it a public or private
IP address. Now you have your instance connected to a new network based on that ENI.

### Endpoints ###

- A connection that allows VPCs to access ASW services or another VPC.
- With endpoints, services are specified based on region and service name.
- Can enforce policies on different endpoints.

#### Creating an Endpoint ####

Steps

- Specify the Amazon VPC
- Specify the sevice: com.amazonaws.<region>.<service>
- Specify the policy
- Specify the route tables

Process

- VPC Management Console -> Endpoints -> Create Endpoint
- Choose a service category e.g choosing S3, creates a bridge to an S3 service
- Choose the VPC
- Select the provided route table
- Select the provided policy which by default is full access.

### VPC Peering ###

VPC Peering  connects one VPC to another e.g development VPC connected to production VPC
VPC Peering is not transitive. If VPC1 peered with VPC2 and VPC2 is peered with
VPC3, then VPC1 is not peered with VPC3

#### A Note VPC Peering ####

- Initiating VPC sents a request to the receiving VPC
  * Owner role required, not admin or other user
  * IP CIDR blocks in each VPC to be peered must not overlap
Never use the same CIDR block (ip subnet) across different VPC
- Receivin VPC accepts the request
  * Owner role required, not admin or other user
- Each VPC needs a defined route to the other VPC
  * May require routing table modifiations
- Security group rules
  * May require modifications for the VPC peers

#### Create VPC Peer ####

Requester- Create a VPC peer in the originating VPC
- Browse to VPC Dashboard
- Ensure you have at least 2 VPC each with non - overlapping (CIDR block/subnet)
- In VPC Dashboard -> Peering Connections -> Create Peering Connection
- Specify the requestor VPC
- Specify the CIDR
- Select another VPC to peer with (Same account/another, Same region/other etc)
- Specify the VPC accepter.
- Continue to create the peering connection.

Acceptor: Access the VPC request in the target VPC to create the peer connection

- After creating the connection, the acceptor has to accept.
- Acceptor. Go to actions -> Accept request

Both Requester and Accepter - Define Routes

- Route tables by default to self, so add route to other VPC
- Click on your VPC -> Route -> Edit -> Add route rule.
- Add another route rule to the other VPC
- Destination implies IP subnet e.g 10.10.0.0/16
- Target implies peering connection e.g the one created
- Click in the text box under target to see available peering connections.
- Select the target peering connection.

### VPC Security ###

#### Security Groups ####

A security group:

- Acts like a firewall
- Assigned to an instance within a VPC not subnet
- Defines allowed traffic flows (ingress/incoming and egress/outgoin)
- Supports only allow rules - deny is implicit (Closed to open system. Starts out being closed)
- Stateful processing is used

Network Access Control Lists (NACL)

- Applied on the subnets
- Stateless processing
- Supports both allow and deny rules
- Rule number defines prcedence
  * Lowest numbered rules evaluated first
  * First match applies

#### Network Address Translation (NAT) ####

Allows you use IP addresses on your network that are not routable on the internet
and still be able to connect to the internet.

- NAT translates between private IP addresses and public IP addresses

- Private addresses start with:

  * 10
  * 172.16-31
  * 192.168

- With NAT, devices that only have private addresses internally, can still
connect to the internet when they need to.

2 Ways to Implement NAT

- NAT instances

  * NAT implemented on a private and public subnet.
  * EIP associated with NAF instance
  * Instances in the private subnet connect through the NAF instance.

- NAT gateways

  * Works more like traditional NAT servers/appliances

How to Create a NAT Instance

- Source of AMI images

  * Own AMI  
  * Marketplace
  * Community
  * Free tier only  

- Choose free tier only -> community
- Search for NAT
- Select an image which supports NAT and launch the instance
- Create EIP
- Create ENI
- Connect the ENI to the launched instance
- Assign the EIP to the ENI
- Thats what gives you public facing IP
- Then have another network card connected to the local subnet internet within
the VPC

How to Create NAT Gateway

- Make sure you have an EIP first
- Management Console -> Services -> VPC -> NAT Gateways
  * Select the subnet
  * Select the EIP (Which is public/routable on the internet)
- Add a route that goes to:
  * Destination: 0.0.0.0/0 - This means anything not defined in the route table.
  * Target: the id of the NAT Gateway you just created.

#### Gateways (VPGs and CGWs) ####

We use a VPN to connect local network to VPN

A VPN is a public network that is encrypted

Virtual Private Gateway (VPG) - on the AWS side

- Connects your local networks to the VPC.
- Is the VPN concentrator inside the VPC.
  You could have 2 or more connections concentrated into that VPG

Customer Gateway (CGW) - on the customer side  

- Physical device or software application
- Connects to the VPG

A VPG together with a CGW forms a VPN

Alternative Connections

- AWS hardware VPN
- AWS Direct Connect
- VPN CloudHub
- Software VPN (As long as the protocol used is supported by AWS)

How to Create a VPN

- You also need to have a configured CGW
- AWS Management Console -> Networking & Content Delivery -> VPC
- Select Customer Gateway on the left -> Create Customer Gateway
- Enter the IP address of the customer gateway which you must have already configured
on-premises at you location.

- You need to have a configured VPG
- AWS Management Console -> Networking & Content Delivery -> VPC
- Select Virtual Private Gateway on the left -> Create Virtual Private Gateway
- Choose the Amazon default ASN

- Create the VPN
- AWS Management Console -> Networking & Content Delivery -> VPC
- Select VPN Connections on the left -> Create VPN Connection
- Enter the CGW and VPN earlier created.

#### Amazon Virtual Private Cloud illustration 1 ####

![Amazon Virtual Private Cloud](https://www.iteanz.com/tutorials/wp-content/uploads/2017/12/aws10.png "Amazon Virtual Private Cloud")

#### Amazon Virtual Private Cloud illustration 2 ####

![Amazon Virtual Private Cloud](https://www.iotone.com/files/software/amazon-virtual-private-cloud--vpc-_1.png "Amazon Virtual Private Cloud")

### Notes ###

- VPC peering is not transitive
- Never use the same CIDR block (ip subnet) across different VPC
- Security groups are not for user management
- Security groups are applied at the instance level not subnet level
- NAT is used to interconnect Point private networks and public networks
- An EIP is associated with the NAT instance for the public facing side
- Instances in the private subnet of a VPC use the NAT to connect to internet
- A VPG together with a CGW forms a VPN
- Gateways are used to connect to VPCs from local networks
- Gateways are effectively VPN endpoints

### Acronyms ###

- VPC - Virtual Private Cloud
- VPN - Virtual Private Connection
- CIDR - Classless Inter-domain Routing
- DHCP - Dynamic Host Configuration Protocol
- DNS - Domain Name Server
- NTP Server - Network Time Protocol Server
- EIP - Elastic Internet Protocol (address)
- ENI - Elastic Network Interface
- NACL - Network Access Control List
- NAT - Network Address Translation
- VPG - Virtual Private Gateway
- CGW - Customer Gateway
