---
path: "./2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-7_Autoscaling-and-virtual-network-services.md"
date: "2020-03-09T15:50:10"
title: "AWS Certified Solutions Architect Associate - Part 7 - Autoscaling and virtual network services"
description: "Easy to understand tutorial: AWS Certified Solutions Architect Associate - Part 7 - Autoscaling and virtual network services"
lang: "en-us"
---

### Auto Scaling Solutions ###

#### Overview ####

Ability to automatically scale out or in according to our needs at a point in time.

- Monitors apps
- Adjusts capacity

How Auto Scaling Works

![How Auto Scaling Works](https://d1.awsstatic.com/product-marketing/AutoScaling/aws-auto-scaling-how-it-works-diagram.d42779c774d634883bdcd0463de7bd86f6e2231d.png "How Auto Scaling Works")

Scalable Resources

- EC2 Auto Scaling Groups
- Aurora DB clusters
- DynamoDB global secondary indexes
- DynamoDB tables
- Elastic Container Service (ECS)
- Spot fleet requests

Auto Scaling Costs

- Free to use
- Results of use may cost:
  * More instances
  * CloudWatch
  * ELB load balancers

You could limit total amount spendable per hour due to auto scaling.

#### Scaling Groups ####

- A collection of instances with similar characteristics
  * Can be scaled based on criteria like network comms, CPU utilization
  * Unhealthy instances can be automatically replaced. Any state other than running
  is considered unhealthy

Group Considerations

- Time to launch and configure server is required, to be able to scale out
- Relevant metrics to your application
  * CPU utilization
  * Network throughput
  * Free memory
- What AZs should the auto scaling group span. Recommended to have multiple
instances over different AZs.
- Scale to increase or decrease capacity
- Specify the minimum number of instances always running

#### Termination Policies ####

- Select AZ with most instances
- If more than one of above, select AZ with instances with oldest launch config
- if more than one instance with oldest launch config, select instance closest to
the next billing hour

Auto scaling of instances may rely on: Number, Age, Cost

Auto Scaling Termination Policy

![Auto Scaling Termination Policy](https://camo.githubusercontent.com/bb3fffa0e54653fbd38f536637efb4661a601d43/68747470733a2f2f646f63732e6177732e616d617a6f6e2e636f6d2f6175746f7363616c696e672f6563322f7573657267756964652f696d616765732f7465726d696e6174696f6e2d706f6c6963792d64656661756c742d666c6f7763686172742d6469616772616d2e706e67 "Auto Scaling Termination Policy")

Custom Termination Policies

- OldestInstance - likely to need overhaul
- NewestInstance
- OldestLaunchConfigurations
- ClosestToNextInstanceHour - likely to reduce cost
- Default

#### Auto Scaling Configuration ####

- Configuration for how instances added to the auto scaling group will be initiated
- When Creating and instance, you could choose to create more than one and launch
them in an auto scaling group
- A launch configuration is needed to create an auto scaling group. It can be
created before or during the creation of the auto scaling group.
- The launch configuration contains the instance type, key pair, security groups etc

Steps to create an auto scaling group

- Management Console -> EC2
- Auto scaling on the left -> Auto scaling groups -> Create auto scaling group
- Create or select a launch template
- AMI. You could create a new AMI or from an existing instance.
Create Launch Configuration
- Name
- Request spot instances
- IAM role - Role you want associated with this instance
- CloudWatch
- Advanced details
  * Kernel ID
  * RAM disk ID
  * Bootstrap - linux bash scripts for startup
- Assign Elastic IPs (EIP) for any instances you want to be publicly accessible
over internet
- If using load balancer then no need to worry about EIPs.
- Add security group to enable e.g HTTP, SSH, SMTP etc
- Review
- New or existing key pair
- Click create launch configuration
Create Auto Scaling Group
- Minimum instances in the auto scaling group
- Network
- Subnet - Choose all/multiple subnets in your VPC to allow autoscaling across
multiple or all AZs -> High availabiity.
- Advanced details, Enable load balancing etc.
- Select use scaling policies to adjust capacities
  * Metric - CPU usage
  * Value - 80 -> 80%
- Review
- Create

#### Launch Methods ####

- Launch template, prerequisites

  * The template must include all the required parameters need to launch an EC2
  instance otherwise error: You need to use a fully formed launch template
  * IAM user/role must have permission for EC2:run instances permission

- Launch config

- Existing EC2 instance

- EC2 launch wizard. When you create an EC2 instance, there is an option
to create more than one instance with a link to put that instance in an auto
scaling group

#### Load Balancer Concepts ####

Load balancer sits between the requesters and servicers (which respond to requests)
Load balancer takes the incoming request and decide which servicer will handle.

Load Balancing Categories
- Sender initiated. Sender locates the best target e.g DNS load balancing. DNS sends names of
multiple names and the client selects the best target.
- Receiver initiated. Receiver selects best target.

Classes of Load Balancing
- Static Load Balancing used by multi-tier application
- Dynamic load balancing - true load balancing
  * Actions dynamically assigned
  * Scalability is provided by this load balancing model

Elastic Load Balancer (ELB) uses dynamic load balancing.

Load Balancing Algorithms

- Round robin - serial allocation of requests to servicing nodes
- Randomized
- Centrally managed - Decisions made based on available info on the environment
- Theshold based - Send request to on servicing node up to a certain amount
before switching to the next servicing node

Remember all nodes need to have same data
- replication
- storage arean network

#### Elastic Load Balancing (ELB) ####

ELB Benefits
- Highly available
- Secure (Security handled by AWS)
- Flexible
- Monitoring and auditing included
- Elastic
- Hybrid - Multiple types of load balancing could be associated

ELB Types

- Classic Load Balancer. Not recommeded for any newly deployed instances
- Application Load Balancer - Web app, HTTP level (OSI model layer 7)
- Network Layer - TCP level (OSI model layer 4)

Supported Services

- EC2
- ECS
- Auto Scaling
- CloudWatch
- Route53

Read Amazon AWS Documentation

Elastic Load Balancing Features -> Product Comparison

### Virtual Network Services

#### Domain Name System (DNS)  ####

Domain Name System (DNS) - Name, location service for IP networks. DNS helps
resolve human friendly names to actual IP address.

- RFC 1034 and 1035
- Provides name to IP address mapping

Fully Qualified Domain Name (FQDN)

- Format: <host>.<management/organization domain>.<top level domain>
- Example: aws.amazon.com
- Domain = <organization domain>.<top level domain> = amazon.com
- Example with sub domain: services.aws.amazon.com



Requestor   -> root DNS server -> top level domain DNS Server -> Specific DNS Server
Web browser -> root DNS serve  -> com server                  -> AWS DNS Server

Requestor could be device, server, DNS server etc

DNS Hosting
- Provide name resolution with:
  * Caching
  * Recursion - recursively looksup till arriving at the required IP address
- Stores DNS database which supports aliasing
- Provides DNS zone transfers
  * Offloads name resolution processing

DNS Resolution
- Requests IP address of a host name (forward lookup)
- Request host name of an IP address (reverse lookup)
- Requestor is configured with a DNS server address for example Google public
DNS servers 8.8.8.8 and 8.8.4.4

DNS Records
- A and AAAA
  * Used to resolve hostname to IP address (A for IPv4, AAAA for IPv6)
- NS records (Name server records)
  * Domain to hostname resolution
- MX record - Mail record
- CNAME record
  * Alias for real name (canonical)
- SRV record - service records

#### Configuring DNS ####

Customize IAM login link

- Management Console -> IAM
- At the top you see the login link
- Click customize to give it an alias
- Alias must be globally unique

Each VPC as a DNS configuration set. AWS generates a host name and public IP
address for EC2 instances etc. The host name cannot be changed but another host
name could be associated with the public ip address using Route53 or other
DNS Services.

#### Configuring Route 53 ####

- Managed (you don't have to create an instance) Service within AWS
- Management Console -> Networking & Content Delivery -> Route53
- DNS Management
- Hosted Zone (Info on traffic routing) -> Create Hosted Zone
  * Hosted Zone is like an internal private DNS
  * We create a hosted zone for routing traffic within our VPC.
- Specify a domain name.
  * Domain names are resoloved within the VPC. If the domain names you specify already
  exists publicly (internet) then the conflict may prevent users of your VPC
  from browsing to the already existing public domain.
- Specify your VPC
- Click create
- After creation it shows you (SOA) the dns server under which you are and also
.com, .org and .co.uk servers.
- You can mirror another internet network into yours by importing their zone and
using the same subnets and ip addresses.

Routing Example using 2 IP addresses and clustered load balancing techniques

- Management Console -> Networking & Content Delivery -> Route53
- Create a Record
- Select Type: A - Ipv4
- Values -> Enter 2 ip addresses
- Routing Policies
  * Simple traditional DNS
  * Weighted - Go to one IP 80% and another IP 20% of the time. Useful when
  deploying a new version of an app etc
  * Latency - Send users to the destination with the lowest latency
  * Failover - Use one ip address until it fails then use the second
  * Geo location - Get the location of the user and forward to closest server
  * Multi value answer -

Health Check
- Protocol
- IP address
- Host name
- port
- pat

#### Configuring ACLs and NACLs ####

ACL

- Management Console -> s3
- Click on a bucket
- Click on permissions

NCL

- Management Console -> VPC
- Click on Network ACL

Create Network ACL
- Name
- VPC
- Create

After creating the ACL
- Set inbound rules.
  * Rules are processed for the top of the list and once a match applys stops.
- Set outbound rules
  * Rules are processed for the top of the list and once a match applys stops.
  * Default denys all access by default.
  * Add a rule

Working with ACL/rules at the subnet level

Mehod A

- Select an existing ACL
- Select subnet associations
- Click on edit
- Select the subnet(s) you want to apply the ACL to
- Click save

Method B

- Go to subnets
- Click a subnet
- At the bottom portion is the network ACL
- You can associate subnets via this section as well

- ACLs are legacy, whereas security policies are recommended

_Read: CIDR notation_

#### Flow Logs ####

Capture information about (metadata) traffic moving in the environment. Called
netflow in localized environment.

Flow logs store the logs in the CloudWatch service thus incur cost.

Get S3 bucket ARN

- Management Console -> S3
- Create an S3 bucket
- Give it a name relating to flow logs
- Click on the S3 bucket
- Under properties click 'Copy bucket ARN'

Setup flowlog for Network Interface
- Management Console -> EC2 -> Network Interfaces
- Choose a Network Interface
- Select the flowlog tab -> Create flow log
- Filter = all
- S3 Bucket ARN
- Create

Setup flowlog for VPC
- Management Console -> Networking & Content Delivery -> VPC
- Choose a VPC

Setup flowlog for Subnet

### Notes ###

- You could limit total amount spendable per hour due to auto scaling.
- If using load balancer then no need for Elastic IPs ???
- Choose all/multiple subnets in your VPC to allow autoscaling across multiple
or all AZs to achieve high availabiity. For high performance leave single AZ.
- Classic Load Balancer. Not recommeded for any newly deployed instances
- Route 53 services include DNS registration, resolution & management as well as
health checks.
- DNS are configured at the VPC level
- Domain names registered outside AWS will have to be directed to the Route53
service
- NACLs are applied to VPCs
- ACLs are legacy, whereas security policies are recommended
- ACL rules are processed for the top of the list and once a match applys stops.
- ACL can be configured from the management console or from CLI
- S3 buckets, VPCs and subnets can use ACLs
- Flow logs store the logs in the CloudWatch service thus incur cost.

### Acronyms ###

- ECS - Elastic Container Service
- ELB - Elastic Load Balancing
- DNS - Domain Name System
- FQDN - Fully Qualified Domain Name
- TLD - Top Level Domain
- ACL - Access Control Lists
- NACL - Network Access Control Lists
- ACLs are legacy, whereas security policies are recommended
