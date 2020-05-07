---
path: "./2018/01/24/Software-Engineers-Reference_Dictionary-Encyclopedia-or-Wiki-for-Software-Engineers.md"
date: "2018-01-24T23:49:28"
title: "Software Engineers Reference - Dictionary, Encyclopedia or Wiki - For Software Engineers"
description: "Curated must-know information for Software Engineers"
tags: ["DevOps", "HDFS", "YARN", "CQRS", "CAP theorem", "ACID", "SOLID", "Idempotence", "Continuous Integration (CI)", "Continuous Delivery (CD)", "Infrastructure as a Service (IaaS)", "Platform as as Service (PaaS)", "Software as a Service (SaaS)", "Infrastructure as Code (IaC)", "Application Programming Interface (API)", "Service Oriented Architecture (SOA)", "Remote Procedure Call (RPC)", "The twelve (12) factor app", "HTTP Verbs"]
lang: "en-us"
---

### ACRONYMS ###

- CAP			Consistency Availability Partition tolerance
- CQRS			Command Query Responsibility Segregation
- ACID			Atomicity Consistency Isolation Durability
- SOLID
- DDL			Data Definition Language
- DML			Data Manipulation Language
- RDD			Resilient Distributed Dataset
- SLA			Service Level Agreement
- HDFS			Hadoop Distributed File System
- YARN			Yet Another Resource Negotiator
- YAML 			YAML Ain't Markup Language

### MEANINGS ###

__DevOps__: 		Practice where the team which builds a service operates it.

__HDFS__: The Hadoop Distributed File System (HDFS) is the primary data storage system used by Hadoop applications. It employs a NameNode and DataNode architecture to implement a distributed file system that provides high-performance access to data across highly scalable Hadoop clusters.

__YARN__: Apache Hadoop YARN is the resource management and job scheduling technology in the open source Hadoop distributed processing framework. YARN stands for Yet Another Resource Negotiator. Yarn allows different data processing engines like graph processing, interactive processing, stream processing as well as batch processing to run and process data stored in HDFS (Hadoop Distributed File System). Apart from resource management, Yarn is also used for job Scheduling.

__CQRS__: stands for Command Query Responsibility Segregation. It's a pattern that I first heard described by Greg Young. At its heart is the notion that you can use a different model to update information than the model you use to read information. Notable, methods Object get() as well as void set(Object o) meet the requirements but Object update(Object o) does not.

__CAP theorem__: states that it is impossible for a distributed data store to simultaneously provide more than two out of the following three guarantees:
  - `Consistency`: Every read receives the most recent write or an error
  - `Availability`: Every request receives a (non-error) response, without the guarantee that it contains the most recent write
  - `Partition tolerance`: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes

  When a network partition failure happens should we decide to:
  - Cancel the operation and thus decrease the availability but ensure consistency
  - Proceed with the operation and thus provide availability but risk inconsistency

__ACID__: Atomicity, Consistency, Isolation, Durability.

  - `Atomicity`: Transactions are often composed of multiple statements. Atomicity guarantees that each transaction is treated as a single "unit", which either succeeds completely, or fails completely. An atomic system must guarantee atomicity in each and every situation, including power failures, errors and crashes.

  - `Consistency`: Consistency ensures that a transaction can only bring the database from one valid state to another, maintaining database invariants: any data written to the database must be valid according to all defined rules, including constraints, cascades, triggers, and any combination thereof. This prevents database corruption by an illegal transaction, but does not guarantee that a transaction is correct.

  - `Isolation`: Transactions are often executed concurrently (e.g., multiple transactions reading and writing to a table at the same time). Isolation ensures that concurrent execution of transactions leaves the database in the same state that would have been obtained if the transactions were executed sequentially. Isolation is the main goal of concurrency control; depending on the method used, the effects of an incomplete transaction might not even be visible to other transactions.

  - `Durability`: Durability guarantees that once a transaction has been committed, it will remain committed even in the case of a system failure (e.g., power outage or crash). This usually means that completed transactions (or their effects) are recorded in non-volatile memory.

__SOLID__

  - Single responsibility principle. A class should only have a single responsibility, that is, only changes to one part of the software's specification should be able to affect the specification of the class.

  - `Open–closed principle`: "Software entities ... should be open for extension, but closed for modification."

  - `Liskov substitution principle`: "Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program." See also design by contract.

  - `Interface segregation principle`: "Many client-specific interfaces are better than one general-purpose interface."

  - `Dependency inversion principle`: One should "depend upon abstractions, [not] concretions."

__Idempotence__: Idempotence is the property of certain operations in mathematics and computer science whereby they can be applied multiple times without changing the result beyond the initial application.

__Continuous Integration (CI)__: Developers integrate code into a shared repository multiple times a day and each isolated change to the code is tested immediately in order to detect and prevent integration problems.
Some vendors and tools: Jenkins, Teamcity, TravisCI, CircleCI

__Continuous Delivery (CD)__: As an extension of CI and the next step in incremental software delivery, continuous delivery (CD) ensures that every version of the code that is tested in the CI repository can be released at any moment.
Some vendors, tools: Jenkins, Teamcity, TravisCI, Electric Cloud, Go, Codeship, AWS CodeDeploy

__Infrastructure as a Service (IaaS)__: @TODO

__Platform as a Service (PaaS)__: @TODO

__Software as a Service (SaaS)__: @TODO

__Infrastructure as Code (IaC)__: @TODO

