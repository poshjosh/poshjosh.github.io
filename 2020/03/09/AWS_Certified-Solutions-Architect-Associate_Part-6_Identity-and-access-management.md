---
path: "./2020/03/09/AWS_Certified-Solutions-Architect-Associate_Part-6_Identity-and-access-management.md"
date: "2020-03-09T14:20:50"
title: "AWS Certified Solutions Architect Associate - Part 6 - Identity and access management"
description: "Easy to understand tutorial: AWS Certified Solutions Architect Associate - Part 6 - Identity and access management"
lang: "en-us"
---

### Identity and Access Management (IAM) ###

#### Overview ####

- About managing access to AWS. What people can do within AWS
- Supports users, groups and roles
- Using IAM is free. No charge for user account, roles or groups
- Actions by other users could incur cost for the root user account

Concepts

- Resources are things on which actions can be taken. E.g EC2 intances
- Principals are things which take actions.
  * Users
  * Groups
  * Roles
- Policies - Decide which principals can act on what resources
  * Identity, Group, Resource based policies

![Intro to IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/images/intro-diagram%20_policies_800.png "Intro to IAM Policies")

#### Principals ####

Principals are basically identities.
- Also called identities
- Entity that can perform an action:
  * Users
  * Groups
  * Role
- IAM users are entities created in AWS
- Entities = Person or service with permissions
  * AWS Management Console
  * AWS API/CLI

Users
- user credentials
  * Consists of a name and password and up to 2 access keys
    . Access keys are used with API or CLI
- Users can be members of groups

Groups
- Collection of IAM users
- Permissions should be managed at group level
- Users can be added and removed
- Groups are not used to logged in

Roles - Set of actions that someone, something can take
- An identity that is granted permissions
- Roles aren't permanently assigned
- Assumable by any entity with a need for it
- Compatible with federated users
  * What is Federated Identity?
    . Concept of federated identity means taking existing user account from one
    identity system and given then ability to do something in another identity system.
    . Often implies giving uses Single Sign On (SSO)

Users vs Roles
- Create user accounts when:
  * You are the only person working with the account
  * Multiple people need permanent access
  * One or more users require CLI access
- Create Roles when:
  * Apps need access
  * Mobile phone apps make requests of AWS
  * Existing company users need federated access

- Primay principals in AWS are users, groups and roles
- An entity may also be a service in addition to a person
- Recommended to always use role with CLI access
- Before you create a user account make the decision whether to best create a user
or a role

#### Root User ####

AWS account root user

- Email address used to create the AWS subscription
- Unlimited capabilities (any one who logs on with the root user account)
- Not recommended for everyday access even for the owner
- Avoid giving out root user details, rather create an IAM admin user to give out

Root Access Tasks (Tasks exclusive to root users)
- Modifying the root users
- Changing the AWS support plan
- Closing AWS account
- Creating a CloudFront key pair
- Enabling Multi-Factor Authentication (MFA) on an S3 bucket
- Restore permissions for other IAM users

#### Authentication ####

Basic process of validating identity. Involves making a claim and providing
proof that the claim is true.

- Validation of credentials
- Credentials provide identity
- Single factor proof
- Multi factor proof

Authentication of:
- Persons e.g with user accounts in AWS
- Processes e.g with roles in AWS

Athentication in AWS
- Required to manage AWS
- S3 allows anonymous access
- Usually a username and password
  * Management console
- Access key and secret key (more secure that password)
  * API access
  * CLI access  

#### Authorization Policies ####

Validation of actions

- Rules that determine allowed actions or access
- Used throughout AWS
- Policies could be attached to users, groups, roles, resources
- Uses JSON which could be:
  * Created by GUI
  * Coded directly
- Vary by objects
- Provided by AWS policies
- Identity-based polices
  * Users, group, roles
- Resource-based policies
  * Used for cross-account access (accounts from different AWS subscriptions)

Policy Processing

- By default all requests are denied
- Explicit allow overrides the default
- Permission boundaries can override explicit allows
- Explicit denies override explicit allows. If you are a member of 2 groups A and B
Group A allows you do something and Group B denys you, then the deny is enforced.

Actions of Operations (When a user attempts to take an action)
- Request is authenticated
  * Action or operation is processed
- Request is authorized
  * Linked to a service
- Process against a resource
- Includes CRUD concept
  * Create
  * Read
  * Update
  * Delete

