---
path: "./2018/12/14/Automate-deployment-of-Jenkins-to-AWS_Part-1_Semi-automation_Single-EC2-instance.md"
date: "2018-12-14T11:48:00"
title: "Automate deployment of Jenkins to AWS - Part 1 - Semi automation - Single EC2 instance"
description: "Automate deployment of Jenkins to AWS - From semi automation of single to full automation of cluster - Part 1"
tags: ["Jenkins", "Automation", "AWS", "AWS Command Line Interface (CLI)", "jenkins docker image", "Putty", "docker"]
lang: "en-us"
---

__Last updated__: 2020-05-06T17:25:00

### Before we set off ###

__About this Article__

- Hi, this article is one of a series of 3 articles.

- The series is a guide to walk you through automating the deployment of
Jenkins to AWS; from semi-automation of a single EC2 instance to full
automation of a Jenkins cluster. Below are the links to each of the 3 parts:

  * [Part 1 - Semi automation of a single EC2 instance](/2018/12/14/Automate-deployment-of-Jenkins-to-AWS_Part-1_Semi-automation_Single-EC2-instance)

  * [Part 2 - Full automation of a single EC2 instance](/2018/12/14/Automate-deployment-of-Jenkins-to-AWS_Part-2_Full-automation_Single-EC2-instance)

  * @TODO@

- This article is the first part of the series.

