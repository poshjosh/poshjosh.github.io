---
path: "./2020/03/10/Jenkins-on-AWS_Setup.md"
date: "2020-03-10"
title: "Jenkins on AWS - Setup"
description: "Easy to understand tutorial: Jenkins on AWS Setup"
lang: "en-us"
---

### Prerequisites ###

To prepare for this tutorial, you will need:

- An AWS account.
- An AWS Identity and Access Management (IAM) user with access key ID and secret access key.
- An Amazon EC2 key pair.
- A configured Virtual Private Cloud (VPC)

The foregoing requirements are beyond the scope of this article. Please refer to the relevent AWS Documentation.

- Login to AWS

- I logged in with an admin user

### Create a Security Group for Your Amazon EC2 Instance ###

- Management Console -> Compute -> EC2
- Network and Security -> Security Groups -> Create Security Group
- Enter a name
- Choose the VPC
- On the Inbound tab, add the rules as follows:
  * Click Add Rule, and then choose SSH from the Type list. Under Source, select
  Custom and in the text box enter the public IP address range that you decided
  on in step 1.
  * Click Add Rule, and then choose HTTP from the Type list.
  * Click Add Rule, and then choose Custom TCP Rule from the Type list. Under
  Port Range enter 8080.

Note: for the SSH, because my IP is not static, I specified a CIDR block deduced
from my ip:

