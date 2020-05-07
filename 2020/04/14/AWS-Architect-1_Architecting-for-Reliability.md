---
path: "./2020/04/14/AWS-Architect-1_Architecting-for-Reliability.md"
date: "2020-04-14T08:15:00"
title: "AWS Achitect 1 - Architecting for Reliability"
description: "Game changing tutorial on AWS - Architecting for Reliability"
tags: ["AWS", "tutorial", "architect", "reliability", "availability", "fault tolerance", "game changing", "certified solutions architect associate", "must read", "core concepts", "Identity and Access Management (IAM)", "CloudTrail", "Web Application AWS Filter", "AWS shield", "AWS Config", "Trusted Advisor", "vertical scaling", "horizontal scaling", "loose coupling", "CloudFormation", "Availability Zone", "Elastic  Load Balancer", "Network Address Translation (NAT)", "Relational Database Service (RDS)", "Simple Storage Service (S3)"]
lang: "en-us"
---

### Acronyms ###

- AZ - Availability Zone
- CDN - Content Delivery Network
- DDoS - Distributed Denial of Service
- ELB - Elastic Load Balancer.
- EFS - Elastic File System
- IAM - Identity and Access Management
- MFA - Multi Factor Authentication
- NAT - Network Address Translation
- NFS - Network File System
- RDS - Relational Database Service
- S3 - Simple Storage Service.
- WAF - Web Application Filter

### Before we set off ###

__About this Artcile__

- Hi, this article contains game changing information for those who would like
to get AWS Certified Cloud Architect certified.

- It is one of a series of articles.

- This is the first article in the series.

- It is an estimated 35 minute read.

__This Article is not an Introduction to AWS__

This series of articles is not an introduction to AWS or any of the core
concepts of the AWS Cloud. You need to be already familiar with some core
concepts of AWS Cloud to fully benefit from this article. In order words,
if you are not familiar with the AWS cloud, you should read the series of
articles beginning at:

- [Notes on Amazon Web Services - 1](/2020/03/02/Notes-on-Amazon-Web-Services_1_Introduction)

- [AWS Certified Solutions Architect Associate - Part 1](/2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-1_Key-services-relating-to-the-Exam)

### What does it mean to architect for reliability? ###

First off, what is reliability?

Reliability
: is the quality of being trustworthy or of performing consistently well

