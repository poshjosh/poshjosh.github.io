---
path: "./2020/03/09/CIDR-Blocks.md"
date: "2020-03-09T23:04:19"
title: "Introduction to CIDR Blocks"
description: "Brief Introduction to CIDR Blocks"
lang: "en-us"
---

### Acronyms ###

CIDR - Classless Inter-Domain Routing

### What is CIDR? ###

Classless Inter-Domain Routing (CIDR) is a method for allocating IP addresses
and IP routing. CIDR notation is a compact representation of an IP address and
its associated routing prefix. The notation is constructed from an IP address,
a slash ('/') character, and an integer.

### CIDR Notation ###

An IPv4 address has 4 parts (e.g `192.168.100.0`), each of which is 8 bits. Hence and
IPv4 address is 32 bits in total. On the other hand, a CIDR block may be represented thus:
`192.168.100.14/24` The integer `24` after the slash `/` tells us that 24 out
of 32 bits of the ip address identify the network, wile 8 out of 32 bits identify the host.

- `192.168.100.14/24` represents the IPv4 address `192.168.100.14` and its associated routing
prefix `192.168.100.0`, or equivalently, its subnet mask `255.255.255.0`, which has 24 leading 1-bits.

- The IPv4 block `192.168.100.0/22` represents the 1024 IPv4 addresses from `192.168.100.0` to `192.168.103.255`.

- The IPv6 block `2001:db8::/48` represents the block of IPv6 addresses from
`2001:db8:0:0:0:0:0:0` to `2001:db8:0:ffff:ffff:ffff:ffff:ffff`.

- `::1/128` represents the IPv6 loopback address. Its prefix length is 128 which is the number
of bits in the address.

For IPv4, CIDR notation is an alternative to the older system of representing networks
by their starting address and the subnet mask, both written in dot-decimal notation.
`192.168.100.0/24` is equivalent to `192.168.100.0/255.255.255.0`.

### Calculating the number of addresses in a subnet ###

The number of addresses of a subnet may be calculated as 2 _to the power_
(address length − prefix length), where address length is 128 for IPv6 and 32
for IPv4. For example, in IPv4, the prefix length /29 gives:

2 _to the power_ (32 − 29)
2 _to the power_ 3 = 8
8 addresses

### References ###

- [Wikipedia - Class Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing "Wikipedia - Class Inter-Domain Routing")
