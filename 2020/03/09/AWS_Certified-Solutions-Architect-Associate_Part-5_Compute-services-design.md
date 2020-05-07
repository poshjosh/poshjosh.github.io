---
path: "./2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-5_Compute-services-design.md"
date: "2020-03-09T11:59:58"
title: "AWS Certified Solutions Architect Associate - Part 5 - Compute services design"
description: "Easy to understand tutorial: AWS Certified Solutions Architect Associate - Part 5 - Compute services design"
lang: "en-us"
---

### Compute Services Design ###

Most important area of AWS is storage, Second most important area is compute.
Elastic Compute Cloud (EC2) is a service that allows you to run virtual servers
(machines) in the cloud.

#### Elastic Compute Cloud (EC2) Overview ####

EC2 instance:

- Virtual machine in the cloud. A virtual machine emulates software.
- Pay as you go model
- Integrates with storage, networking and security. Use the instance to access
storage, utilize networking and configure security.
- Amazingly FAST to deploy
- Operation Systems
  * Windows 2003 R2 - 2016
  * Amazon Linux
  * Debian
  * SUSE
  * CentOS
  * Red Hat Enterprise Linux
  * Ubuntu
  * You could build your own AMI, but the above are supported by Amazon support.

Benefits of EC2

- Time to market
- Scalability
- Reliability
- Security
- Control
- Services Integration

EC2 Deployment

- Select an AMI
- Configure networking and security
- Choose the instance type, considering performance and cost etc
- Choose Availability Zone (AZ)
- Attach Storage - Elastic Block Store (EBS)
- Start the instance

#### EC2 Instance Types ####

Match the goal of use with the class type of EC2 instance

- General Purpose - T2, M5, M4 and M3
  * Balance of memory and network resources
  * Good for servers which are not very busy
  * T2 provides burst performance. While your instance is idle, credits accrue.
  When extra load comes the credits are used to burst performance.
  * M3, M4, M5 good for development environment

- Compute Optimized - C5, C4, C2. For CPU/compute intensive apps
  * Media coding
  * Intensive batch jobs
  * Many concurrent users
  * Gaming servers

- Memory Optimized - X1e, X1, R4, R3 - High memory requirments
  * Processing large data sets
  * In-memory databases
  * Big data processing

- Storage Optimized - H1, I2, D2 - High sequential read/writes to local storage
  * Relational databases
  * Data warehousing
  * Image storage and processing

Note: There is an overlap of the use cases of memory optimized and storage optimized
You can choose a memory optimized and impl an EBS volume that gives storage optimization

- Advanced Computing - P3, P2, G3, F1. For speciality hardware compute requirements
  * Graphics Processing Unit (GPU)
  * Field-Programmable Gate Array (FPGA)

For example, penetration testing with hash cracking -> You use CPU + GPU to
increase speed.

#### EC2 Pricing ####

Pricing changes periodically.

3 things which impact cost: instance running, storage, network throughput.

- Pricing Categories
  * On - demand pricing.
    . Charge for usage time, at a flat rate.
    . Billed in 60 seconds increments rounded up i.e to the next minute.
  * Reserved. Far less expensive than on - demand
    . Reserving usage time in a minimum 1 year increment.
    . Maximum 3 years is the least expensive
    . 3 Categories: No, partial, all up front cost.
  * Spot
    . Bid on Amazon's unused compute time.
    . Could give savings of up to 90%
    . Could be used for batch jobs when most other users services in AWS are
    less busy.
  * Dedicated Host

aws.amazon.com/ec2/pricing

#### Elastic Block Store (EBS) and ECS ####

Not all EC2 instances are EBS optimized.

Elastic Block Store (EBS)

- A persistent block storage. Changes last after restart

- Requires EBS optimized instance

- Magnetic (slower less expensive) or SSD (faster more expensive)

### Compute Services Implementation ###

#### Launching an EC2 Linux/Windows Instance ####

Elastic beanstalk automates launching of instances. Will be discussed later.

