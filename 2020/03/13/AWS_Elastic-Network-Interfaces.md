---
path: "./2020/03/13/AWS_Elastic-Network-Interfaces.md"
date: "2020-03-13"
title: "AWS Elastic Network Interfaces"
description: "Easy to understand tutorial: AWS elastic network interfaces"
lang: "en-us"
---

- Every instance in a VPC has a default network interface, called the
__primary network interface (eth0)__. You cannot detach a primary network interface
from an instance. You can create and attach additional network interfaces.

- __You can't detach the primary network interface (eth0).__

- __Attaching from same subnet causes conflict__ If you attach two or more network
interfaces from the same subnet to an instance, you may encounter networking issues
such as asymmetric routing. If possible, use a secondary private IPv4 address on
the primary network interface instead.

- __Hot, warm, cold attaching of eni.__ You can attach a network interface to an
instance when it's running (hot attach), when it's stopped (warm attach), or
when the instance is being launched (cold attach).

- __You can move a network interface from one instance to another__, if the
instances are in the same Availability Zone and VPC but in different subnets.

- Attaching another network interface to an instance does not increase or double
the network bandwidth to or from the dual-homed instance.

### Scenarios for Network Interfaces ###

Attaching multiple network interfaces to an instance is useful when you want to:

- __Create a management network.__ primary network interface (eth0) on the instance
handles public traffic and the secondary network interface (eth1) handles backend
management traffic and is connected to a separate subnet in your VPC that has more
restrictive access controls.

![Image](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/EC2_ENI_management_network.png)
<br/>
Source: _docs.aws.amazon.com_

- __Use network and security appliances in your VPC.__ Some network and security
appliances, such as load balancers, network address translation (NAT) servers,
and proxy servers prefer to be configured with multiple network interfaces.

- __Create dual-homed instances with workloads/roles on distinct subnets.__ You
can place a network interface on each of your web servers that connects to a mid-tier
network where an application server resides. The application server can also be
dual-homed to a backend network (subnet) where the database server resides.

- __Create a low-budget, high-availability solution.__ If one of your instances
serving a particular function fails, its network interface can be attached to a
replacement or hot standby instance pre-configured for the same role in order
to rapidly recover the service.

### References ###

- [AWS - Using Elastic Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni/)