- You can find the source for this article [here](https://github.com/poshjosh/automate-jenkins-to-aws)

- The objective of this first part is to create a re-usable amazon AMI which
auto installs Jenkins, some specified plugins and omits the initial user
password.

- It is an estimated 12 minute walk through.

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

### Create an amaon EC2 instance ###

- In order to create a re-usable amazon AMI which auto installs Jenkins, we
need to first create an amazon EC2 instance.

  * Browse to AWS Management Console -> Compute -> EC2
  * Click on ```Launch Instance``` (at the top)
  * Select the Amazon Machine Image (AMI) - Make sure to use an ```Amazon Linux 2```
  image or you won't be able to find or install ```amazon-linux-extras```
  required later in this article.
  * Select the instance type. Free tier `t2.micro` will not have a enough
  capacity for both jenkins and sonarqube.
  * Enter other instance details
  * Click on advanced details
  * Enter the bash script below for the user data. It will be run on first launch.
```
#!/bin/bash
@echo off
echo "Re-start required after running this script."
echo "... Updating system"
sudo yum update -y
echo "... Installing docker"
sudo amazon-linux-extras install docker
echo "... Starting docker service"
sudo service docker start
echo "... Adding ec2 user to docker group"
sudo usermod -a -G docker ec2-user
echo "... Restarting docker"
sudo service docker restart
```

You can download actual file with the above contents, [here](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/first-launch.sh)

- Wait till the EC2 instance is running and has passed status checks

### Builder a jenkins image via docker ###

- Next we create 3 files as follows and put them in any directory of choice.
I will be putting mine in `/jenkins-auto-install`.

View the 3 files through the following links:

  * [default-user.groovy](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/jenkins-auto-install/default-user.groovy). This file helps create the jenkins default user

  * [Dockerfile](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/jenkins-auto-install/Dockerfile).
  This is the docker file that will be used to build the jenkins image

  * [jenkins-plugins](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/jenkins-auto-install/jenkins-plugins).
  This file contains the names of plugins to install, one on each line.

- Next we transfer the contents of the entire directory we just created to the
remote EC2 instance. We achieve this using Putty.

The following example transfers the file Sample_file.txt from the C:\ drive on a
Windows computer to the ec2-user home directory on an Amazon Linux instance:

```
pscp -i C:\path\my-key-pair.ppk C:\path\Sample_file.txt ec2-user@public_dns:/home/ec2-user/Sample_file.txt
```

Open a command prompt on windows and enter the following command, replacing
`<path-to-ppk-file>` and `<public_dns>` with appropriate values.

```
pscp -i "<path-to-ppk-file>" C:\Users\User\Documents\GitHub\jenkins-auto-install\* ec2-user@<public_dns>:/home/ec2-user/
```

- Next we run the following command to build jenkins image from Dockerfile in current directory:
_The docker file was one of the files we transfered to the EC2 instance_

```
sudo docker build -t poshjosh/jenkins:lts .
```		

### Update user data of the EC2 instance ###

- Next we update the user data of our EC2 instance:
  * Stop the EC2 instance
  * Click Actions
  * View/Edit user data
  * Download [this file](https://github.com/poshjosh/automate-jenkins-to-aws/blob/master/subsequent-launches.sh "EC2 user - data") and use it to update the user data.

- Now start the EC2 instance

- Wait till the EC2 instance is running and has passed status checks

- Browse to ```http://<ec2_public_dns>:8080``` to view your jenkins installation.

- If you encounter any problems read the observations section at the end of this article.

### Create an Amazon Machin Image (AMI) from EC2 instance ###

Login with the password which we hard coded into the Dockerfile
```
ENV JENKINS_USER poshjosh
ENV JENKINS_PASS UB40-music
```

- Now create a re-usable image

  * Click Actions -> Image - Create image
  * Follow the instructions to create a re-usable amazon AMI which auto installs jenkins, some specified plugins and omits the initial user password.

- This series was designed to give you a feel of automating jenkins installation
on aws with a semi-automated exampl. The [next](/2018/12/14/Automate-deployment-of-Jenkins-to-AWS_Part-2_Full-automation_Single-EC2-instance) article will involve the fully automated deployment of a single
jenkins server to an AWS ec2 instance.

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

__2. Use Putty to execute a shell script from Windows to remote linux (AWS)__

- Linux shell - Bad interpreter error
```
[ec2-user@ip-172-31-16-248 ~]$ chmod +x docker_run_jenkins_and_sonarqube.sh
[ec2-user@ip-172-31-16-248 ~]$ ./docker_run_jenkins_and_sonarqube.sh
-bash: ./docker_run_jenkins_and_sonarqube.sh: /bin/bash^M: bad interpreter: No such file or directory
```

- Solution

The script indicates that it must be executed by a shell located at /bin/bash^M.
There is no such file: it's called /bin/bash.

The ^M is a carriage return character. Linux uses the line feed character to
mark the end of a line, whereas Windows uses the two-character sequence CR LF.
Your file has Windows line endings, which is confusing Linux.

Remove the spurious CR characters. You can do it with the following command:

```
sed -i -e 's/\r$//' create_mgw_3shelf_6xIPNI1P.sh
```

__3. Use Putty to send a directory from windows to remote linux server (AWS)__

- Space not allowed in file paths. In this case in `.ppk` file path
```
C:\Windows\system32>pscp -i C:\Users\User\Google Drive\IT\software\AWS\demo.ppk -r C:\Users\User\Documents\GitHub\jenkins-auto-install <public_dns>:/home
Unable to use key file "C:\Users\User\Google" (unable to open file)
FATAL ERROR: No supported authentication methods available (server sent: publickey)
```

- `%20` can't be used to replace space in file paths. In this case in `.ppk` file path
```
C:\Windows\system32>pscp -i C:\Users\User\Google%20Drive\IT\software\AWS\demo.ppk -r C:\Users\User\Documents\GitHub\jenkins-auto-install <public_dns>:/home
Unable to use key file "C:\Users\User\Google%20Drive\IT\software\AWS\demo.ppk" (unable to open file)
FATAL ERROR: No supported authentication methods available (server sent: publickey)
```

- Single quote will not work to enclose file paths with space(s)
```
C:\Windows\system32>pscp -i 'C:\Users\User\Google Drive\IT\software\AWS\demo.ppk' -r C:\Users\User\Documents\GitHub\jenkins-auto-install <public_dns>:/home
Unable to use key file "'C:\Users\User\Google" (unable to open file)
FATAL ERROR: No supported authentication methods available (server sent: publickey)
```

- Missing user name (i.e `ec2-user`) prefix in target dir
```
C:\Windows\system32>pscp -i "C:\Users\User\Google Drive\IT\software\AWS\demo.ppk" -r C:\Users\User\Documents\GitHub\jenkins-auto-install <public_dns>:/home
Server refused our key
FATAL ERROR: No supported authentication methods available (server sent: publickey)
```

- Adding `-l user ec2-user` will not help as long as there is missing user name (i.e `ec2-user`) prefix in target dir
```
C:\Windows\system32>pscp -i "C:\Users\User\Google Drive\IT\software\AWS\demo.ppk" -l user ec2-user -r C:\Users\User\Documents\GitHub\jenkins-auto-install <public_dns>:/home
Server refused our key
FATAL ERROR: No supported authentication methods available (server sent: publickey)
```

- Adding `-l user ec2-user`, while having user name (i.e `ec2-user`) prefix in target dir, will lead to error
```
C:\Windows\system32>pscp -i "C:\Users\User\Google Drive\IT\software\AWS\demo.ppk" -l user ec2-user -r C:\Users\User\Documents\GitHub\jenkins-auto-install ec2-user@<public_dns>:/home
pscp: ec2-user: No such file or directory

pscp: -r: No such file or directory

pscp: C:\Users\User\Documents\GitHub\jenkins-auto-install: not a regular file
```

- You obviously cannot copy to `/home/` directory this way. Rather copy to `/home/ec2-user` (user home path)
```
C:\Windows\system32>pscp -i "C:\Users\User\Google Drive\IT\software\AWS\demo.ppk" C:\Users\User\Documents\GitHub\jenkins-auto-install\* ec2-user@<public_dns>:/home/
pscp: unable to open /home//default-user.groovy: permission denied
pscp: unable to open /home//Dockerfile: permission denied
pscp: unable to open /home//jenkins-plugins: permission denied
```
- Success
```
C:\Windows\system32>pscp -i "C:\Users\User\Google Drive\IT\software\AWS\demo.ppk" C:\Users\User\Documents\GitHub\jenkins-auto-install\* ec2-user@<public_dns>:/home/ec2-user/
default-user.groovy       | 0 kB |   0.7 kB/s | ETA: 00:00:00 | 100%
Dockerfile                | 0 kB |   0.6 kB/s | ETA: 00:00:00 | 100%
jenkins-plugins           | 0 kB |   0.2 kB/s | ETA: 00:00:00 | 100%
```

### References ###

- [AWSEC2 User Guide - Putty](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html "AWSEC2 User Guide - Putty")

- [PSCP upload folder windows to linux](https://serverfault.com/questions/295565/pscp-upload-an-entire-folder-windows-to-linux "PSCP upload folder windows to linux")

- [Linux shell - Bad interpreter error](https://askubuntu.com/questions/304999/not-able-to-execute-a-sh-file-bin-bashm-bad-interpreter "Linux shell - Bad interpreter error")
