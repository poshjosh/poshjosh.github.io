---
path: "./2020/04/14/AWS-Architect-2_Architecting-for-Security.md"
date: "2020-04-14T10:05:00"
title: "AWS Achitect 2 - Architecting for Security"
description: "Game changing tutorial on AWS - Architecting for Security"
tags: ["AWS", "tutorial", "architect", "security", "defense in depth", "root user", "Identity and Access Management (IAM)", "policies", "permissions", "encryption", "multi-factor authentication", "CloudTrail", "CloudWatch", "AWS Config", "Network Access Control List (NACL)", "security group", "IAM role", "VPC Endpoint", "HTTPS", "TLS", "SSL", "subnet", "Customer Master Key", "CloudFront", "backup", "replication", "recovery", "versioning", "cross region replication", "lifecycle rules", "key management service (KMS)", "Origin Access Identity (OAI)"]
lang: "en-us"
---

### Acronyms ###

- ACM - AWS Certificate Manager
- AZ - Availability Zone
- CMK - Customer Master Key
- EBS - Elastic Block Store
- EC2 - Elastic Cloud Compute
- HTTP - Hyper Text Transfer Protocl
- IAM - Identity and Access Management
- KMS - Key Management Service
- MFA - Multi - factor Authentication
- NACL - Network Access Control List
- OAI - Origin Access Identity
- S3 - Simple Storage Service
- SSL - Secure Sockets Layer
- TLS - Transport Layer Security  

### Before we set off ###

__About this Article__

- Hi, this article contains game changing information for those who would like
to get AWS Certified Cloud Architect certified.

- It is one of a series of articles.

- This is the second article in the series. [The first part](/2020/04/14/AWS-Architect-1_Architecting-for-Reliability).

- It is an estimated 35 minute read.

__This Article is not an Introduction to AWS__

This series of articles is not an introduction to AWS or any of the core
concepts of the AWS Cloud. You need to be already familiar with some core
concepts of AWS Cloud to fully benefit from this article. In order words,
if you are not familiar with the AWS cloud, you should read the series of
articles beginning at:

- [Notes on Amazon Web Services - 1](/2020/03/02/Notes-on-Amazon-Web-Services_1_Introduction)

- [AWS Certified Solutions Architect Associate - Part 1](/2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-1_Key-services-relating-to-the-Exam)

### What does it mean to architect for security? ###

First off, what is security?

Security
: is the state of being free from danger or threat, also the procedures followed
or measures taken to ensure this state.

It makes sense to have our cloud based applications free from danger or threats.
This is why security is one of Amazon's [5 pillars of a well architected system](https://aws.amazon.com/blogs/apn/the-5-pillars-of-the-aws-well-architected-framework/)
and security involves 6 design principles:

1. Implement a strong identity foundation.

2. Enable traceability.

3. Apply security at all layers.

4. Automate security best practices.

5. Protect data in transit and at rest.

6. Prepare for security events.

The goal of information security is to protect data. This specifically means
protecting or ensuring: `Confidentiality`, `Integrity` and `Availability` (CIA)
of data.

- __Confidentiality__. Only authorized access to data. Know when un authorized
access happens.

- __Integrity__. Protect data from improper modification. Know when it happens.

- __Availability__. Data can be accessed when needed. Know when data is not
available.

__Defence in depth__. Apply security controls to every thing that touches the
data i.e storage, compute and networking.

__Levels of Security__

- AWS Services - AWS takes care of securing managed services
- Operation Systems - Secure OS on your instances
- Applications - Secure your apps

### Protecting AWS Credentials ###

__Credential Types__

- __Root user__ - Only one per account, has full access

