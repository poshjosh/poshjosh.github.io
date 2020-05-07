---
path: "./2019/10/02/Packer-an-introduction.md"
date: "2019-10-02"
title: "Packer - an introduction"
description: "Quickly get started with Hashicorp's Packer"
tags: ["Packer", "Hashicorp", "machine image", "variables", "builders", "provisioners"]
lang: "en-us"
---

### Why Use Packer? ###

Pre-baked machine images have a lot of advantages, but most have been unable to
benefit from them because images have been too tedious to create and manage. There
were either no existing tools to automate the creation of machine images or they
had too high of a learning curve. The result is that, prior to Packer, creating
machine images threatened the agility of operations teams, and therefore aren't used,
despite the massive benefits.

Packer changes all of this. Packer is easy to use and automates the creation of any
type of machine image. It embraces modern configuration management by encouraging you
to use a framework such as Chef or Puppet to install and configure the software within
your Packer-made images.

### Installing Packer ###

To install the precompiled binary, download the appropriate package for your system.
Packer is currently packaged as a zip file. We do not have any near term plans to
provide system packages.

Next, unzip the downloaded package into a directory where Packer will be installed.
On Unix systems, ```~/packer``` or ```/usr/local/packer``` is generally good,
depending on whether you want to restrict the install to just your user or install
it system-wide. If you intend to access it from the command-line, make sure to place
it somewhere on your ```PATH``` before ```/usr/sbin```. On Windows systems, you can put
it wherever you'd like. The ```packer``` (or ```packer.exe``` for Windows) binary
inside is all that is necessary to run Packer. Any additional files aren't required to
run Packer.

After unzipping the package, the directory should contain a single binary program called
```packer```. The final step to installation is to make sure the directory you installed
Packer to is on the PATH. See [this page](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux)
for instructions on setting the PATH on Linux and Mac. [This page](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)
contains instructions for setting the PATH on Windows.

### Packer GNU/Linux Example with Provisioners ###

Create a file named welcome.txt and add the following:
```
This is my first packer. It was made deliberately detailed for a beginner
```

Create another file named start.sh and add the following
```shell
#!/bin/bash
echo "Hello World"
```

Set your access key and id as environment variables, so we don't need to pass
them in through the command line:
```
export AWS_ACCESS_KEY_ID=MYACCESSKEYID
export AWS_SECRET_ACCESS_KEY=MYSECRETACCESSKEY
```

Now save the following packer ```template```, which is in ```[JSON](http://www.json.org/)```
format in a file named ```hellowold.json```
```
{
    "variables": {
        "aws_access_key": "{{env `AWS_ACCESS_KEY_ID`}}",
        "aws_secret_key": "{{env `AWS_SECRET_ACCESS_KEY`}}",
        "region":         "us-east-1"
    },
    "builders": [
        {
            "access_key": "{{user `aws_access_key`}}",
            "ami_name": "packer-linux-aws-demo-{{timestamp}}",
            "instance_type": "t2.micro",
            "region": "{{user `region`}}",
            "secret_key": "{{user `aws_secret_key`}}",
            "source_ami_filter": {
              "filters": {
              "virtualization-type": "hvm",
              "name": "ubuntu/images/*ubuntu-xenial-16.04-amd64-server-*",
              "root-device-type": "ebs"
              },
              "owners": ["099720109477"],
              "most_recent": true
            },
            "ssh_username": "ubuntu",
            "type": "amazon-ebs"
        }
    ],
    "provisioners": [
        {
            "type": "file",
            "source": "./welcome.txt",
            "destination": "/home/ubuntu/"
        },
        {
            "type": "shell",
            "inline":[
                "sleep 30",
                "sudo apt-get update",
                "ls -al /home/ubuntu",
                "cat /home/ubuntu/welcome.txt"
            ]
        },
        {
            "type": "shell",
            "script": "./example.sh"
        }
    ]
}
```

### Variables ###

The variables section contains variables which may be referenced/used in
other sections of the template. In this example they are gotten from the earlier
exported environment variables. However, you could leave them blank as shown below.
```
{
    "variables": {
        "aws_access_key": "",
        "aws_secret_key": "",
        "region":         "us-east-1"
    }
}
```

