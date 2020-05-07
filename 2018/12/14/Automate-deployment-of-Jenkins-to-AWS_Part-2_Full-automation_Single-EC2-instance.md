---
path: "./2018/12/14/Automate-deployment-of-Jenkins-to-AWS_Part-2_Full-automation_Single-EC2-instance.md"
date: "2018-12-14T21:51:10"
title: "Automate deployment of Jenkins to AWS - Part 2 - Full automation - Single EC2 instance"
description: "Automate deployment of Jenkins to AWS - From semi automation of single to full automation of cluster - Part 2"
tags: ["Jenkins", "Automation", "AWS", "AWS Command Line Interface (CLI)", "jenkins docker image", "Putty", "docker", "terraform", "packer"]
lang: "en-us"
---

__Last updated__: 2020-05-06T19:59:00

### Before we set off ###

__About this Article__

- Hi, this article is one of a series of 3 articles.

- The series is a guide to walk you through automating the deployment of
Jenkins to AWS; from semi-automation of a single EC2 instance to full
automation of a Jenkins cluster. Below are the links to each of the 3 parts:

  * [Part 1 - Semi automation of a single EC2 instance](/2018/12/14/Automate-deployment-of-Jenkins-to-AWS_Part-1_Semi-automation_Single-EC2-instance)

  * [Part 2 - Full automation of a single EC2 instance](/2018/12/14/Automate-deployment-of-Jenkins-to-AWS_Part-2_Full-automation_Single-EC2-instance)

  * @TODO@

- This article is the second part of the series.