- __IAM (non-root)  principal__ - An entity that can perform actions on AWS.
Policies determine what permissions a principal has.

  * __IAM Users__

  > An IAM user is an entity that you create in AWS. The IAM user represents the
  > person or service who uses the IAM user to interact with AWS. A primary use for
  > IAM users is to give people the ability to sign in to the AWS Management Console
  > for interactive tasks and to make programmatic requests to AWS services using
  > the API or CLI.

  * __IAM Groups__

  > An IAM group is a collection of IAM users. You can use groups to specify
  > permissions for a collection of users, which can make those permissions easier
  > to manage for those users. For example, you could have a group called Admins
  > and give that group the types of permissions that administrators typically need.
  > Note that a group is not truly an identity because it cannot be identified as a
  > Principal in a resource-based or trust policy. It is only a way to attach
  > policies to multiple users at one time.

  * __IAM Roles__

  > An IAM role is very similar to a user, in that it is an identity with
  > permission policies that determine what the identity can and cannot do in AWS.
  > However, a role does not have any credentials (password or access keys)
  > associated with it. Instead of being uniquely associated with one person, a
  > role is intended to be assumable by anyone who needs it.

  * __Temporary Credentials__

  > Temporary credentials are primarily used with IAM roles, but there are also
  > other uses. You can request temporary credentials that have a more restricted
  > set of permissions than your standard IAM user. A benefit of temporary
  > credentials is that they expire automatically after a set period of time.
  > You have control over the duration that the credentials are valid.

AWS account is the container that houses resources and billing info amongst
others. You log in to that container/account as the root or IAM user.

- Enable MFA

- Don't use the root user for administrative tasks.

__Policies and Permissions__

You create IAM users and grant them permissions. A policy consists of one or
more permission statements.

A __permission statement__ comprises:

- Effect (allow or deny) - e.g `Allow`
- Service - `EC2`
- Action/operation - e.g `RunInstances`
- Resource (depends on service) - e.g `image/ami-e5d9439a`
- Request condition (MFA, IP range, time etc) - e.g `198.51.100.0/24`

- There are AWS managed policies. Rather than creating policies from scratch,
start with an AWS managed policies e.g `AdministratorAccess` whose policy
values follow:

```
Effect: allow
Service: *
Action: *
Resource: *
```

- You could also have a user with `AdministratorAccess` but apply another
policy which restricts the user from terminating EC2 instances, with the following values:

```
Effect: deny
Service: EC2
Action: TerminateInstances
Resource: *
```

- Note the deny effect takes precedence over the allow effect.

- You can also apply a policy to a group by creating an inline policy for
the group. This is useful when many users require the same permissions.

IAM Console -> Groups -> Click `permissions tab` -> Click on inline policy

### Capturing and Analyzing Logs ###

You need to have an insight into what is happening as well as keep track of
changes that happen to your AWS resources etc. CloudTrail keeps default logs
but keeps them for a limited period of time. If you want improved security you
follow the outline process:

- Configure CloudTrail to capture every event that happens in our environment and
store records of them in log files.

- Use CloudWatch to browse and search the events.

- Also use CloudWatch alarms to alert us whenever a change takes place.

- Search the logs with Athena using SQL queries

- Track changes to configuration in AWS environment with AWS Config.

__CloudTrail__ log AWS logs and stores them in S3 buckets. CloudTrail can log:

- Management events: All, read-only, write-only, none

- Data events: S3, Lamda

For example we can use CloudTrail to monitor all write events.

__CloudWatch Logs__

- Aggregates log files from multiple sources including CloudTrail and other
non-AWS sources.

- Provides interface to view and search the logs.

To use CloudWatch logs:

- Create a log group

- Create an IAM role for CloudTrail to use, which contains.

  * Inline service policy that grants CloudTrail permissions to send
  logs to CloudWatch logs.

  * Trust policy that allows CloudTrail assume the role.

- Create CloudWatch alarm to trigger when CloudTrail logs a specific event.
The alarm is not real-time.

- Search the logs with Athena using SQL queries

- Track changes to configuration in AWS environment with AWS Config.

__Tracking changes: CloudTrail vs AWS Config__

__CloudTrail__ tracks who made the change, what resource changed and when.

On the other hand, __AWS Config__ tracks what a service configuration looks like
at a point in time. For example, you could use it to see what a configuration
looked like at some point in the past.

