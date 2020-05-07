---
path: "./2019/10/02/Terraform-an-introduction.md"
date: "2019-10-02"
title: "Terraform - an introduction"
description: "Quickly get started with Hashicorp's Terraform"
tags: ["Terraform", "Hashicorp", "infrastructure", "AWS Command Line Interface (CLI)", "terraform configuration", "provider", "variable", "resource", "output", "terraform init", "terraform apply", "terraform show", "terraform destroy"]
lang: "en-us"
---

### What is Terraform? ###

Terraform is used to create, manage, and update infrastructure resources such as
physical machines, VMs, network switches, containers, and more. Almost any
infrastructure type can be represented as a resource in Terraform.

### Install Terraform ###

To install Terraform, find the [appropriate package](https://www.terraform.io/downloads.html)
for your system and download it. Terraform is packaged as a zip archive.

After downloading Terraform, unzip the package. Terraform runs as a single binary
named terraform. Any other files in the package can be safely removed and
Terraform will still function.

The final step is to make sure that the terraform binary is available on the PATH.
See [this page](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux)
for instructions on setting the PATH on Linux and Mac.
[This page](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)
contains instructions for setting the PATH on Windows.

After installing Terraform, verify the installation worked by opening a new
terminal session and checking that terraform is available. By executing
`terraform` you should see help output.

### Install and Configure AWS CLI ###

Here are links to install the AWS CLI on various platforms

- [AWS CLI - Install v2 on Linux](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html)
- [AWS CLI - Install v2 on Mac OS](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html)
- [AWS CLI - Install v2 on Windows](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html)
- [AWS CLI - Install v2 on Docker](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-docker.html)

Quickly Configuring the AWS CLI

For general use, the ```aws configure``` command is the fastest way to set up your
AWS CLI installation. The following example shows sample values. Replace them with
your own values as described in the following sections.

```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

When you enter the command ```aws configure```, the AWS CLI prompts you for four
pieces of information (access key, secret access key, AWS Region, and output
format). These are described in the following sections. The AWS CLI stores this
information in a profile (a collection of settings) named default.
The information in the default profile is used any time you run an AWS CLI
command that doesn't explicitly specify a profile to use.

### Terraform Configuration ###

The set of files used to describe infrastructure in Terraform is known as a
Terraform configuration. You'll write your first configuration now to launch a
single AWS EC2 instance.

A configuration should be in its own directory. Create a directory for the new
configuration.
```
$ mkdir terraform-aws
```

Change into the directory.
```
$ cd terraform-aws
```

Create a file for the configuration code.
```
$ touch aws-ec2-ubuntu-busybox-httpd.tf
```

The format of the configuration files is [documented here](https://www.terraform.io/docs/configuration/index.html).

Paste the configuration below into `aws-ec2-ubuntu-busybox-httpd.tf` and save
it. Later when you run Terraform, it will load all files in the working directory that
end in `.tf`

```
/**
 * Run this script thus:
 *
 * terrform init
 * terraform apply -var "server_port=8080"
 */
provider "aws" {
    # profile refers to the user profile we are using to connect to AWS
    # It is available after installing AWS CLI and running command aws configure
    profile = "default"
    region  = "us-east-2"
}

variable "server_port" {
    description = "The port the server will use for HTTP requests"
    type        = number
    default     = 8080
}

variable "aws_ami" {
    description = "The Amazon Machine Image (ami)"
    type        = string
    default     = "ami-0fc20dd1da406780b" # Ubuntu Server 18.04 LTS (HVM) SSD 64bit x86
}

resource "aws_security_group" "security_group_1" {
    name = "webserver-security-group"
    ingress {
        from_port   = var.server_port
        to_port     = var.server_port
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }
}

resource "aws_instance" "instance_1" {
    ami           = var.aws_ami
    instance_type = "t2.micro"
    vpc_security_group_ids = [aws_security_group.security_group_1.id]

    # - Write the text "Hello, World" into index.html
    # - Run a webserver on specified port using busybox (available on Ubuntu by default)
    # - Use nohup to ensure the web server keeps running even after script exits
    # - & at the end so the web server runs in background; and the script can exit
    # instead of waiting for the webserver
    #
    user_data = <<-EOF
                #!/bin/bash
                echo "Hello, World" > index.html
                nohup busybox httpd -f -p "${var.server_port}" &
                EOF

    tags = {
        # The actual name that will be displayed on AWS
        Name = "instance_from_terraform"
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
output "public_ip" {
    value       = aws_instance.instance_1.public_ip
    description = "The public IP of the web server"
}
```

### Provider ###

The provider block is used to configure the named provider, in our case "aws". A
provider is responsible for creating and managing resources. Multiple provider
blocks can exist if a Terraform configuration manages resources from different
providers. You can even use multiple providers together. For example you could
pass the ID of an AWS instance to a monitoring resource from DataDog.

[Terraform Providers](https://www.terraform.io/docs/providers/index.html)

provider - profile

The profile attribute here refers to the AWS Config File in ```~/.aws/credentials```
on MacOS and Linux or ```%UserProfile%\.aws\credentials``` on a Windows system.
It is HashiCorp recommended practice that credentials never be hardcoded into
`*.tf` configuration files. We are explicitly defining the default AWS
config profile here to illustrate how Terraform accesses sensitive credentials.

### Variable ###

Terraform allows you to define input variables, which may be referenced throughout
the configuration. If you define a variable without a default value, then you need
to provide the value when you call apply e.g ```$ terraform apply -var "server_port=8080"```.
If you do not, you will be prompted by terraform to enter the required variable
before proceeding with the apply.

You could also set the variable via an environment variable named TF_VAR_<name> where
<name> is the name of the variable you’re trying to set:

```
$ export TF_VAR_server_port=8080
$ terraform apply
```

To use the value from an input variable in your Terraform code, you can use a new type
of expression called a variable reference, which has the following syntax:

`var.<VARIABLE_NAME>` e.g `var.server_port`

To use a reference inside of a string literal, you need to use a new type of expression
called an interpolation, which has the following syntax:

`"${...}"` e.g `"${var.server_port}"`

#### Resource ####

The resource block defines a piece of infrastructure. A resource might be a
physical component such as an EC2 instance, or it can be a logical resource such
as a Heroku application.

#### Output ####

Output variables show up in the console after you run terraform apply command. Users
of the terraform code may find this usesful. For example, in this case after deploying
a web server, we need an ip address to use in accessing that server.

### Some Terraform Commands ###

The first command to run for a new configuration -- or after checking out an
existing configuration from version control -- is terraform init. Subsequent
commands will use local settings and data that are initialized by terraform init.

Terraform uses a plugin-based architecture to support hundreds of infrastructure
and service providers. The terraform init command downloads and installs providers
used within the configuration, which in this case is the aws provider.

__init__

In the same directory as the terraform configuration ```.tf``` file you created,
run the following command.

```
$ terraform init
```
Terraform downloads the aws provider and installs it in a hidden subdirectory of
the current working directory.

If you are copying configuration snippets or just want to make sure your
configuration is syntactically valid and internally consistent, the built in
`terraform validate` command will check and report errors within modules,
attribute names, and value types.

```
Note: The commands shown in this guide apply to Terraform 0.11 and above. Earlier
versions require using the terraform plan command to see the execution plan before
applying it. Use terraform version to confirm your running version.
```

__apply__

In the same directory as the terraform configuration ```.tf``` file you created,
run the following command.

```
$ terraform apply
```

This output shows the execution plan, describing which actions Terraform will take
in order to change real infrastructure to match the configuration. The output format
is similar to the diff format generated by tools such as Git

After this, Terraform is all done! You can go to the EC2 console to see the
created EC2 instance. (Make sure you're looking at the same region that was
configured in the provider configuration!)

__show__

You can inspect the current state using the following command

```
$ terraform show
```

Terraform also wrote some data into the ```terraform.tfstate``` file. This state
file is extremely important; it keeps track of the IDs of created resources so
that Terraform knows what it is managing. This file must be saved and distributed
to anyone who might run Terraform. It is generally recommended to
[setup remote state](https://www.terraform.io/docs/state/remote.html)
when working with Terraform, to share the state automatically.

__destroy__

The terraform destroy command terminates resources defined in your Terraform
configuration. This command is the reverse of terraform apply in that it terminates
all the resources specified by the configuration. It does not destroy resources
running elsewhere that are not described in the current configuration.

When you are done here run ```terraform destroy```

### Modifying Infrastructure with Terraform ###

Infrastructure is continuously evolving, and Terraform was built to help manage
and enact that change. As you change Terraform configurations, Terraform builds
an execution plan that only modifies what is necessary to reach your desired
state. By using Terraform to change infrastructure, you can version control not
only your configurations but also your state so you can see how the infrastructure
evolved over time.

In our case, if we change the value of the default ami in the  ```aws_ami``` section
as follows:

```
variable "aws_ami" {
    description = "The Amazon Machine Image (ami)"
    type        = string
    default     = "ami-08cec7c429219e339" # Ubuntu Server 16.04 LTS (HVM), SSD Volume Type (64-bit x86)
}
```

And apply it as follows:
```
$ terraform apply
```

From the console output, the prefix ```-/+``` means that Terraform will destroy
and recreate the resource, rather than updating it in-place. While some attributes
can be updated in-place (which are shown with the ```~``` prefix), changing the
AMI for an EC2 instance requires recreating it. Terraform handles these details
for you, and the execution plan makes it clear what Terraform will do.

Additionally, the execution plan shows that the AMI change is what required your
resource to be replaced. Using this information, you can adjust your changes to
possibly avoid destroy/create updates if they are not acceptable in some situations.

Once again, Terraform prompts for approval of the execution plan before proceeding.
Answer ```yes``` to execute the planned steps:

Terraform will first destroy the existing instance and then created a new one in
its place. You can use terraform show again to see the new values associated with
this instance.

### References ###
- [Terraform installation](https://learn.hashicorp.com/terraform/getting-started/install.html)
- [Terraform build](https://learn.hashicorp.com/terraform/getting-started/build)
- [Terraform change](https://learn.hashicorp.com/terraform/getting-started/change)
- [Terraform destroy](https://learn.hashicorp.com/terraform/getting-started/destroy)
- [Terraform dependencies](https://learn.hashicorp.com/terraform/getting-started/dependencies)
- [Terraform provision](https://learn.hashicorp.com/terraform/getting-started/provision)
- [Terraform variables](https://learn.hashicorp.com/terraform/getting-started/variables)
- [Terraform outputs](https://learn.hashicorp.com/terraform/getting-started/outputs)
- [AWS CLI - Install v2 on Linux](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html)
- [AWS CLI - Install v2 on Mac OS](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html)
- [AWS CLI - Install v2 on Windows](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html)
- [AWS CLI - Install v2 on Docker](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-docker.html)
- [AWS CLI - Configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)
- [Terraform Providers](https://www.terraform.io/docs/providers/index.html)