- Management Console -> Services -> EC2
- Launch Instance
- Choose the AMI
- Choose an Instance type
- Choose the number of instances
- Add specific storage required e.g General purpose SSD, Magnetic etc
- Deselect ```delete on termination```
- Add tags
- Notes
  * Placement group lets you implement redundancy, high networking thoughput etc
  * You get basic monitoring at no extra charge. Detailed monitoring costs.
  * T2,T3 Unlimited option enables bursting.
  * You can add a start up script that runs any time the instance loads
  * To be able to encrypt the root volume of the instance, you need to have selected
  an instance which supports encryption.
  * Even if the AMI does not support encryption, you can still encrypt add-on volumes.
- Configure Security Groups.
  * Default for linux is to allow SSH for remote access
  * Default for Windows is to allow Remote Desktop Protocol (RDP) for remote access
    . Limit to specific remote IP addresses that could connect remotely
- Use a key pair to SSH into the AMI instance
  . Choose existing or create new
  . You could share key pairs between linux and windows instances

Notes

- Windows AMIs more disk intensive than linux

#### Configuring an EC2 Linux Instance ####

The key pair is a PEM file

After launching an EC2 instance access it via its

- Public DNS e.g ```http://ec2-3-19-159-115.us-east-2.compute.amazonaws.com```
- Public IP e.g ```http://3.19.159.115```
For example if you have tomcat installed for port 8080 access the tomcat service
via the address: ```http://3.19.159.115:8080```

The Public IP might change but the DNS address will not ???
Except you request a static IP address ???

If you want the public to access the website easily then I need to get a DNS
hostname to associate with the public IP address. Exammples of a host names
are ```google.com```, ```yahoo.com``` (obviously already owned by google, yahoo)

- To connect to your EC2 instance from your local machine, use an SSH client with
the PEM file.
- PEM files supported by SSH client in linux
- Putty (SSH Client for windows) does not support PEM files.
- Use Putty Gen will convert the PEM file to a PPK file which is supported by putty
  * Launch Putty Gen
  * Click load
  * Browse to where the PEM file and select 'all files' to view files of type PEM
  * Choose the PEM file and click open. Putty Gen warns you that the file needs to
  be saved as a private key.
  * Click OK to save as private key
  * Use a passphrase to save the private key (as a level of protection)
  * Save the key as a PPK file
- Use Putty to connect to your EC2 instance
  * Open putty
  * Paste the IP address
  * Go to SSH section
  * Browse for the ppk file
  * You could browse to main session and save the current session, with a name
  * Click on open
  * Enter the default user ```ec2-user```
  * When you enter the linux machine you are in a linux shell and you could run
  commands like:
    . ```ls```
    . ```apt update``` for Ubuntu, Debian and ```yum update``` for CentOS,

#### Tenancy Models ####

- Tenancy model default setting is shared tenancy
- The tenancy model is selected during launch
- Shared Tenancy, Dedicated Host, Dedicated Instance

#### Shared Tenancy ####

Shared Tenancy

- Multiple customers share the time and space on the physical machine
- Default instance behaviour
- Pros
  * Reduced cost
  * Simpler deployment
- Cons
  * Lower performance
  * Less control (E.g to meet some regulatory standards require more control)

#### Dedicated Host ####

Dedicated Hosts

- Physical machine is dedicated to you with the virtualization layer in place.
You can run any virtual machine.
- Used by one customer
- Must be explicitly configured
- Not available in the free tier
- Pros
  * More accurate licensing management (Some licenses are based on number of
  machines, others are tied to particular machines)
  * More detailed reporting
  * More control for compliance management
  * Determine host placement during instance restarts
- Cons
  * Costs more  

Bring Your Own License (BYOL)- Import a VM (e.g HyperV or VMWare ) into Amazon
EC2 using the import image CLI command

#### Dedicated Instance ####

Dedicated Instance:

- Physical machine
- Only instane on the physical machine
- On restart many be move to another instance
- Pros
  * Runs on hardware dedicated to the customer
  * Provides peformance advantage of a dedicated host