- You can find the source for this article [here](https://github.com/poshjosh/automate-jenkins-to-aws)

- The objective of this first part is to create a re-usable amazon AMI which
auto installs Jenkins, some specified plugins and omits the initial user
password.

- It is an estimated 18 minute walk through.

__Prerequisites__

- For Part 1 you need to enjoy some familiarity with AWS CLI, Putty,
linux/windows shell commands and docker.

- For Parts 2 and 3 you need to be familiar with terraform and packer in
addition to the aforementioned.

- It is expected that you enjoy some familiarity with these tools.
Notwithstanding, if you follow the instructions, you should get by just fine.

- For an introduction to terraform and packer use the following links:

  * [Terraoform - an introduction](/2019/10/02/Terraoform-an-introduction)

  * [Packer - an introduction](/2019/10/02/Packer-an-introduction)

- To work along with this tutorial, you will need an AWS account and an AWS
EC2 key pair. Usually a `.ppk` (windows) or `.pem` (linux) file.

- Have AWS-CLI installed and AWS configured with secret keys:

  * `Secret access key`

  * `Access Key ID`     

__And then some__

Moving from semi - automation of a single amazon EC2 instance to full
automation requires the introduction of some tools. For this use case we
will use [terraform](https://www.terraform.io/) and [packer](https://www.packer.io/).

We will be build an amazon ami with packer. The ami will then be accessed
by terraform and used to provision an EC2 instance.

### Build Amazon Machine Image (AMI) with Packer ###

We begin by creating a directory named `automate-deployment-of-jenkins-to-aws` to hold all our files. Do this by opening
a shell and running the following command.

```sh
mkdir automate-deployment-of-jenkins-to-aws
cd automate-deployment-of-jenkins-to-aws
```

The second command above changes the current directory to the newly
created `automate-deployment-of-jenkins-to-aws`

Now create a file called `packer.json` and add the following contents:

```json
{
    "builders" : [
        {
            "type" : "amazon-ebs",
            "profile" : "default",
            "region" : "us-east-2",
            "instance_type" : "t3a.large",
            "source_ami" : "ami-0e01ce4ee18447327",
            "ssh_username" : "ec2-user",
            "ami_name" : "awslinux-dockerce-jenkins-sonarqube_{{timestamp}}",
            "ami_description" : "Amazon Linux Image with Docker-CE, Jenkins and Sonarqube",
            "run_tags" : {
                "Name" : "packer-builder-docker-jenkins-sonarqube"
            },
            "tags" : {
                "Tool" : "Packer",
                "Author" : "ChinomsoIkwuagwu",
                "OS_version": "amazon linux 2"
            }
        }
    ],
    "provisioners" : [
        {
            "type" : "file",
            "source" : "./docker",
            "destination" : "/tmp/docker"
        },
        {
            "type" : "file",
            "source" : "./jenkins-auto-install/",
            "destination" : "/tmp/"
        },
        {
            "type" : "shell",
            "script" : "./setup.sh",
            "execute_command" : "sudo -E -S sh '{{ .Path }}'"
        }
    ]
}
```

You can download the file for the above `.json` config [here](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/packer.json)

__Notes on packer config__

- We use `t3a.large` as `t2.micro` etc does not have enough capacity for both
jenkins and sonarqube.

- With the `provisioners.type = file` we are copying files/directories from
our local machine to the remote server.

Need to know about the `packer file provisioner`

> Warning: You can only upload files to locations that the provisioning user
> (generally not root) has permission to access. Creating files in /tmp and
> using a shell provisioner to move them into the final location is the only
> way to upload files to root owned locations.

> The file provisioner can upload both single files and complete directories.

- With the `provisioners.type = shell` we are running a shell script named
`setup.s` on the remote machine. The shell script is located on our local
machine and is transferred by packer to the remote machine and executed.

- For the shell script, create a file name `setup.sh` and add the following content:

```sh
#/bin/sh
yum update -y
yum install docker -y
service docker start
usermod -aG docker ec2-user
mv /tmp/docker /etc/sysconfig/docker
chmod 644 /etc/sysconfig/docker
service docker restart
mv /tmp/default-user.groovy ./default-user.groovy
mv /tmp/Dockerfile ./Dockerfile
mv /tmp/jenkins-plugins ./jenkins-plugins
docker build -t poshjosh/jenkins:lts .
docker volume create jenkins-data
docker run --name jenkins-lts --rm --detach --privileged -p 8080:8080 -p 50000:50000 -v jenkins-data:/var/jenkins_home -v $(which docker):/usr/bin/docker -v /var/run/docker.sock:/var/run/docker.sock -v "$HOME":/home poshjosh/jenkins:lts
docker run --name sonarqube --rm --detach -p 9000:9000 sonarqube
```

You can download the file for the above `.sh` commands [here](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/setup.sh)

The following files from the first part of this series of articles is
also required in the `automate-deployment-of-jenkins-to-aws` directory, so
download and emplace them accordingly.

  * [default-user.groovy](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/jenkins-auto-install/default-user.groovy). This file helps create the jenkins default user

  * [Dockerfile](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/jenkins-auto-install/Dockerfile).
  This is the docker file that will be used to build the jenkins image

  * [jenkins-plugins](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/jenkins-auto-install/jenkins-plugins).
  This file contains the names of plugins to install, one on each line.

These 2 files are also required in the `automate-deployment-of-jenkins-to-aws` directory:

  * [docker](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/docker) - Docker system configuration.

  * [user-data.txt](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/user-data.txt)
  Script, usually to be called on first launch of the EC2 instance, but
  configured in this case to be called each time the instance launches.

The `automate-deployment-of-jenkins-to-aws` directory should look like this:

```
automate-deployment-of-jenkins-to-aws/
    - jenkins-auto-install/
         - default-user.groovy
         - Dockerfile
         - jenkins-plugins
    - docker
    - packer.json
    - setup.sh
    - user-data.txt
```

To build the AMI run the following command in a shell:

```sh
# validate packer template
packer validate packer.json

# build ami
packer build packer.json
```

The above command should take some minutes, and end with the following sample output:
```
==> Builds finished. The artifacts of successful builds are:
--> amazon-ebs: AMIs were created:
us-east-2: ami-093f64ce76b765b1a
```

We are now ready to provision an EC2 with the newly created ami using terraform.

### Provision an EC2 instance using Terraform ###

Create a file named `terraform.tf` and add the following contents:

```
variable "aws_region" {
    description = "The AWS region"
    type        = string
    default     = "us-east-2"
}

variable "instance_type" {
    type        = string
    default     = "t3a.large"
}

variable "security_group_name" {
    description = "The name of the associated resource as will be displayed on AWS"
    type        = string
    default     = "aws_sec_grp_incoming_custom_and_ssh-linux-docker"
}

variable "instance_name" {
    description = "The name of the instance as will be displayed on AWS"
    type        = string
    default     = "awslinux-dockerce-jenkins-sonarqube_1"
}

variable "server_port" {
    description = "The port the jenkins server will use for HTTP requests"
    type        = number
    default     = 8080
}

variable "sonarqube_port" {
    description = "The port the sonarqube server will use for HTTP requests"
    type        = number
    default     = 9000
}

provider "aws" {
    # profile refers to the user profile we are using to connect to AWS
    # It is available after installing AWS CLI and running command aws configure
    profile = "default"
    region  = var.aws_region
}

resource "aws_security_group" "security_group_1" {
    name        = var.security_group_name
    description = "security group that allows all egress traffic, ingress for ssh 22 and tcp 8080, 9000"

    # Terraform removes the default rule
    egress {
      description = "All outgoing to anywhere"
      from_port = 0
      to_port = 0
      protocol = "-1"
      cidr_blocks = ["0.0.0.0/0"]
    }
    ingress {
        description = "Allow SSH"
        from_port   = 22
        to_port     = 22
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }
    ingress {
        description = "Allow tcp jenkins port"
        from_port   = var.server_port
        to_port     = var.server_port
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }
    ingress {
        description = "Allow tcp sonarqube port"
        from_port   = var.sonarqube_port
        to_port     = var.sonarqube_port
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }
    ingress {
        description = "Allow http"
        from_port   = 80
        to_port     = 80
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }
    ingress {
        description = "Allow tls (https) from anywhere"
        from_port   = 443
        to_port     = 443
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }
    tags = {
        # The actual name that will be displayed on AWS
        Name   = var.security_group_name
        Tool   = "Terraform"
        Author = "ChinomsoIkwuagwu"
    }
}

# name refers to ami_name in packer .json file
# Which should have been created by packer before applying this config
data "aws_ami" "ami_1" {
    most_recent = true
    owners      = ["self"]
    filter {
        name   = "name"
        values = ["awslinux-dockerce-jenkins-sonarqube*"]
    }
}

resource "aws_instance" "instance_1" {
    ami           = data.aws_ami.ami_1.id
    instance_type = var.instance_type
    vpc_security_group_ids = [aws_security_group.security_group_1.id]

    user_data = file("user-data.txt")

    tags = {
        # The actual name that will be displayed on AWS
        Name       = var.instance_name
        Tool       = "Terraform"
        OS_version = "amazon linux 2"
        Author = "ChinomsoIkwuagwu"
    }
}

resource "aws_eip" "ip" {
    vpc      = true
    instance = aws_instance.instance_1.id
}

/**
 * Output variables show up in the console after you run terraform apply
 * command. Users of the terraform code may find this usesful. For example,
 * in this case after deploying a web server, we need an ip address to
 * use in accessing that server
 */
output "Public_ip" {
    value       = aws_instance.instance_1.public_ip
    description = "The public IP of the web server"
}

output "Jenkins_security_group_ID" {
    value = aws_security_group.security_group_1.id
}
```

You can download the file for the above terraform config [here](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/terraform.tf)

Values for `aws_region` and `instance_type` must match those in the file
`packer.json`

Using the `aws` provider, this terraform configuration will provision the
following AWS resources:

- __AWS security group__ An AWS security group that allows all egress traffic
(because Terraform removes the default egress allow-all rule) as well as
ingress for ssh 22 and tcp 8080, 9000

- __AWS EC2 instance__ An AWS EC2 instance to host the jenkins and sonarqube
server. The provisioned security group will be applied to the EC2 instance.

- __AWS Elastic IP__ An elastic ip.

At a minimum, you need to provide values for the following variables:

  * `aws_region`, default here is 'us-east-2'
  ```
  variable "aws_region" {
      description = "The AWS region"
      type        = string
      default     = "us-east-2"
  }
  ```

Now run the following command to provision the EC2 instance and associated
security group and elastic ip.

```sh
terraform init

terraform apply
```

You may now browse to your automatically provisioned jenkins server
via `http://<public-dns>:8080` and sonarqube server via `http://<public-dns>:9000`
where public dns is the public dns of the EC2 instance.

If you encounter any problems read the observations section at the end of this article.

Remember,  we hard coded jenkins credentials into the Dockerfile.
```
ENV JENKINS_USER poshjosh
ENV JENKINS_PASS UB40-music
```

This was only done here for simplicity. In the third part of this series,
we will adhere to best practices. We will also build a fully automated
deployment of a Jenkins ec2 cluster to AWS. Including auto scaling capabilities
as well as best practices.

### Clean up ###

__To remove the terraform provisioned EC2 instance__

```sh
terraform destroy
```

__To remove packer generated image__

After building images with packer, your AWS account will have the AMI you
built associated with it. AMIs are stored in S3 by Amazon, so unless you want
to be charged about $0.01 per month, you'll probably want to remove it. Remove
the AMI by:

  * First deregister the AMI on the [AWS AMI management page](https://console.aws.amazon.com/ec2/home?region=us-east-1#s=Images).

  * Next, delete the associated snapshot on the [AWS snapshot management page](https://console.aws.amazon.com/ec2/home?region=us-east-1#s=Snapshots).

### Observations ###

__1. Cannot access jenkins or sonarqube via the web browser__

If you can't browse to `http://<public-dns>:8080` or `http://<public-dns>:9000`

  * Confirm that you are using the correct EC2 instance type.
    - Browse to EC2 console -> Click on the EC2 instance
    - Click on the Monitoring tab
    - Check `CPU Utilization (Percent)`. If it is very high, say 90% then you
    probably have the wrong instance type.
  * Check that you have selected the correct EC2 instance and thus copied
  the correct `public-DNS`.  
  * Confirm that the port numbers for both jenkins and sonarqube are consistent
  in `setup.sh`, `user-data.txt` and `terraform.tf`   

__2. Terraform cannot find ami built by packer__

  * Error
  ```
  Error: Your query returned no results. Please change your search criteria and try again.

    on terraform.tf line 52, in data "aws_ami" "ami_1":
    52: data "aws_ami" "ami_1" {
  ```

  * Resource causing error
  ```
  data "aws_ami" "ami_1" {
      most_recent = true
      owners      = ["self"]
      filter {
          name   = "name"
          values = ["awslinux-dockerce-jenkins-sonarqube"]
      }
  }
  ```

  * Solution: Append asterix `*` to the value of the name attribute of filter
  as such: `awslinux-dockerce-jenkins-sonarqube` becomes
  `awslinux-dockerce-jenkins-sonarqube*` as shown below:
  ```
  data "aws_ami" "ami_1" {
      most_recent = true
      owners      = ["self"]
      filter {
          name   = "name"
          values = ["awslinux-dockerce-jenkins-sonarqube*"]
      }
  }
  ```

### References ###

- [Terraform](https://www.terraform.io/)

- [Packer](https://www.packer.io/)

- [Packer - file provisioner](https://www.packer.io/docs/provisioners/file/)

- [Packer - shell provisioner](https://www.packer.io/docs/provisioners/shell/)

- [Terraform - security group resource](https://www.terraform.io/docs/providers/aws/r/security_group.html)