__AWS Config__

- Tracks configuration changes over time.

- Records changes in S3

- Notifications of changes.

### Protecting Network and Host-level Boundaries ###

__Network level boundary__ - network ACL - applies to subnets.

__Host level boundary__ - security group - applies to EC2 instances.

__Application layer__ - App and Database, S3

- Dynamo Db like S3 lives outside the VPC

- For dynamo db to reach the internet gateway we need to:

  * Modify the main route table for the VPC

  ```
  DynamoDB - internet gateway - route table - NACL - security group - app
  ```

  OR

  * Use a VPC endpoint (this gives a non-internet connection).

  ```
  DynamoDB - VPC endpoint - route table - NACL - security group - app
  ```

  _When using VPC endpoint, remember to block all outbound internet traffic._

To protect network and host-level boundaries:  

- Create VPC or use an existing one

- Create subnet or use an existing one

- Create internet gateway

- Attach it to the VPC

- Modify default route table to make the subnet public. Add this rule:
  * Destination: 0.0.0.0/0, Target=[The internet gateway] e.g `igw-CEd23vwMs21022`

- Reconfigure default security group to allow access SSH and HTTP access only
for our ip address.

- Create a policy to allow application access DynamoDB
  * IAM -> Policies -> Create Policy

- Create a role for the EC2 to assume and which we will attach the policy to.
  * IAM - Roles -> Create Policy
  * Attach the Policy you created earlier
  * The role contains trust policy to allow EC2 instances assumme the role

- When we create the IAM role an `instance profile` will be automatically created
for us. `IAM role` and `instance profile` are practically the same.   

- When we launch an EC2 instance, we:
  * Attach the `instance profile` under the `IAM role` option.
  * Attach the security group we created earlier

Using __VPC Endpoints__ offers more security.

- Creating a VPC Endpoint

  * VPC Console -> Endpoints -> Click on `Create Endpoint`
  * Select the service by name
  * Select the route table (any existing connections will be interrupted)
  * A VPC endpoint policy is created for you

- Creating a VPC Endpoint should not be done during business hours as
any existing connections will be interrupted.

- The VPC endpoint modifies the route table (you selected when creating it)
to add a route with destination having prefix `pl-` and target having prefix
`vpce-`. `pl` stands for prefix list which is a list of ip prefixes while
`vpce` stands for VPC Endpoint.

- The prefix list could be viewed using the AWS CLI via the command.
```
aws ec2 describe-prefix-lists
```

To protect the AWS resources in each subnet, you can use multiple layers of security,
including security groups and network access control lists (ACL). For more information,
see [Internetwork Traffic Privacy in Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security/).

__Security Groups__

By default, a security group includes an outbound rule that allows all outbound traffic.
You can remove the rule and add outbound rules that allow specific outbound traffic
only. If your security group has no outbound rules, no outbound traffic originating
from your instance is allowed.

- Security groups -> Outbound rules
  * Remove all outbound rules
  * Add custom tcp rule for
  Type=`HTTPS`, Protocol=`TCP`, Port Range=`443`, Destination=`the prefix list from the
  VPC Endpoint`.

- Security groups -> Inbound rules
  * Remember if you allow traffic rule from an ip address, the stateful nature
  of the security group will allow outbound traffic from the same address.

__NACL__

You can associate a network ACL with multiple subnets. However, a subnet can be
associated with only one network ACL at a time. When you associate a network ACL
with a subnet, the previous association is removed.

Every VPC gets a default NACL and the default subnet gets the default NACL.

__NACL vs Security Group__

_TL;DR: Security group is the firewall of EC2 Instances whereas Network ACL is the firewall of the Subnet._

