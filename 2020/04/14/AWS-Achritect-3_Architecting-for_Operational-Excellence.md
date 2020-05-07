---
path: "./2020/04/14/AWS-Achritect-3_Architecting-for_Operational-Excellence.md"
date: "2020-04-14T11:48:00"
title: "AWS Achitect 3 - Architecting for Operational Excellence"
description: "Game changing tutorial on AWS - Architecting for Operational Excellence"
tags: ["AWS", "tutorial", "architect", "operational", "excellence", "infrastructure as code", "operations as code", "CloudFormation", "AWS developer tools", "CodeCommit", "CodeDeploy", "CodePipeline", "AWS Systems Manager (SSM)", "LifeCycle Hooks", "CloudWatch Events", "AWS Config"]
lang: "en-us"
---

### Acronyms ###

- EC2 - Elastic Cloud Compute
- IAM - Identity and Access Management
- JSON - Javascript Object Notation
- S3 - Simple Storage Service
- SSM - AWS Systems Manager
- YAML - YAML ain't Markup Language

### Before we set off ###

__About this Article__

- Hi, this article contains game changing information for those who would like
to get AWS Certified Cloud Architect certified.

- It is one of a series of articles.

- This is the third article in this series.

- [Click here for the previous article](/2020/04/14/AWS-Architect-2_Architecting-for-Security).
To start at the beginning of the series, [click here](/2020/04/14/AWS-Architect-1_Architecting-for-Reliability).

- This article is an estimated 18 minute read.

__This Article is not an Introduction to AWS__

This series of articles is not an introduction to AWS or any of the core
concepts of the AWS Cloud. You need to be already familiar with some core
concepts of AWS Cloud to fully benefit from this article. In order words,
if you are not familiar with the AWS cloud, you should read the series of
articles beginning at:

- [Notes on Amazon Web Services - 1](/2020/03/02/Notes-on-Amazon-Web-Services_1_Introduction)

- [AWS Certified Solutions Architect Associate - Part 1](/2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-1_Key-services-relating-to-the-Exam)

### What does it mean to architect for Operational Excellence? ###

- First off, what is operations?

  `Operations`
  : are those ongoing activities necessary to keep your apps and
  infrastructure running properly.

- What is excellence then?

  `Excellence`
  : is the quality of being outstanding or extremely good.

- From the foregoing:

  `Operational Excellence`
  : is the quality of being outstandingly able to keep your apps and
  infrastructure running properly.