Into AWS Documentations

Title: Actions, Resources and Condition Keys for AWS Services

Under EC2
- Defines action that could be taken on EC2 instances
- Resources Defined by EC2 e.g DHCP options

Use the above documentation as a guide.

#### Multi-Factor Authentication (MFA) ####

- Best Practice
- Couple username and password with another factor, Something you
  * Know
  * Have
  * Are
  * Receive (e.g SMS MFA)
- Can be enabled for the root user as well as other user accounts
- Free types are SMS and Virtual token (Open standard OTOP)

#### Key Rotation ####

- Best practice suggests rotating keys:
  * Access key ID
  * Secret access key
- Key rotation only applies to user accounts
- Not all accounts needs these keys e.g account which does not API or CLI access

Key Rotation Process

- Create a second access key in addition to the one in use
- Update all your applications to use the new access key and validate that the
applications are working
- Change the state of the previous access key to inactive
- Validate that all your applications are still working as expected
- Delete inactive access key

CLI is efficient for doing key rotation

- List access keys
```
aws iam list-access-keys --user-name <PUT_YOUR_USER_NAME_HERE>
```

- Create access key
```
aws iam create-access-key --user-name <PUT_YOUR_USER_NAME_HERE>
```

#### Multiple Permissions ####

- User permissions
- Group permissions
- Allow policies are cumulative
- Deny policies are overriding. If your user policy allows an action but a group
you belong to denys you that action. Then the deny is enforced.
- Boundaries allows you to go beyond typical group and user environment
  * Don't actually give permission, but sets the boundary.
  * Limit user to specific services
- Boundary does not give permission. Boundary defines boundary to take effect when
permission is given.

#### AWS Compliance Programs ####

Live up to defined standards.

- Browse to: ```aws.amazon.com/compliance```
- Click the: ```view our compliance program``` link

AWS lists common compliance programs for global and for various regions e.g:
- Payment Industry - PCIDSS Level 1
- ISO 90001:2015 Compliance

#### Shared Responsibility Model ####

AWS

- Provides security of the cloud
  * Physical
  * Network - The actual network, not e.g the Elastic Network Interface (ENI)
  * Hypervisor
  * Managed Services (DynamoDB, Aurora etc) Services not managed by you directly

You

- Provide security in the cloud
  * Guest OS
  * Application
  * User data

AWS Shared Responsiblity Model

