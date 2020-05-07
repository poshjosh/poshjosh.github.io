---
path: "./2020/04/03/Questions_AWS-Certified-Cloud-Architect-Associate.md"
date: "2020-04-03T22:02:48"
title: "Questions and Answers - AWS Certified Cloud Architect Associate"
description: "Questions and Answers - AWS Certified Cloud Architect Associate"
tags: ["AWS", "Certification", "Certified Cloud Architect", "Questions", "Answers"]
lang: "en-us"
---

### Questions ###

1. What is the maximum number of vaults an AWS account can create in a region?

2. A solutions arch is designing a highly scalable system to track patient records
Records must remain available for immediate download:

  a. Store files in EBS, create lifecycle policy to move them to Glacier Glacier after 6 months

  b. Store files in Glacier, create lifecycle policy to move them to Glacier S3 after 6 months

  c. Store files in S3, create lifecycle policy to move them to Glacier Glacier after 6 months

  d. Store files in EFS, create lifecycle policy to move them to Glacier after 6 months

3. What is longest duration of an SWF workflow execution
  - 12 months
  - 30 days
  - 364 days
  - 10 days

4. Which services provide full administrative control of EC2 instances
   - Elastic Beanstalk
   - RDS
   - MapReduce
   - LightSail
   - DynamoDB
   - ElasticCache

5. Standard retrieval of S3 Glacier data typically completes between.
   - 1 - 24 hours
   - 3 - 5 hours
   - 1 - 5 hours
   - 5 - 24 hours              

6. Which of the following are true?
  - Transfer Acceleration is only supported on virtual style requests.
  - Transfer Acceleration is only supported on path style requests.
  - Transfer Acceleration is supported for both virtual and path style requests.
  - The name of the bucket used for Transfer Acceleration must be DNS-compliant and must not contain periods (".").

7. Which of the following 3 API actions in AWS STS return temporary security
credentials with a default expiration time of one hour
   - GetFederationToken
   - AssumeRole
   - AssumeRolewithSAML
   - AssumeRoleWithWebIdentity
   - GetSessionToken

8. Which of the following are true

   - S3 One Zone Infrequent Access does not support SSL
   - S3 Intelligent-Tiering accrues a small monthly monitoring and auto-tiering fee
   - S3 Glacier provides three retrieval options that range from a few minutes to hours.
   - Data stored in S3 One Zone Infrequent Access will be lost in the event of
   Availability Zone destruction.

9. Database requires occasional internet connection to download system and
database updates
  - Db in private subnet
  - Db in public subnet
  - NAT instance in public subnet and route internet bound traffic to NAT from
  private subnet
  - NAT instance in private subnet and route internet bound traffic to NAT from
  private subnet

10. Which is true. S3 supports
   - Eventual consistency for overwrite PUTS and UPDATES
   - Eventual consistency for overwrite PUTS and DELETES
   - Read after write consistency for PUTS of new objects in all regions
   - Read after write consistency for PUTS of new objects in US regions

11. Requirement to host a database on an EC2 instance. The storage option chosen
must support 28,000 IOPs

  - EBS Provisioned IOPS SSD

  - EBS Throughput Optimized HDD

  - EBS General Purpose SSD

  - EBS Max IOPS SSD

12. An application is being designed for deployment into AWS. The application will
use Amazon S3 buckets for storing as well as reading data. The write traffic
is expected to be 6,500 requests per second and the read traffic will be
around 8,000 requests per second.

  What is the best way to architect the solution for maximum Amazon s3
  performance?

  -  Use as many s3 prefixes as you need in parallel to achieve the required
  throughput.

  - Prefix each object name with a hex hash key along with the current date.
  Make the keys distinctive.

  - Enable versioning on the S3 bucket.

  - Setup cross region replication on the bucket and preform reads from the
  secondary bucket.

13. Which AWS network feature gives low latency and high packet per second network performance
  - Amazon Hypervisor
  - Security Group
  - Amazon HVM
  - Placement Group

14. A company has an application hosted in AWS. The application is deployed on a
set of Ec2 instances across two AZs for high availability. The infrastructure
is deployed behind a application load balancer.

  The following are requirements from and administrative perspective.

  - Ensure notifications are sent when the read requests exceed 100 per minute.

  - Ensure latency exceeds 15 seconds

  - Any API activity which calls sensitive data must be monitored.

  Which of the following meets the requirements? Choose 2.

  a. Use CloudTrail to monitor API activity.

  b. Use CloudWatch to monitor API activity.
  Not used to monitor API activity.

  c. Use CloudWatch metrics to create custom metrics and setup an alarm
  to send out notifications when the threshold is reached.

  d. Use custom log software to monitor latency and read requests to the
  application load balancer.

