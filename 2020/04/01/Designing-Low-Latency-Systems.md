---
path: "./2020/04/01/Designing-Low-Latency-Systems.md"
date: "2020-04-01"
title: "Designing Low Latency Systems"
description: "Quickly get started designing low latency systems"
tags: ["AWS", "low latency", "designing low latency", "java memory model", "java.nio", "Financial Information Exchange (FIX)", "High Frequency Trading (HFT)"]
lang: "en-us"
---

### Acronyms ###

- FIX Protocol - Financial Information Exchange Protocol

- HFT - High-Frequency Trading

### What does Low Latency mean ###

- __Latency__ is a time interval between the stimulation and response, or, from
a more general point of view, a time delay between the cause and the effect of
some physical change in the system being observed.

- __Low latency__ describes a computer network that is optimized to process a
very high volume of data messages with minimal delay (latency).

### Designing for low latency in Java Programming Language ###

- Detailed knowledge of the Java memory model and techniques to avoid unnecessary
garbage collection (e.g via object pooling) is very important. Some of the
techniques used might typically be regarded as "anti-patterns" in a traditional
OO-environment.

- Detailed knowledge of TCP/IP and UDP multicast including utilities for debugging and measuring latency (e.g. DTrace on Solaris).

- Experience with profiling applications.

- Knowledge of the java.nio package, experience developing NIO-based scalable server applications, experience designing wire protocols. Also note that we typically avoid using frameworks and external libraries (e.g. Google Protobuf), preferring to write a lot of bespoke code.

- Knowledge of [FIX Protocol](http://www.marketswiki.com/wiki/Financial_Information_Exchange_protocol)

=========== TO DO ===========
Very Important - Read link first link below and add
https://www.linkedin.com/pulse/what-ive-learned-after-coding-hft-low-latency-systems-ariel

Multicast, UDP, reliable multicast, Kernel bypass (supported by Java 7, Informatica Ultra Messaging, and many others) on Infiniband networks are some common technologies used by all companies in this field.
http://code.google.com/p/disruptor/

pre-allocate objects(i.e use flyweight design pattern), use primitive objects

- [The C10k Problem](http://www.kegel.com/c10k.html)

- [G-wan - Help IoTs make better use of CPU cores](http://g-wan.com/)

- [Java Development without Garbage Collection](http://www.coralblocks.com/index.php/java-development-without-gc/)

### References ###

- [Webopedia - Latency](http://www.webopedia.com/TERM/L/latency.html)

- [Informatica - Low Latency](https://www.informatica.com/services-and-training/glossary-of-terms/low-latency-definition.html)

- [FIX Protocol](http://www.marketswiki.com/wiki/Financial_Information_Exchange_protocol)