![AWS Shared Responsiblity Model](https://d1.awsstatic.com/security-center/Shared_Responsibility_Model_V2.59d1eccec334b366627e9295b304202faf7b899b.jpg "AWS Shared Responsiblity Model")

### IAM Best Practices ###

#### User Accounts ####

Create an admin account

- Browse to: Management Console -> Services -> Security Identity & Compliance

- Administrator access is an existing policy in AWS. By attaching that policy to
a group and put people in that group they become admin users

- Click on Add user
- Enter username and password
- Select management console access
- Require password reset when creating an account for users other than yourself.
- Set permissions
  * Add user to group
  * You can copy permissions from anotheruser
  * Attach existing policies directly
    . Here you can select administrator access to make this user an administrator.
    . Remember you could also make the user an admin by adding that user to a
    group with admin access.
- Set permission boundaries
- Review entries
- Click on create user
- You could download a CSV file with information on the newly created user.
- The file contains a unique URL the user could use to login
- Newly created users need their console link in order to login to AWS

First create the user as above and then create an access key for API, CLI access

- Under an existing user
- Go to security credentials -> Access key -> Create access key
- Download CSV file containing access key ID, and secret access key

If the user needs to use SSH you could upload SSH public keys for the user

- Under ```console password``` you see if the user has ever signed in. You could
remove all users who have never signed in.

#### Password Policies ####

Default Password Policies
- Minimum of 8 passwords
- Maximum of 128 characters
- At least 3 of 4 character types
  * Upper case
  * Lower case
  * Numbers
  * Special characters e.g _, !
- Password cannot be the same as account name or email

Password Best Practices

- Change password periodically
- Use unique password for AWS
- Avoid easily guessed passwords

Custom Password Policy

- Management Console -> Services -> Security Identity & Compliance -> IAM -> Account Settings
- Click Password Policies, at the top
- Select the various rules like length, expiration etc
- Click apply password policy
- Policies apply to new or changing passwords

#### Credential Rotation ####

- Reduces vulnerability

- Management Console -> Security Identity & Compliance -> IAM -> Account Settings
  * Enable password expiration
  * Prevent password reuse, Number of passwords to remember
    . Send to 13 rather than 12 to prevent synchronized rotation with months of
    the year which may cause users to keep One password for each month of year.
  * Password expiration requires administrator reset. If password expires before
  being changed, then admin must reset.

#### Principles of Least Privilege ####

- Grant only the access needed
- Review granted access periodically
- To aid review AWS has summaries
  * Policy summary - Applies to services
  * Service summary - Applies to actions
  * Action summary - Applies to resources
- Policy summary could be used to see the cummulative permissions a user has
- Management Console -> Services -> Security Identity & Compliance -> IAM
- Select a User
- Click on the expansion arrow by the Policy
- Greyed out button should not deter you -> the button toggles with the next which
is a JSON button
- Click show one more and more will be displayed if available
- If both buttons are greyed out then you may need to click on the link to go
to the owner of the policy from where both buttons will not be greyed out.

#### IAM Roles ####

- Management Console -> Security Identity & Compliance -> IAM -> Account Settings
- Click roles by the left
- Click Create role
  * AWS Service - Allow an AWS service to take action on other AWS service. For
  example an EC2 instance may have this role to carry out actions on other services
  * Another AWS Account
  * Web Identity - E.g Single Sign On ID provider like Cognito etc
  * SAML 2.0 Federation - Allow users from a different Identity Management Service
- Click AWS Service
- Click EC2 instance
- Attach permission policies -> E.g Select Amazon S3 full access
- Set permission boundaries
- Enter role name e.g ```EC2_access_S3```
- Best practice is never make a role an administrator.
- Review
- Click on Create Role

#### Policy Conditions ####

- Management Console -> Security Identity & Compliance -> IAM
- Select Policies -> Create Policy
- Select service to apply policy to e.g S3 service
- Set actions
- Level of access e.g list, read etc
- Choose the bucket resources -> e.g all resources
- Conditions. Defaults are:
  * MFA required
  * Source IP
- Add Condition
  - E.g Time, Referrer, Prefix on S3 object
  - Qualifier -> Any value
  - Operator -> E.g string like
  - Value -> Marketing
- Enter Name e.g S3_list_and_read_on_string_like_marketing
- Enter Description e.g Why created
- Click Create Policy

#### CloudTrail ####

CloudTrail is the logging solution in AWS, it gives:

- Event histories of
  * Management console
  * AWS SDK
  * CLI actions
  * Additional AWS Services

- You can setup CloudTrail and define an S3 bucket for storage

- A log of CloudTrail events is delivered to S3 bucket and optionally to CloudWatch
logs and CloudWatch events.

- CloudWatch is our event notification system.

How CloudTrail Works

![How CloudTrail Works](https://i1.wp.com/d1.awsstatic.com/products/CloudTrail/Cloudtrail_How-it-works_final.3fa7690040cb1b8fb5fa4f669b8a0148f370a76f.png "How CloudTrail Works")

Create Cloud Trail

- Management Console -> Management Tools -> CloudTrail
- By default event histories stay for only 90 days
- When we create a trail we can store the history in S3 bucket as long as we want
- Click create trail
- Select write, read etc events
- Select create new S3 bucket
- Advanced include log prefix, encryption, log file validation etc

CloudTrail S3 Bucket Names are more demanding than normal S3 bucket names
- Must be globally unique
- Must not contain underscore
- etc

Due to the above, it is recommended to:
- First create an S3 bucket
- Use the existing bucket for CloudTrail

### Notes ###

- Primay principals in AWS are users, groups and roles
- An entity may also be a service in addition to a person
- Recommended to always use role with CLI access
- Before you create a user account make the decision whether to best create a user
or a role
- Management console access - Username and password
- API access - Access key and secret key
- CLI access - Access key and secret key
- Roles are temporary and do not require rotation like keys
- Allow policies are cumulative
- Deny policies are overriding. If your user policy allows an action but a group
you belong to denys you that action. Then the deny is enforced.
- Permissions can come from multiple sources
- Newly created users need their console link in order to login to AWS. The link
is sent as a CSV file when you create the user.
- Policy summary could be used to see the cummulative permissions a user has

### Acronyms ###

- SSO - Single Sign On
- CRUD - Create, Read, Update, Delete
- MFA - Multi-Factor Authentication