- My ip format 111.222.333.444
- My CIDR block format: 111.222.0.0/16 (Gives a range)
- Or browse for ```ipv4 to CIDR converter```, I got [this link](https://www.ipaddressguide.com/cidr) which I used to convert my ip to a CIDR block

### Launch Your EC2 Instance ###

- In the left-hand navigation bar of the Amazon EC2 console,  choose Instances,  and then click Launch Instance.

- On the Choose an Amazon Machine Image page, select Free tier only,  and then select an Amazon Linux AMI with the HVM virtualization type.

- I chose an Amazon Linux AMI (an EBS-backed, AWS-supported image. The default image includes AWS command line tools, Python, Ruby, Perl, and Java. The repositories include Docker, PHP, MySQL, PostgreSQL, and other packages)

- [Link to chosen Amazon Linux AMI](https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/)

- On the Choose an Instance Type page, the t2.microinstance is selected by default. Keep this instance type to stay within the free tier.

- Click Next: Configure Instance Details.

- On the Configure Instance Detailspage, do the following:

  * T2 instances must be launched into a subnet. From Network chooseyour VPC,  and from Subnet choose one of your public subnets.
  * For Auto-assign Public IP,  ensure thatEnable is selected from the list. Otherwise, your instance will not get a public IP address or a public DNS name.
  * Click Review and Launch. If you are prompted to specify the type of root volume, make your selection and then click Next.

- On the Review Instance Launch page, click Edit security groups.

- On the Configure Security Group page:

  * Select Select an existing security group.   
  * Select the WebServerSG security group that you created.
  * Click Review and Launch.

- On the Review Instance Launch page, click Launch.

- In the Select an existing key pair or create a new key pair dialog box, select Choose an existing key pair or create new.

- Click the acknowledgement check box, and then click Launch Instances.

- In the left-hand navigation bar, choose Instances to see the status of your instance. Initially, the status of your instance is pending. After the status changes to running, your instance is ready for use.

### Connect to Your Instance from Windows Using PuTTY ###

Download the Putty client for windows

- From the Start menu, choose All Programs > PuTTY > PuTTY.

- In the Category pane, select Session,  and complete the following fields:
  * In Host Name, enter ec2  -user@public_dns_name
  * Ensure port is 22
- In the Category pane, expand Connection, expand SSH, and then select Auth. Complete the following:
  * Click Browse
  * Select the .ppk file that you generated for your key pair.
- Go back to sessions tab
- Under saved sessions section enter a session name and click save
- Click open to start the session connected to your AWS EC2 instance.

### Connect to Your Instance from Linux or Mac OS X Using SSH ###

- Use the ssh command to connect to the instance. You will specify the private key (.pem) file and ec2-user@public_dns_name
```
$ ssh -i /path/my-key-pair.pem ec2-user@ec2-198-51-100-1.compute-1.amazonaws.com
```

- Sample response
```
The authenticity of host 'ec2-198-51-100-1.compute-1.amazonaws.com (10.254.142.33)' can't be established.
RSA key fingerprint is 1f:51:ae:28:bf:89:e9:d8:1f:25:5d:37:2d:7d:b8:ca:9f:f5:f1:6f.
Are you sure you want to continue connecting (yes/no)?
```

- Enter yes

- Sample response
```
Warning: Permanently added 'ec2-198-51-100-1.compute-1.amazonaws.com' (RSA) to the list of known hosts.
```

```
sudo yum update –y
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
sudo yum install jenkins -y
sudo service jenkins start
```

- To ensure that your software packages are up to date on your instance, perform a quick software update
- Add the Jenkins repo
- Import a key file from Jenkins-CI to enable installation from the jenkins repo package
- Install Jenkins
- Start Jenkins as a service

sudo yum update –y

sudo java -version
sudo yum install java-1.8.0-openjdk
sudo update-alternatives --config java
```
There are 2 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
*  1           /usr/lib/jvm/jre-1.7.0-openjdk.x86_64/bin/java
 + 2           /usr/lib/jvm/jre-1.8.0-openjdk.x86_64/bin/java

Enter to keep the current selection[+], or type selection number:
```

Enter ```2``` to select java 8

Update your bash profile file with the ```JAVA_HOME``` variable:

```
sudo vim .bash_profile
```

Example: _My .bash_profile file_
```
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk.x86_64

PATH=$PATH:$HOME/.local/bin:$HOME/bin:$JAVA_HOME/bin

export PATH
```

Check the results
```
$ echo $JAVA_HOME
/usr/lib/jvm/jre-1.8.0-openjdk.x86_64/bin/java
```

Install Jenkins from the Long Term Support release line:
```
$ sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
$ sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
$ sudo yum install jenkins
```

Start Jenkins
```
$sudo service jenkins start
```

Jenkins is now installed and running on your EC2 instance. To configure Jenkins:

- Browse to ```http://<your_server_public_DNS>:8080`` from your browser. You will
be able to access Jenkins through its management interface:

![Prompt to Unlock Jenkins](https://i1.wp.com/opensourceforu.com/wp-content/uploads/2017/08/Figure-1-Unlocking-Jenkins.jpg "Prompt to Unlock Jenkins")

- As prompted, enter the password found in /var/lib/jenkins/secrets/initialAdminPassword. Use the following command to display this password:
```		
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

- The Jenkins installation script directs you to the Customize Jenkinspage. Click Install suggested plugins.

- Once the installation is complete, enter Administrator Credentials,  click Save Credentials,  and then click Start Using Jenkins.

- On the left-hand side, click Manage Jenkins,  and then click Manage Plugins.

- Click on the Available tab,  and then enter ```Amazon EC2``` at the top right.

- Select the checkbox next to Amazon EC2 plugin, and then click ```Install without restart``.

- Once the installation is done, click Go back to the top page.

- Configure Cloud (Older versions of Jenkins)
  * Click on Manage Jenkins,  and then Configure System.
  * Scroll all the way down to the section that says Cloud.

- Configure Cloud (Recent versions of Jenkins)
  * Click on Manage Jenkins,  and then Manage Nodes and Cloud
  * Click on Configure Clouds

- Click Add a new cloud, and select Amazon EC2. A collection of new fields appears.

- Fill out all the fields.  (Note: You will have to Add Credentials of the kind AWS Credentials.)You are now ready to use EC2 instances as Jenkins build slaves.

- Name
- Amazon EC2 Credentials
  * Click add credentials
  * Enter the details including access key ID and secret access key
- Region e.g ```us-east-2``` I stayed within same region
- Click test connection
Under AMIs (Add as many AMIs with the following procedure)
- Click Add
- AMI description e.g ```slave_1_amazonlinux-x86-64```
- AMI ID e.g ```ami-01b01bbd08f24c7a8```
- Click Check AMI
- Instance type e.g ```T2Micro```
- Availability zone - e.g ```us-east-2``` I used same as master
- Security group name - enter the security group of EC2 instance where you installed jenkins
- AMI type: ```unix```
- Labels e.g ```amazonlinux linux x86 64 ci``` Multiple labels separated by space