Difference between Security Groups and NACLs
<br/>
![Difference between Security Groups and NACLs](https://miro.medium.com/max/944/1*pwAjuZMHsDJV6XckZGARxA.png "Difference between Security Groups and NACLs")
<br/>
_Difference between Security Groups and NACLs. Source: miro.medium.com_

- __Scope: Subnet or EC2 Instance__

When you apply an NACL to a subnet, it's rules apply to all instances in the
subnet. On the other hand, security groups have to be applied to each instance.
If you have many instances, managing the firewalls using NACL could be very useful

- __State: Stateful vs Stateless__

Security groups are stateful. This means any changes applied to an incoming rule
will be automatically applied to the outgoing rule. e.g. If you allow an incoming
port 80, the outgoing port 80 will be automatically opened.

Network ACLs are stateless. This means any changes applied to an incoming rule
will not be applied to the outgoing rule. e.g. If you allow an incoming port 80,
you would also need to apply the rule for outgoing traffic.

- __Rules: Allow or Deny__

Security group support allow rules only (by default all rules are denied). e.g.
You cannot deny a certain IP address from establishing a connection.

Network ACL support allow and deny rules. By deny rules, you could explicitly deny
a certain IP address to establish a connection example: Block IP address
`123.201.57.39` from establishing a connection to an EC2 Instance.

- __Rules Process order__

All rules in a security group are applied whereas rules are applied in their order
(the rule with the lower number gets processed first) in Network ACL.i.e. Security
groups evaluate all the rules in them before allowing a traffic whereas NACLs do it
in the number order, from top to bottom.

[Click here for more VPC Security](/2020/03/04/AWS_Virtual-Private-Cloud-VPC) including
[detailed differences between NACL and security groups](/2020/03/04/AWS_Virtual-Private-Cloud-VPC)   

### Protecting Data at Rest ###

Two ways to protect data at rest on AWS are:

- __Access permissions__ e.g policies and ACLs

- __Encryption__. Requires acces to a key to encrypt and decrypt the data.

__Protecting data at rest tasks include:__

- Create a Customer Master Key (CMK) key using Key Management Service (KMS)

- Encrypt an EBS Volume

- Create and use S3 access control lists, bucket polices and user polices

- Securely grant anonymous access to S3 objects

- Encrypt S3 objects.

__A Word on Encryption Keys__

- Key administrator can manage the key but not use the key. Managing the key
includes making changes to the key and giving other principals
(including themselves) permission to use the key.

- You can allow principals from another AWS account use the key you create.

- AWS keys have policies.

- AWS creates default encryption keys for you but you could also create yours.

__To Encrypt an existing EBS Volume__

- Stop the EC2 instance

- Take a snapshot of the root volume

- Make an encrypted copy of the snapshot

- Create an AMI using the encrypted snapshot

- Launch another instance using the new AMI

__S3 Bucket Policies__

__Grant access to a particular principal.__

- Create a bucket policy using the policy generator.

- Specify the arn of the user you want to grant access under the `Principal` option.

Bucket policies are the preferred way to secure bucket objects. However
ACLs could be used to grant anonymous access to specific bucket objects.

__Grant anonymous access to a specific S3 object__. Two ways:

1. Grant read permissions ot everyone using the particular object's ACL

2. Use a bucket policy to grant everyone permission to perform the
`GetObject` action against the particular object.

__Encrypt S3 Objects using a Customer Master Key__

- When S3 Objects are encrypted with a CMK, principals must be granted
permission to use the CMK before they can decrypt data from the S3 bucket.

- To encrypt S3 objects:

  * Generate a Customer Master Key

  * Enable encryption on the S3 bucket

  * Verify that unauthorized users can't decrypt data from the S3 bucket,

- Encrypting s3 objects doesn't apply to existing objects.

- If you want existing objects to be encrypted you have to modify the properties
of each such object to enable encryption.

- Before you delete a key you use to encrypt or decrypt data, make sure you
decrypt all data the key has been used to encrypt.

- AWS imposes a minimum 7 day waiting period before it deletes a CMK key.

__Access S3 Objects through CloudFront__

For added security you could access s3 bucket objects only through
CloudFront. This could be achieved by granting CloudFront origin access
identity to an s3 bucket. This is useful when you want to make s3 bucket
objects only avaiable from a cloudfront distribution and not directly from
an s3 bucket. To achieve this:

- Create an origin access identity (OAI)

- Grant OAI read-only access to the bucket

- Create a CloudFront distribution which uses the OAI.

### Protecting Data in Transit ###

Standard for encryption over the internet is Transport Layer Security (TLS)
TLS is often incorrectly referred to as secure sockets layer (SSL). HTTPS
uses TLS. Traffic within the AWS cloud is not encrypted by default.

You could terminate TLS connections on:

- Individual EC@ instances - not recommended

- Application load balancer - requires at least 2 AZs.

__To Encrypt Data in Transit on AWS__

- Use AWS Certificate Manager (ACM) to generate a TLS certificate. The
certificate for the entire domain e.g `*.example.com`

- The certificate establishes the identity of the domain name.

- Create an AWS application load balancer

- Configure application load balancer to use TLS by installing the TLS
certificate on the load balancer.

- Force all clients through the load balancer by setting up a DNS record for
the domain name pointing to the load balancer.

### Configuring Data Backup, Replication and Recovery in S3 ###

Prefer S3 to EBS volumes. You can protect data stored in S3 by using:

- Versioning - A new version is created each time you update an object.
Even when you delete an object it is not deleted instantly but marked.
To totally delete an object:

  * Delete the object.
  * Show object versions
  * Delete all versions

- LifeCycle rules. Lets you move data to different storage classes based on
the age of the object. Lifecycle rules could also be used to delete old objects.

- Cross region replication. Not enabled by default.
  * By default an S3 bucket is created in only one region and AZ.
  * Replication when applied, does not affect existing objects.
  * To enable replication:
  ```
  S3 -> Management -> Replication -> Click the `Add Rule` button
  ```
  * If you delete a replicated object in the source region, s3 does not
  automatically delete the replicated instance of the object.

### Takeaways ###

- Enable MFA

- Don't use the root user for administrative tasks.

- For policies, the deny effect takes precedence over the allow effect.

- Configure CloudTrail to capture every event that happens in our environment and
store records of them in log files.

- Use CloudWatch to browse and search the events.

- Use CloudWatch alarms to alert you whenever a change takes place.

- Track changes to configuration in AWS environment with AWS Config.

- Using VPC Endpoints offers more security.

- Creating a VPC Endpoint should not be done during business hours as
any existing connections will be interrupted.

- Remember if you allow traffic rule from an ip address, the stateful nature
of the security group will allow outbound traffic from the same address.

- Key administrator can manage the key but not use the key. Managing the key
includes making changes to the key and giving other principals
(including themselves) permission to use the key.

- You can allow principals from another AWS account use the CMK key you create.

- For added security you could access s3 bucket objects only through
CloudFront. This could be achieved by granting CloudFront origin access
identity to an s3 bucket.

- Encrypting s3 objects doesn't apply to existing objects.

- If you want existing objects to be encrypted you have to modify the properties
of each such object to enable encryption.

- AWS imposes a minimum 7 day waiting period before it deletes a CMK key.

- Traffic within the AWS cloud is not encrypted by default.

- For added security, use an application load balancer (configured to use TLS)
and ensure all traffic routes through the load balancer.

- Application load balancer requires at least 2 Availability Zones.

- Protect data in S3 buckets through versioning, life cycle rules and
cross region replication.

- For S3 buckets, cross region replication is not enabled by default

- If you delete a replicated object in the source region, s3 does not
automatically delete the replicated instance of the object.

### References ###

- [Lexico.com - Definition of Security](https://www.lexico.com/en/definition/security)

- [AWS - 5 Pillars of Well Architected Framework]([pillar](https://aws.amazon.com/blogs/apn/the-5-pillars-of-the-aws-well-architected-framework/))

- [AWS - Security](https://wa.aws.amazon.com/wat.pillar.security.en.html)

- [AWS - Security Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Security-Pillar.pdf)

- [AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html)

- [AWS VPC](/2020/03/04/AWS_Virtual-Private-Cloud-VPC)