- It is no surprise that `Operational Excellence` is one of Amazon's
[5 pillars of a well architected system](https://aws.amazon.com/blogs/apn/the-5-pillars-of-the-aws-well-architected-framework/)
and it involves 6 design principles:

  * Perform operations as code
  * Annotate documentation
  * Make frequent, small, reversible changes
  * Refine operations procedures frequently
  * Anticipate failure
  * Learn from all operational failures

- Operational excellence require some degree of automation.

  * Which tasks need to be automated.
  * The degree of automation
  * How to automate the tasks.

- Achieving Operational Excellence does not require automating everything

- Define your workflows and AWS resources as code, because.

  * Code is repeatable and executes quickly.

  * Code changes can be easily tracked over time.

  * Code serves as defacto documentation.

### CloudFormation ###

__CloudFormation Terminology__

- Stack - AWS resources that you create, update and delete as a single unit.
If you delete a stack, you have effectively deleted all the resources therein.

- Stacks are created using templates which are `JSON` or `YAML` format.

- Templates divided into sections including:

  * Parameters
  * Mappings
  * Resources (only required section)
  * Outputs

- CloudFormation can only read templates from an S3 bucket.

- Stack Policies - Control which resources can be updated and deleted

- Stack Outputs and Exports - Extract info from resources in stacks as well
as pass info between stacks.

- Nested Stacks - A stack created by another stack

- Helper Scripts - Use to signal to signal to C when your EC2 instances are
up and configured the way you want them.

- Here are some useful CloudFormation links:

  * [CloudFormation UserGuide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
  * [CloudFormation Sample Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-sample-templates.html)
  * [CloudFormation Template Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-reference.html)
  * [CloudFormation Intrinsic Function Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html)

### AWS Developer Tools ###

__Generating Credentials for AWS Developer Tools__

To use AWS developer tools your IAM user needs to have credentials for
CodeCommit.

- IAM Console -> Click on the IAM user

- Click on the security credentials tab

- Under signin credentials, next to console password, click manage.

- Select the enable radio.

- Enter the password and click apply.

- Scroll to the bottom of the page. You will find a section for generating
HTTPS git credentials for AWS CodeCommit. Click the generate button.

- Download the generated credentials.

- Scroll back to the top and under Summary, you will notice a console sign-in
link. Click the link

- Enter the IAM username and password as required.

- You are now ready to use the AWS Developer tools.

Adopting and infrastructure as code is essential to operational excellence.

__CodeCommit__

CodeCommit is based on git version control system. Let's you create private
git repositories.

__CodeDeploy__

Deploys and application to EC2 instances.

__CodeDeploy Agent__ is a service that must be running on your EC2 instances
for CodeDeploy to deploy applications to them. The agent does not ship with
Amazon Linux AMIs.

You can use the AWS Systems Manager to push the CodeDeploy agent to EC2
instances.

- Create a command document. which contains command we want to execute on
our instances.

- Exeute the document against our instances.

__IAM Role for CodeDeploy__

CodeDeploy requires an IAM service role. The role will grant CodeDeploy access
to read and modify necessary resources.

__CodePipeline__   

Let's you automate the entire software development process and release process.

CodePipeline needs a service IAM role. It creates one by itself.

CodePipeline can deploy from a CodeCommit repository.

### AWS Systems Manager ###

- The names EC2 systems manager or Simple Systems Manager describe what is now
called AWS Systems Manager (SSM).

- Let's you automate actions against many AWS resources.

- Systems manager works on both EC2 instances and on-premises instances.

- Instances need permissions defined in the AWS managed policy named
`AmazonEC2RoleforSSM`.

__System Manager Agent__

- System manager requires Systems Manager (SSM) agent to be installed.

- SSM agent ships on base images for Amazon Linux, Amazon Linux 3,
Ubuntu Server 16.04 and 18.04 LTS

__Capabilities of AWS Systems Manager__

The following services work together.

- Actions

  * Patch Manager Service - Automate the installation of software
  updates on you instances

  * Ensures all instances have software and configuration setttings
  you define.

- Insights

  * Inventory Manager - Collects software inventory as well as network and
  CPU info from all your instances.

  * Configuration Compliance. Shows you the patching and configuration status
  of your instances.

__Best Practices enabled by AWS Systems Manager__  

- Ensure software is up to update

- Take inventory of software installed on EC2 instances.

- Maintain a consistent configuration state across a fleet of EC2 instances.

__Systems Manager vs CloudWatchEvents + AWSConfig__

System Manager enables you do a lot. However it is schedule based. This means
that we can set actions to take place at specific times. What happens when
we want an action to take place when, for example, a new EC2 instance is
launched? This is where `CloudWatch Events` and `AWS Config` come in.

### CloudWatch Events and AWS Config ###

__Auto Scaling LifeCycle Hooks__

- Auto Scaling LifeCycle Hooks let you perform actions during launch or
termination of an instance. These actions include:

  * Installing an app
  * Patching the instance with latest software updates
  * Pulling logs off the instance.

- LifeCycle Hooks do not exist by defaut.

- Auto Scaling LifeCycle Hooks only applies to instances within an Auto Scaling
group.

- During a LifeCycle Hook, the instances are placed into a wait state for
a finite period of time. During this time, the instance is up and running,
has network access, but Auto Scaling has not yet placed it into the Load
Balancer's target group, hence it cannot receive traffic.

- To view existing LifeCycle Hooks

  * EC2 Dashboard -> Auto Scaling Groups

  * Scroll to the bottom and you will see the LifeCycle Hooks tab at the en

__CloudWatch Events__

- A CouldWatch event can include:

  * Changes to athe state of an AWS resource

  * API call to a resource

  * Management console sign-in

  * Scheduled Event

- __CloudWatch Events Rule__ Lets you take some action in response to an event.

  * Source: Any CloudWatch Event

  * Target: An AWS resource

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

### Takeaways ###

- CloudFormation can only read templates from an S3 bucket.

- Stack Policies - Control which resources can be updated and deleted

- Systems manager works on both EC2 instances and on-premises instances.

- Use automation and change tracking.

- Don't automate what you don't understand.

- Don't automate a bad process.

- You could also use AWS CLI for some process

- Use CloudWatch events for full automation using auto scaling lifecycle hooks
and CloudWatch event rules.

- Use infrastructure as code.

- AWS Systems Manager can help enable operations as code.

- Ensure software is up to update

- Take inventory of software installed on EC2 instances.

- Maintain a consistent configuration state across a fleet of EC2 instances.

- Use AWS Config for tracking state changes.

### References ###

- [Lexico.com - Definition of Excellence](https://www.lexico.com/en/definition/excellence)

- [AWS - 5 Pillars of Well Architected Framework]([pillar](https://aws.amazon.com/blogs/apn/the-5-pillars-of-the-aws-well-architected-framework/))

- [AWS - Operational Excellence](https://wa.aws.amazon.com/wat.pillar.operationalExcellence.en.html)

- [AWS - Operational Excellence Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Operational-Excellence-Pillar.pdf)

- [AWS Config](https://aws.amazon.com/config/)

- [CloudFormation UserGuide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)

- [CloudFormation Sample Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-sample-templates.html)

- [CloudFormation Template Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-reference.html)

- [CloudFormation Intrinsic Function Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html)