15. An EC2 instances hosts a voting application that accesses DynamoDB table.
The instance needs to be able to access the table in the most secure way
possible.

  Which of the following is the most secure way for the EC2 instance to
  access the DynamoDB table?

  - Use KMS keys with permissions to interact with DynamoDb and assign those
  keys to the applications.

  - Use an IAM user account that is designated as a service account to ensure
  minimum required credentials and assing to the instance.

  - Use an IAM role with permissions to interact with DynamoDB and assign it
  to the EC2 instance.

  - Configure a VPC gateway endpoint to allow the resources to access
  DynamoDB

  __Note__: _Always choose a role over a user account_.

16. Where to get info like timestamps, client ip, latencies, request paths from
load balancers.
  - Metrics from CloudWatch
  - Access Logs from the web servers
  - Access Logs from the load balancers
  - Metrics from CloudTrail

17. A company has a workflow that sends video files from their datacenter into
the cloud for transcoding. They are using EC2 instances to pull transcoding
jobs from SQS.

  Why is SQS the best choice for creating a decoupled architecture?

  - SQS guarantees the order of messages.

  - SQS checks the health of the worker instances.

  - SQS makes it easier to carry out horizontal scaling of the encoding tasks.

  - SQS synchronously provides transcoding output.

18. Connecting to EC2 via putty receives 'Connection timed out' error. What
possible causes? Check the:
  - Role attached to EC2 instance
  - Security Group rules
  - Private/public keys
  - Route table for the subnet
  - Username/password
  - Network access control list

19. Which S3 encryption method could be used for data assuming you do not want
to manage the encryption keys yourself?

  - SSE-S3
  - SSE-C
  - SSE-KMS
  - SSE-KMS with CloudHSM

20. An RDS MySQL database is getting lots of read and has become the bottleneck
for the application. What action can be peformed to ensure that the
database does not remain a bottleneck.

  - Setup CloudFront distribution in front of the database.
  CloudFront in front of a database is not a typical architecture.

  - Setup an Elastic Load Balancer in front of the database.
  Load Balancers sit in front of application and web servers not database.

  - Setup an ElastiCache cluster in front of the database.

  - Setup SNS in front of the database

21. Default visibility time for a queue in SQS
  - 12 hours
  - 30 secs
  - 1 day
  - 1 hour

22. Custom application with 200GB MySQL database runs on an EC2 instance.
The application is only being used for short periods of time in the morning
and sometimes in the evening.

  What is the most cost effectic storage type?

  - Amazon EBS provisioned IOPS SSD.

  - Amazon EBS Throughput Optimized HDD

  - Amazon EBS General Purpose SSD

  - Amazon EFS

23. Which of the following are true?

  - Default max amount of bucket is 100

  - Default max amount of objects in bucket is infinity (theoretically)

  - AWS Inspector is an automated security assessment tool which needs no agent
  installed on target instances.

  - AWS Systems manager uses an event based architecture

24. ADD QUESTION HERE

25. A consultant designs large scale architectures using several AWS services
that include IAM, EC2, RDS, Dynamo DB and VPC. The consultant would like
to take his designs and make them easier to deploy to AWS, that is, in a more
automated manner.

  Which service would best meet the requirement?

  - Elastic Beanstalk.

  - CodeDeploy

  - CloudFormation

  - OpsWorks

26. ADD QUESTION HERE

28. A database application running on an EC2 instance needs to get updates from
the internet. A solutions architect needs to design a solution to get the
updates without exposing the instance to the internet.

  Which solution best meets these requirements?

  - Attach a VPC endpoint and add routes for 0.0.0.0./0

  - Launch a NAT gateway and add routes for 0.0.0.0./0

  - Deploy a NAT instance in a public subnet and add routes for 0.0.0.0./0

  - Attach an internet Gateway and add routes for 0.0.0.0./0

29. ADD QUESTION HERE

30. A solutions architect is designing a system which needs a minimum of 8
m5.large instances to serve traffic. The system will be deployed in us-eas-1
and needs to be able to handle the failure of an entire availability zone (AZ).

  Assume all instances properly linked and you can use AZs `a` through `f`

  How should you distribute the servers to save as much cost as possible
  while maintaining high availability?

  - 3 servers in each AZ (a - d)
  - 8 servers in each AZ (a and b)
  - 2 servers in each AZ (a - e)
  - 4 servers in each AZ (a - c)

### Answers ###

- [Click here](/2020/04/03/Answers_AWS-Certified-Cloud-Architect-Associate) for
the answers to these questions.