If you leave the variables blank as shown above, then you have to enter them with
the build command as shown below:

```
packer build -var 'aws_access_key=YOUR ACCESS KEY' -var 'aws_secret_key=YOUR SECRET KEY' myfirstpacker.json
```
In the above, for windows systems, use double quotes.

### Builders ###

The builders section contains an array of JSON objects configuring a
specific builder. A builder is a component of Packer that is responsible for
creating a machine and turning that machine into an image.

In this case, we're only configuring a single builder of type amazon-ebs. This is
the Amazon EC2 AMI builder that ships with Packer. This builder builds an EBS-backed
AMI by launching a source AMI, provisioning on top of that, and re-packaging it
into a new AMI.

The additional keys within the object are configuration for this builder, specifying
things such as access keys, the source AMI to build from and more. The exact set of
configuration variables available for a builder are specific to each builder and can
be found within the [packer documentation](https://packer.io/docs/index.html)

### Provisioners ###

The provisioners section is an array of provisioners to run. If multiple provisioners
are specified, they are run in the order given. By default, each provisioner is run
for every builder defined. So if we had two builders defined in our template, such
as both Amazon and DigitalOcean, then the shell script would run as part of both
builds. There are ways to restrict provisioners to certain builds, covered in more
detail in the [packer documentation](https://packer.io/docs/index.html).

In the above example, we use the built-in shell provisioner that comes with Packer
to update the linux system via ```sudo apt-get update```

__Notes__

- The ```.txt``` and ```.sh``` files should be in the safe directory as ```myfirstpacker.json```
- Use an ami available in the region you specify
- Note that if you wanted to use a ```source_ami``` instead of a ```source_ami_filter```
it might look something like this: ```"source_ami": "ami-fce3c696"```.
- The sleep 30 in the shell provisioner above is very important. Because Packer is able to
detect and SSH into the instance as soon as SSH is available, Ubuntu actually doesn't
get proper amounts of time to initialize. The sleep makes sure that the OS properly
initializes.

Before we take this template and build an image from it, let's validate the template by
running packer validate example.json. This command checks the syntax as well as the
configuration values to verify they look valid. The output should look similar to below,
because the template should be valid. If there are any errors, this command will tell you.

```
$ packer validate myfirstpacker.json
Template validated successfully.
```

Next, let's build the image from this ```template```.

```
$ packer build myfirstpacker.json
```

At the end of running packer build, Packer outputs the artifacts that were created as
part of the build. Artifacts are the results of a build, and typically represent an ID
(such as in the case of an AMI) or a set of files (such as for a VMware virtual machine).
In this example, we only have a single artifact: the AMI in ```us-east-1``` that was
created. This AMI is ready to use. If you wanted you could go and launch this AMI right
now and it would work great.

__Notes__

- If you see a ```VPCResourceNotSpecified``` error, Packer might not be able to determine
the default VPC, which the t2 instance types require. This can happen if you created your
AWS account before 2013-12-04. You can either change the instance_type to m3.medium, or
specify a VPC. Please see [this amazon document](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/default-vpc.html)
for more information. If you specify a ```vpc_id```, you will also need to
set ```subnet_id```. Unless you modify your subnet's [IPv4 public addressing attribute](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-ip-addressing.html#subnet-public-ip),
you will also need to set associate_public_ip_address to true, or set up a
[VPN](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_VPN.html).

- Packer only builds images. It does not attempt to manage them in any way. After they're
built, it is up to you to launch or destroy them as you see fit. After running the above
example, your AWS account now has an AMI associated with it. AMIs are stored in S3 by
Amazon, so unless you want to be ```charged about $0.01 per month```, you'll probably want
to remove it. Remove the AMI by first deregistering it on the AWS AMI management page.
Next, delete the associated snapshot on the AWS snapshot management page.

### References ###

- [Packer build image](https://packer.io/intro/getting-started/build-image.html)
- [Packer provision](https://packer.io/intro/getting-started/provision.html)
- [Packer documentation](https://packer.io/docs/index.html)