__Application Programming Interface (API)__: is an interface or communication protocol between different parts of a computer program intended to simplify the implementation and maintenance of software.

More recently, the term has been often used to refer to a specific kind of interface between a client and a server, which has been described as a “contract” between both - such that if the client makes a request in a specific format, it will always get a response in a specific format or initiate a defined action.
This is a specialized form of API, sometimes defined as a Web API.

__Service-oriented architecture (SOA)__: is a style of software design where services are provided to the other components by application components, through a communication protocol over a network. An SOA service is a discrete unit of functionality that can be accessed remotely and acted upon and updated independently, such as retrieving a credit card statement online. SOA is also intended to be independent of vendors, products and technologies.

A service has four properties according to one of many definitions of SOA:

  - It logically represents a business activity with a specified outcome.
  - It is self-contained.
  - It is a black box for its consumers, meaning the consumer does not have to be aware of the service's inner workings.
  - It may consist of other underlying services.

SOA is related to the idea of an application programming interface (API), an interface or communication protocol between different parts of a computer program intended to simplify the implementation and maintenance of software. An API can be thought of as the service, and the SOA the architecture that allows the service to operate.

The related buzzword service-orientation promotes loose coupling between services. SOA separates functions into distinct units, or services, which developers make accessible over a network in order to allow users to combine and reuse them in the production of applications. These services and their corresponding consumers communicate with each other by passing data in a well-defined, shared format, or by coordinating an activity between two or more services.

A manifesto was published for service-oriented architecture in October, 2009. This came up with six core values which are listed as follows:

  - Business value is given more importance than technical strategy.
  - Strategic goals are given more importance than project-specific benefits.
  - Intrinsic inter-operability is given more importance than custom integration.
  - Shared services are given more importance than specific-purpose implementations.
  - Flexibility is given more importance than optimization.
  - Evolutionary refinement is given more importance than pursuit of initial perfection.

SOA can be seen as part of the continuum which ranges from the older concept of distributed computing and modular programming, through SOA, and on to current practices of mashups, SaaS, and cloud computing (which some see as the offspring of SOA).

__Remote Procedure Call (RPC)__: In distributed computing, RPC is when a computer program causes a procedure (subroutine) to execute in a different address space (commonly on another computer on a shared network), which is coded as if it were a normal (local) procedure call, without the programmer explicitly coding the details for the remote interaction. That is, the programmer writes essentially the same code whether the subroutine is local to the executing program, or remote. This is a form of client–server interaction (caller is client, executor is server), typically implemented via a request–response message-passing system. In the object-oriented programming paradigm, RPC calls are represented by remote method invocation (RMI). The RPC model implies a level of location transparency, namely that calling procedures is largely the same whether it is local or remote, but usually they are not identical, so local calls can be distinguished from remote calls. Remote calls are usually orders of magnitude slower and less reliable than local calls, so distinguishing them is important.

__The Twelve Factor App__: The twelve-factor app is a methodology for building software-as-a-service apps that:

Use declarative formats for setup automation, to minimize time and cost for new developers joining the project;
Have a clean contract with the underlying operating system, offering maximum portability between execution environments;
Are suitable for deployment on modern cloud platforms, obviating the need for servers and systems administration;
Minimize divergence between development and production, enabling continuous deployment for maximum agility;
And can scale up without significant changes to tooling, architecture, or development practices.

1. Codebase - One codebase tracked in revision control, many deploys
2. Dependencies - Explicitly declare and isolate dependencies
3. Config - Store config in the environment
4. Backing services - Treat backing services as attached resources
5. Build, release, run - Strictly separate build and run stages
6. Processes - Execute the app as one or more stateless processes
7. Port binding - Export services via port binding
8. Concurrency - Scale out via the process model
9. Disposability - Maximize robustness with fast startup and graceful shutdown
10. Dev/prod parity - Keep development, staging, and production as similar as possible
11. Logs - Treat logs as event streams
12. Admin processes - Run admin/management tasks as one-off processes

__HTTP Verbs__

- `GET`: GET requests are used to retrieve resource representation/information only – and not to modify it in any way. GET requests do not change the state of the resource. Additionally, GET APIs should be idempotent, which means that making multiple identical requests must produce the same result every time until another API method changes the state of the resource on the server.

- `DELETE`: DELETE requests are used to delete resources (identified by the Request-URI).

- `POST`: If we wanted to create a new article we would perform a POST to /articles, and our article would be created and given a URI, for example – /articles/1234.

- `PUT`: If we perform a PUT to /articles/1234 and it does not already exist, it will create that resource for me at that URI. I can also perform a PUT to /articles/1234 if it DOES exist, and it will update the resource.

- `PATCH`: PATCH was introduced for partial resource updating. Of course the resource must already exist and the resource's URI known.

- __Idempotence__ Of the major HTTP verbs, GET, PUT, and DELETE should be implemented in an idempotent manner according to the standard, but POST need not be.

__HTTP PUT vs POST__

- When using PUT, we expect to already KNOW the URI of the resource.

- This seemingly minor difference is an important distinction. The HTTP specification considers `PUT to be 'idempotent' while POST is not`. What this means is that I can execute the same PUT request with the same data to /articles/1234 infinite times and expect the same result or side effects. If I execute a POST to /articles with the same data infinite times, I will get infinite new resources (thus POST is NOT idempotent).

- Use POST to create a new resource where I don’t know the URI. Use PUT to create a new resource where I DO know the URI or if I want to update a resource.
