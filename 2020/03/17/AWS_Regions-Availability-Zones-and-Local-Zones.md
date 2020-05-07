---
path: "./2020/03/17/AWS_Regions-Availability-Zones-and-Local-Zones.md"
date: "2020-03-17"
title: "AWS Regions, Availability Zones and Local Zones"
description: "Easy to understand article on AWS Regions, Availability Zones and Local Zones"
lang: "en-us"
---

### Acronyms ###

- AZ - Availability Zone
- EC2 - Elastic Cloud Compute

### Introduction ###

Amazon EC2 is hosted in multiple locations world-wide. These locations are
composed of Regions, Availability Zones, and Local Zones. Resources aren't
replicated across Regions unless you specifically choose to do so.

- Each __Region__ is a separate geographic area.

- Each Region has multiple, isolated locations known as __Availability Zones__.

- __Local Zones__  A Local Zone is an extension of a Region that is in a
different location from your Region. It provides you the ability to place
resources, such as compute and storage, in multiple locations closer to your
end users. It provides a high-bandwidth backbone to the AWS infrastructure and
is ideal for latency-sensitive applications, for example machine learning.

### Concepts ###

Each Region is completely independent. Each Availability Zone is isolated, but
the Availability Zones in a Region are connected through low-latency links. The
following diagram illustrates the relationship between Regions, Availability
Zones, and Local Zones.

__Region, AZ, Local Zone concepts__
<br/>
![Region, AZ, Local Zone concepts](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/aws_regions.png)
<br/>
Region, AZ, Local Zone concepts. Source: _docs.aws.amazon.consumers_

Amazon EC2 resources are one of the following: global, tied to a Region, an
Availability Zone, or a Local Zone.

### Regions ###

Each Amazon EC2 Region is designed to be isolated from the other Amazon EC2
Regions. When you view your resources, you see only the resources that are tied
to the Region that you specified.

When you launch an instance, you must select an AMI that's in the same Region.
If the AMI is in another Region, you can copy the AMI to the Region you're using.

Note that there is a charge for data transfer between Regions

### Availability Zones ###

- If you distribute your instances across multiple AZs and one instance fails,
you can design your application so that an instance in another AZ can handle
requests.

- You can also use Elastic IP addresses to mask the failure of an instance in
one AZ by rapidly remapping the address to an instance in another AZ.

- An AZ is represented by a Region code followed by a letter identifier; for
example, `us-east-1a`.

- __Note__ The AZ `us-east-1a` for your AWS account might not be the same
location as `us-east-1a` for another AWS account. This is because, AWS ensures
that resources are distributed across the AZs by _independently mapping AZs to
names for each AWS account_.

- To coordinate AZs across accounts, you must use the AZ ID, which is a unique
and consistent identifier for an Availability Zone. For example, `use1-az1` is
an AZ ID for the `us-east-1` Region and it has the same location in every AWS
account.

- Viewing AZ IDs enables you to determine the location of resources in one
account relative to the resources in another account. For example, if you share
a subnet in the AZ with the AZ ID `use-az2` with another account, this subnet is
available to that account in the AZ whose AZ ID is also `use-az2`. The AZ ID for
each VPC and subnet is displayed in the Amazon VPC console.

- Each account might have a different number of available AZs in a Region. this
is because as AZs grow over time, AWS' ability to expand them becomes
constrained. If this happens, AWS might restrict you from launching an instance
in a constrained AZ (unless you already have an instance in that AZ). Eventually,
AWS might also remove the constrained AZ from the list of AZs for new accounts.

### Local Zones ###

A Local Zone is an extension of an AWS Region in geographic proximity to your
users. When you launch an instance, you can select a subnet in a Local Zone.
Local Zones have their own connections to the internet and support AWS Direct
Connect, so resources created in a Local Zone can serve local users with very
low-latency communications.

A Local Zone is represented by a Region code followed by an identifier that
indicates the location, for example, `us-west-2-lax-1a`.

To use a Local Zone, you must:

- Enable the local Zone.

- Create a subnet in the Local Zone.

- Launch any of the following resources in the Local Zone subnet, so that your
applications are closer to your end users:

  - Amazon EC2 instances

  - Amazon EBS volumes

  - Amazon FSx file servers

  - Application Load Balancer

  - Dedicated Hosts

- Local Zones are not available in every Region. You can list the Local Zones
that are available to your account. For more information, see
[Describing your Regions, Availability Zones, and Local Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#using-regions-availability-zones-describe)

### Network border groups ###

A network border group is a unique set of Availability Zones or Local Zones
from where AWS advertises IP addresses. You can allocate the following resources
from a network border group:

- Elastic IPv4 addresses that Amazon provides

- IPv6 Amazon-provided VPC addresses

A network border group limits the addresses to the group. IP addresses cannot
move between network border groups.

### References ###

- [AWS Concepts - Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-regions-availability-zones)