- Cons
  * Does not allow placement determination
  * Less accurate licensing management

#### Difference between Dedicated Host and Dedicated Instance ####

- Dedicated host = one particular physical machine
- Dedicated instance = one dedicated physical machine (not particular)
- On restart, dedicated instance may be re-launched on another machine. Whereas
dedicated host is always re-launched on the same machine.

#### AMI Virtualization ####

Amazon Machine Image (AMI)

- A blueprint with server configuration details
- Similar to localized imaging solutions
- The term instance indicates the use of an AMI
- All instances are created from an AMI
- Sources of AMIs
  * Amazon (free)
  * AWS Marketplace (free/paid)
  * Community (free) built by other users

Who can launch an AMI

- Public -> Anyone can use/launch
- Explicit -> Specific individuals can use/launch
- Implicit -> Only the owner can use/launch
- Default is implicit

AMI Creation

- Use existing AWS AMI
- Customize existing AMI
- Create one from scratch e.g build entire VM in VMWare, import and save as AMI
- Use existing VMs from public sources (Not recommended)

How Virtualization Works

- Hardware Vitural Machine (HVM)
  * AMIs fully virtualizes hardware
  * Requires hardware assisted virtualization (AMDv / Intel VT etc)
- Paravirtual (PV)
  * Run on hosts without specific support for virtualization
  * Doesn't perform as well as HVM AMIs

Instance Root Volume

Every instance needs a root volume.

- One volume in your instance that contains your boot sector.
- Boot sector initiates the boot loader
- Boot loader launches the OS

Root volumes could used different storages

- Instance store-backed AMI
  * Root volume stored in S3 bucket
  * No support for stop action
  * On failure data is lost. Non-persistent. You are always terminating

- EBS backed AMI
  * Root volume stored in EBS
  * Support for stop action
  * On failure data is not lost. Persistent.

### Compute Services Management ###

#### Instance Management ####

To get instances up and running quickly:

- Bootstrapping - providing code/script to be run on instance launch. E.g to install
updates or required software.
- VM Import / Export - import existing VM into EC2
- Instance Metadata
  * Security groups
  * Instance ID
  * Instance type
  * AMI base of the instance
  * Custom meta data through the use of tags

To manage various properties of an instance

- Give your instance a descriptive name
- Click an instance -> Description Tab -> AMI ID to view AMI ID
- You could change the instance type: Stop the instance, change the type
and start it again.
- Change security groups on the fly
- Activate termination protection

#### Connecting to Instances ####

Manage the windows instance without CLI but GUI like provided by RDP

- Management Console -> Services -> EC2
- Right click an existing EC2 windows instance
- Click connect. This doesn't connect you but provides file for connecting
- Download the remote desktop file
- Get your password. Usually involves browsing to your PEM file and decrypting
your password.
- Finish up
- Go to your desktop and click on the RDP file
- Enter your password earlier retrieved

#### Workign with Security Groups ####

Security groups

- Limited to 5 per instance
- Could be layered
- Instances receives the default security group for a VPC
- Other security groups may be attached.
- Default security group may be detached

Difference between Security Group and Network ACL

Security Group 						| Network ACL
----------------------------------------------------------------------------------------------------------------
- Operate at the instance level                        	| - Operate at the subnet level
Attach to network interface associated with a network	| Attach to the subnet
- Supports allow rules only				| - Supports allow and deny rules
- Stateful: Return traffic is automatically allowed,	| - Stateless. Return traffic must be explicitly allowed
regardless of any rules					|
- We evaluate all the rules before deciding whether to	| - We process rules in number order and stop as soon as
allow traffic						| there is a rule that matches
- Applies to an instance only if explicitly associated  | - Automatically applies to all instances in the subnet
either during launch of the instance or thereafter.	| it is associated with.
----------------------------------------------------------------------------------------------------------------

Public  -> VPC[ internet gateway -> network ACL -> subnet[subnet 1 -> -> security group -> instance 1, instance 2] ]
Private -> VPC[ VPG		 -> network ACL -> subnet[subnet 1 -> -> security group -> instance 3, instance 4] ]

