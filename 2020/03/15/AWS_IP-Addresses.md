---
path: "./2020/03/15/AWS_IP-Addresses.md"
date: "2020-03-15"
title: "AWS - IP Addresses"
description: "IP Addresses - Must Read, for AWS Certified Cloud Architect certifications"
lang: "en-us"
---

### Acronyms ###

CIDR - Classless Inter-domain Routing
IP - Internet Protocol

### IP addresses ###

- A `Public IP address` is how you are identified from the internet.

- A `Private IP address` is how you are identified from within a private network.

- IP addresses could be static or dynamic.

- A `Static IP address` once assigned to you is not changed. On the other hand,
a dynamic IP address may be changed by the provider each day, per session etc

- In AWS, you cannot reuse a public IPv4 address, and you cannot convert a public
IPv4 address to an Elastic IP address.

- An `Elastic IP address` is a public static IPv4 address designed for dynamic
cloud computing. An Elastic IP address is associated with your AWS account. With
an Elastic IP address, you can mask the failure of an instance or software by
rapidly remapping the address to another instance in your account.

- When you associate an Elastic IP address with an instance or its primary
network interface, the instance's public IPv4 address (if it had one) is
released back into Amazon's pool of public IPv4 addresses.

- While your instance is running, you are not charged for one Elastic IP address
associated with the instance, but you are charged for any additional Elastic IP
addresses associated with the instance.

- Elastic IP addresses incur a small hourly bill if not associated with a
running instance, or if associated with a stopped instance or an unattached
network interface.

- You can bring part or all of your public IPv4 address range from your
on-premises network to your AWS account. You continue to own the address range,
but AWS advertises it on the internet. After you bring the address range to AWS,
it appears in your account as an address pool. You can create an Elastic IP
address from your address pool and use it with your AWS resources, such as
EC2 instances, NAT gateways, and Network Load Balancers.

### IPv6 Addresses ###

- `IPv6 addresses` are globally unique and public. They are therefore reachable
over the Internet.

- `IPv6 CIDR block`

  - VPC - Optionally one IPv6 CIDR block (IPs automatically assigned by Amazon)
  - Subnet - One or more IPv6 CIDR block.

### CIDR Blocks ###

CIDR Blocks enable you specify an IP address range. This is done using an IP
address and a forward slash. [Read this for more information on CIDR Blocks](/2020/03/09/CIDR-Blocks)  

### References ###

- [AWS EC2 - Instance Addressing](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html)

- [AWS EC2 - Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html)

- [AWS EC2 - Bringing your own IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-byoip.html)

- [CIDR Blocks](/2020/03/09/CIDR-Blocks)