So we want our cloud based application to perform consistently well. Well for
AWS, reliability is one of the [5 pillars of a well architected system](https://aws.amazon.com/blogs/apn/the-5-pillars-of-the-aws-well-architected-framework/)
and it includes the ability of a system to:

- Recover from infrastructure or service disruptions.

- Dynamically acquire computing resources to meet demand.

- Mitigate disruptions such as misconfigurations or transient network issues.

### TL;DR ###

Read the `Takeaways` at the end of the article.

### Core Concepts ###

Before delving into reliability proper. Let's take a brief look at some AWS
core concepts. This include: Identity and Access Management (IAM), CloudTrail, AWS Web Application Firewall (WAF), AWS Shield, AWS Config and AWS
Trusted Advisor.

__Identity and Access Management (IAM)__

- Principals (users, apps etc) authenticate via:

  * Username and password
  * Programmatic keys

- Authentication- Who you are

- Authorization - What you can do

- __IAM Users__

> An IAM user is an entity that you create in AWS. The IAM user represents the
> person or service who uses the IAM user to interact with AWS. A primary use for
> IAM users is to give people the ability to sign in to the AWS Management Console
> for interactive tasks and to make programmatic requests to AWS services using
> the API or CLI.

- __IAM Groups__

> An IAM group is a collection of IAM users. You can use groups to specify
> permissions for a collection of users, which can make those permissions easier
> to manage for those users. For example, you could have a group called Admins
> and give that group the types of permissions that administrators typically need.
> Note that a group is not truly an identity because it cannot be identified as a
> Principal in a resource-based or trust policy. It is only a way to attach
> policies to multiple users at one time.

- __IAM Roles__

> An IAM role is very similar to a user, in that it is an identity with
> permission policies that determine what the identity can and cannot do in AWS.
> However, a role does not have any credentials (password or access keys)
> associated with it. Instead of being uniquely associated with one person, a
> role is intended to be assumable by anyone who needs it.

- __Temporary Credentials__

> Temporary credentials are primarily used with IAM roles, but there are also
> other uses. You can request temporary credentials that have a more restricted
> set of permissions than your standard IAM user. A benefit of temporary
> credentials is that they expire automatically after a set period of time.
> You have control over the duration that the credentials are valid.

- Prefer IAM roles over users. Generally:

  * Use `users` for single user access when very specific access is required.

  * Use `roles` for applications, multiple users, federated access. For more on
  federated access [click here](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc.html)
  and [here](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html)

- Do not carryout day to day administration from your root account. Rather
use an IAM account.

- While under IAM console, check your security status to see how much of the
security best practice you have completed. Complete all the security best
practice requirements.

__CloudTrail__

With CloudTrail, you can log, continuously monitor, and retain account
activity related to actions across your AWS infrastructure. CloudTrail
provides event history of your AWS account activity, including actions
taken through the AWS Management Console, AWS SDKs, command line tools,
and other AWS services.

This enables governance, compliance, operational auditing, and risk auditing
of your AWS account. It also simplifies security analysis, resource change tracking, operational analysis and troubleshooting.

__AWS Web Application Filter and AWS Shield__

- `AWS Web Application Filter (WAF)` is a web application firewall that lets
you monitor the HTTP and HTTPS requests that are forwarded to an Amazon API
Gateway API, Amazon CloudFront or an Application Load Balancer.

- AWS WAF lets you control access to your content, based on conditions that
you specify, such as the IP addresses that requests originate from or the
values of query strings

- `AWS Shield` helps protect our AWS based accounts and applications from Distributed Denial of Service attacks.

- AWS Shield is enabled by default.

- AWS Shield Advanced provides expanded DDoS attack protection and includes additional benefits like cost protection from additional costs resulting
from a DDoS attack.

__AWS Config__

- AWS Config can evaluate the settings/configurations we have in our account
and let us know if there is an issue.

- With AWS Config, you are able to continuously monitor and record
configuration changes of your AWS resources.

- AWS Config provides you with the ability to define rules for provisioning
and configuring AWS resources. Resource configurations or configuration changes
that deviate from your rules automatically trigger Amazon Simple Notification
Service (SNS) notifications and Amazon CloudWatch events so that you can be
alerted on a continuous basis.

- You could set up a rule with parameters: `Key=InstanceType and Value=t2.micro`
Then, whenever any of your instance type changes from `t2.micro` AWS Config
automatically triggers Amazon Simple Notification Service (SNS) notifications
and Amazon CloudWatch events

- You could build custom rules with AWS Config.

> AWS Config is a service that enables you to assess, audit, and evaluate the
> configurations of your AWS resources. Config continuously monitors and records
> your AWS resource configurations and allows you to automate the evaluation of
> recorded configurations against desired configurations. With Config, you can
> review changes in configurations and relationships between AWS resources, dive
> into detailed resource configuration histories, and determine your overall
> compliance against the configurations specified in your internal guidelines.
> This enables you to simplify compliance auditing, security analysis, change
> management, and operational troubleshooting.

__AWS Trusted Advisor__

- AWS Trusted Advisor helps optimize costs, performance, security, fault
tolerance and service limits.

- Example of service limit = `5 ip addresses per region`

- Check trusted advisor from time to time.

### Architecting for Availability and Fault Tolerance ###

__Regions and Availability Zones__

Amazon EC2 is hosted in multiple locations world-wide. These locations are
composed of Regions, Availability Zones, and Local Zones. Resources aren't
replicated across Regions unless you specifically choose to do so.

- Each __Region__ is a separate geographic area.

- Each Region has multiple, isolated locations known as __Availability Zones__.

- [Click here for more on Regions and AZs](/2020/03/17/AWS_Regions-Availability-Zones-and-Local-Zones)

- For high availability it makes sense to put resources in different availability
zones (AZs)

- Note that the EC2 instances, key-pairs, security groups etc you create are
limited to the region you created them in.

__Eliminating Single Points of Failure__

- Add redundancy using multiple AZs. This way if an AZ (a physical data center)
goes offline.

- Load balancers can span multiple AZs.

- Latency across AZs is very low.

- Also make your database multi - AZ redundant.

- Setting up multi - AZ databases is quite elaborate.

- AWS Relational Database Service (RDS) is a completely managed service which
makes it easy to achieve multi - AZ redundancy.

- Use AWS RDS.

__Vertical and Horizontal Scaling__

- To scale vertically, you need to stop the server before scaling its capacity
up or down.

- Auto - scaling (horizontally) is supported by AWS

- Use auto - scaling.

- Automating auto - scaling for EC2 instances is quite laborious. To mitigate
this scale services rather than servers.

__scale services rather than servers__

Several managed services in AWS help us build application architectures with
high availability. Each of these services still run on virtual machines behind
the scene but AWS manages them for you.

- Amazon RDS - Relational database service.

- Amazon S3 - Simple Storage Service.

- Dynamo DB - NoSQL database service.

- ELB - Elastic Load Balancing.

AWS also has managed services which enable you build applications without
servers (serverless applications)

- Lambda - Write a function and amazon will run that function.

- API Gateways

- Simple Storage Service

- CloudFront

For example, application backend API actions could be executed as lamda
functions. This could be achieve through Amazon API gateway. On the other
hand, application front end content could be hosted on Amazon S3 which
supports static website hosting. CloudFront is a content delivery network
which works with S3.

When planning out application architecture. Consider using managed services

__Implementing Loose Coupling__

Tight coupling -> Every component of the system is tightly coupled such that
when one fails other coupled components fail.

Example of tight coupling:

```
Web Server -> Database Server
```

By using queues we can implement loose coupling.

```
Web Server -> Queue -> Lamda Function -> Database Server
```

By using [microservices](https://microservices.io/) architecture we can
implement loose coupling.

_Eample of Microservices architecture_
<br/>
![Microservices Architecture](https://microservices.io/i/Microservice_Architecture.png)
<br/>
_Eample of Microservices architecture. Source: microservices.io_

Each component in a microservices architecture is a microservice and can
enjoy custom scaling, security, technologies etc

### Architecting Reliable Virtual Networks ###

__Overview of Amazon VPC__

- An `internet gateway` is like a virtual edge router for the VPC.

- You only need to create one internet gateway for you entire VPC and it could
scan multiple AZs.

- We can add routes from public subnets to the internet gateways to allow
access to the internet.

- `NAT gateway` is attached to the public subnet and used to route all internet
traffic from private subnets to the internet gateway.

- Two routes, one for our app server, the other for the database server

  * Route 1
  ```
  app (in private subnet) -> NAT gateway (in public subnet) -> Internet Gateway (attached to VPC) -> internet
  ```

  * Route 2
  ```
  database (in private subnet) -> NAT gateway (in public subnet) -> Internet Gateway (attached to VPC) -> internet
  ```
- We replicate the above 2 routes in another AZ.

- Note that the internet gateway serves both AZs.

- We add a load balancer to sit in front of the internet gateway i.e between
the internet and the internet gateway.

- We add an application load balancer to sit between the internet gateway
and all nodes of the application.

__Creating a Custom VPC with Public and Private Subnets__

- VPC Dashboard -> Click on `Start VPC Wizard` button

- Select `VPC with Public and Private Subnets`.

- You have the option of using a NAT instance instead of a NAT gateway.

- __NAT instance vs NAT gateway__. NAT gateway is NAT as a service. This
means it is managed by AWS. When you NAT instance you have to manage the
EC2 instance for the NAT.

- Allocate an elastic IP address to the NAT gateway. You can create one via
another browser tab if you don't have one.

- The wizard will only build things in one AZ.

- The wizard, amongst other actions it performed, helps:

  * Create the internet gateway.

  * Set up the NAT gateway including a route from the public subnet to the
  NAT gateway.

  * Attach an elastic IP to the NAT gateway resource.

  * Build a private subnet.

  * Route from private subnet to NAT gateway.

  * Route from the app to the NAT gateway resource.

- VPC Console -> Route Tables -> Click a specific route tables for details

- Each NAT gateway will have its own elastic IP address.

__Adding Public and Private Subnets to an Existing VPC__

- You can create a subnet by:

  * VPC Console -> Subnets -> Click the `Create Subnet` button

- Edit rout table associations to sure:

  * Your public subnets are associated with the internet gateway resource
  identified by prefix `igw-`.

  * Your private subnet are associated with the nat gateway resource
  identified by prefix `nat-`

- You can edit route table associations by:

  * VPC Console -> Subnets
  * Click the `Route Table` Tab
  * Click the `Edit route table association` button

__Validating Outbound Internet Access through NAT Gateways__

To validate outbound internet access requires an EC2 instance. We could
however, do this without an EC2 instance using the Run Command service

```
AWS Management Console -> EC2 -> Run Command
```

- To use `Run Command`, set up an IAM role to allow EC2 instances to use the
`Run Command` feature.

- Create a Role for use with the `Run Command`

- Attach policy `AmazonEC2RoleforSSM` to the role. To find the policy search
for `ssm`

- Launch an EC2 instance into the VPC associated with the NAT gateway we want
to test. Make sure you select the private subnet you want to test for outbound
internet access.  

- Use an amazon linux AMI for the EC2 instance as it contains an agent required
by `Run Command`

- Use the `Run Command` to `ping` google ip address.

__Implementing a Load Balancer in a VPC__

- Application load balancer sits between the internet gateway and application
instances.

- Build security groups one for the load balancer and another for EC2 instances.

- Application load balancer security group should be public
Accept TCP port 80/43 for 0.0.0.0/0 (everyone)

- ACL for both load balancer and instances permit all traffic to flow between
any of the subnets.

- To create a load balancer:

  * EC2 Console -> Load Balancing -> Click on `Create Load Balancer` button
  * Add listeners for HTTP/80 and HTTPS/443 as required
  * Select AZs
  * Select the public subnet for each AZ
  * Select the security group
  * Configure routing
    - Select `New target group`
    - Enter a name for the new target group
    - You can route to an instance or IP address. Choose instance in this case.
  * Add your instances to the newly created target group.
    - select the instances and click the `Add to registered` button

- Your load balancer has a DNS name which you can access from a browser.

- You could use run command to create html content to and index.html document.
```
echo "<h1>Hello World</h1>" > /var/www/html/index.html
```

After running the above command on your instances, browse to your load
balancer's DNS name and you should see `Hello World`

Under Load Balancing - Target Groups, set the path for your health checks
for the load balancer to point to `/index.html`

__Hybrid Networking with Amazon VPC__

- __VPN Gateway__ allows you define a VPN tunnel to a customer gateway
You define a customer gateway which is usually an ip sec capable device eg.
cisco router etc

> __AWS Direct Connect__ is a cloud service solution that makes it easy to
> establish a   dedicated network connection from your premises to AWS. Using
> AWS Direct Connect, you   can establish private connectivity between AWS and
> your datacenter, office, or  colocation environment, which in many cases can
> reduce your network costs, increase  bandwidth throughput, and provide a
> more consistent network experience than Internet  based connections.

### Architecting a Multi Tier Application ###

__Configure Security Groups for the App and Data tiers__

__Build the Data Tier with Amazon RDS__

__Implement Shared Storage Using Amazon EFS__

In order to share storage between multiple EC2 instances. Use the Amazon
Elastic File System (EFS) service to create an Network File System (NFS) share
as a service and then we can mount that share from any EC2 instance.

To use EFS, first create a security group add a rule for:

- Type: `All traffic`
- Protocol: `All`
- Port Range: `0 - 65535`
- Source - `custom` -> `internalEFS`

Note EC2 instances and EFS are going to be part of this security group

To create an EFS:

- AWS Console -> Storage -> EFS Service
- Click on the `Create File System` button
- Select VPC, AZ, subnet, and security group you just created.

The EFS has a DNS name.

A link is provided for `Amazon EC2 mount instructions`

__Load Balance the App Tier on EC2 Instances__

__Domain and DNS Registration with Route 53__

__Configure SSL on the Load Balancer__

### Minimizing Risk with Deployment Automation ###

__Automate Deployments with Auto Scaling Groups__

When using auto scaling groups in the real world, data from existing
application is often used to determine how best to scale.

__Scale Dynamically Based on Resource Utilization__

__Validate Automatic Self Healing Servers__

__Automatically Recover a Failed Availability Zone__

__Automate Infrastructure with CloudFormation__

CloudFormation uses `json` or `yaml` based templates.

AWS has reference architectures for using CloudFormation to build various
types of applications.

- [GitHub - AWS Sample Reference Architectures](https://github.com/search?q=org%3Aaws-samples+refarch)

- [GitHub - AWS CloudFormation Reference Architectures](https://github.com/aws-samples/ecs-refarch-cloudformation)

You could use the existing templates and customize it as needed.

__Common Automated Deployment Patterns__

- Blue Green Deployment

  * Usually we have a production VPC and also a development VPC

  * Once the development VPC is good to go, we switch the DNS record to point
  to the development VPC. If things don't go as planned we can roll back to the
  previous production VPC.

- Canary Deployment.

  * We route some traffic (e.g 30%) to a VPC containing our updated application
  to see how that performs.

### Architecting Multi - region Solutions ###

__Pilot Light Architecture__

- Pick a secondary region and setup the minimum amount of infrastructure needed
to power up our application.

- We leave the servers powered off to limit costs to a minimum.

- Create a read replica in the secondary region.

- If the existing region goes down you could:

  * Change the DNS record for your domain name so it resolves to the standby

  * Scale up services if not using an auto scaling group.

  * Power on the servers.

  * Promote the read replica to a master database if

__Warm Standby Architecture__

- `Warm standby` is like `pilot light` with the exception that the servers are
powered on in `warm standby`.

__Active/Active Multi-region Architecture__

- In this architecture, we use both regions powered on and active.
The load balancer routes to both regions.

- If there is a failure on the region with the master database, the secondary
region with the read replica has to have its database promoted to a master
instance.

__Set up Health Checks in Route 53__

__Create a Route 53 Failover Record Set__  

### Takeaways ###

- Do not carryout day to day administration from your root account. Rather
use an IAM account.

- Use groups. Put multiple users in a group and apply policies to groups.

- Enable Multi Factor Authentication (MFA)

- Setup complex password policies for your IAM users.

- Audit API activity with CloudTrail and save the information in an S3 bucket

- Create activity alerts with CloudWatch

- Create AWS Config rules for change management.

- Use Trusted Advisor for managing service limits.

- Choose your region. Each region has different functionality available.

- Implement multi - AZ redundancy via horizontal scaling.

- Deploy across multiple AZs and regions

- Use auto-scaling groups

- Automate Deployments with Auto Scaling Groups

- Scale dynamically based on resource utilization

- Prefer services like RDS, S3, DynamoDB, ELB, which automate auto-scaling

- Implement louse coupling with SQS and/or microservices

- Use security groups

- Implement shared storage using EFS

- Load balance the application tier with ELB

- Configure SSL on the load balancer not EC2 instances

- Auto scaling groups carry out system checks to ensure the server is running,
whereas ELBs carry out health checks to ensure you application is running.

- Validate automatically using self healing servers

- Automatically recover a failed AZ

- Automate infrastructure with CloudFront

- Use blue/green or canary deployment patterns

- Use multi-region architecture like Pilot Light, Warm Standby and Active/Active

- Setup health checks in Route 53

- Create a Route 53 failover record set.

### Recommended Reading ###

- [AWS - Reliability](https://wa.aws.amazon.com/wat.pillar.reliability.en.html)

- [AWS - Reliability Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf)

- Read managing VPN Connections

### References ###

- [Lexico.com - Definition of Reliability](https://www.lexico.com/en/definition/reliability)

- [AWS - 5 Pillars of Well Architected Framework]([pillar](https://aws.amazon.com/blogs/apn/the-5-pillars-of-the-aws-well-architected-framework/))

- [AWS - Reliability](https://wa.aws.amazon.com/wat.pillar.reliability.en.html)

- [AWS - Reliability Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf)

- [AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html)

- [About Web Identity Federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc.html)

- [About SAML2 Federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html)

- [Microservices](https://microservices.io/)

- [AWS Config](https://aws.amazon.com/config/)

- [AWS Regions and Availability Zones](/2020/03/17/AWS_Regions-Availability-Zones-and-Local-Zones)

- [GitHub - AWS Sample Reference Architectures](https://github.com/search?q=org%3Aaws-samples+refarch)

- [GitHub - AWS CloudFormation Reference Architectures](https://github.com/aws-samples/ecs-refarch-cloudformation)

- [AWS Direct Connect](https://aws.amazon.com/directconnect/)