![AWS Security Diagram](https://docs.aws.amazon.com/vpc/latest/userguide/images/security-diagram.png "AWS Security Diagram")

Constraints with Security Groups

- Only allow rules are permitted
- Separate inbound and outbound rules
- Stateful. No inbound traffic allowed unless a request was earlier made.
- By default all outbound traffic is allowed
- By default security groups are only bound to the primary network interface
- Could be bound to other network interfaes including ENIs

#### Lab: Working with Security Groups ####

- Management Console -> Services -> EC2 -> Security Groups
- Click Create Security Group
- Give the Security Group a name
- Give a description e.g Allows HTTP, HTTPS and SMTP
- Select VPC

Setup rules
- Click on add rule
- Select HTTP, it sets the protocol and port
- Leave source of 0.0.0.0/0 -> any source

Add another rule
- Select HTTPS, it sets the protocol and port
- Leave source of 0.0.0.0/0 -> any source

Add another rule
- Select SMTP,  it sets the protocol and port
- Leave source of 0.0.0.0/0 -> any source

Security groups are associated with instances but could be applied to more than one

Since security groups are allow only, then you don't have to worry about conflicts
between multiple security groups applied to one instance.

Security is by default deny only until allowed. i.e it is closed to open.

You could then add the above security group to any instance which you want to
allow HTTP, HTTPS and SMTP

#### Elastic Container Service (ECS) ####

Elastic Container Service (ECS) - A way to implement docker containers within AWS
With ECS apps could be launched without deploying EC2 instances directly. The
concept of microservices is supported by ECS

- No virtual machine builds required
- Uses Amazon Fargate to automatically build environments
- You could use EC2 instances for more control

Container Usage

Multi (N) tier applications

- Webserver
- Application server
- Messagequeue server
- Backend worker processes

Management Console -> Compute -> ECS

- Work with clusters of servers running containers
- Task definitions
- Build repositories of containers
- Build out deployment of containers that could be scaled out

More covered in developer certification track

#### Elastic Beanstalk Environment ####

Concept - With very little effort you build complex and large systems.

You cannot change the environment tier after creating an environment

- Management Console -> Compute -> Elastic beanstalk
- Click get started
- Name
- Platform - Docker, Multi-container docker, Go, .NET, Java, Nodejs etc
- Application code -> Sample or existing code

When you create, Elastic Beanstalk does much including:

- Create the environment
- Create S3 storage bucket
- Create Security Group
- Create Elastic IP
- Launch EC2 instance
- Make sure correct instance of platform is installed

### Notes ###

- You can have scripts launch an EC2 instance when needed
- aws.amazon.com/ec2/pricing
- Default user for amazon provided AMIs is ```ec2-user```
- Windows instances require more processing power
- Windows instances use Remote Desktop Protocol (RDP)
- BYOL can save a lot of cost by using existing license for software
- Paid AMIs available from Amazon Marketplace
- When you launch a linux instance, by default the security group it creates
allows incoming SSH communications
- security groups starting with launch wizard are usually created by default
during EC2 instance launch
- With Elastic Beanstalk, you cannot change the environment tier after creating
an environment.

User name for each AMI

    For Amazon Linux 2 or the Amazon Linux AMI, the user name is ec2-user.

    For a CentOS AMI, the user name is centos.

    For a Debian AMI, the user name is admin or root.

    For a Fedora AMI, the user name is ec2-user or fedora.

    For a RHEL AMI, the user name is ec2-user or root.

    For a SUSE AMI, the user name is ec2-user or root.

    For an Ubuntu AMI, the user name is ubuntu.

    Otherwise, if ec2-user and root don't work, check with the AMI provider.



### Questions ###

- List 5 differences between security groups and Network ACL

### Acronyms ###

- EC2- Elastic Compute Cloud
- AMI - Amazon Machine Image
- AZ - Availability Zone
- EBS - Elastic Block Store
- RDP - Remote Desktop Protocol
