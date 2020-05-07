---
path: "./2020/03/10/Jenkins-on-AWS_Automation.md"
date: "2020-03-10"
title: "Jenkins on AWS - Automation"
description: "Easy to understand tutorial: Jenkins on AWS Automation"
lang: "en-us"
---

### Background ###

Jenkins Cluster on AWS

![Jenkins Cluster on AWS](https://blog.sonatype.com/hs-fs/hubfs/2.jpeg "Jenkins Cluster on AWS")

The platform has a Jenkins cluster with a dedicated Jenkins master and workers inside an autoscaling group. Each push event to the code repository will trigger the Jenkins master which will schedule a new build on one of the available slave nodes.

The slave nodes will be responsible of running the unit and pre-integration tests, building the Docker image, storing the image to a private registry and deploying a container based on that image to Docker Swarm cluster. If you missed my talk, you can watched it again on YouTube - below.

In this post, I will walk through how to deploy the Jenkins cluster on AWS using the latest automation tools.

The cluster will be deployed into a VPC with 2 public and 2 private subnets across 2 availability zones. The stack will consist of an autoscaling group of Jenkins workers in a private subnet and a private instance for the Jenkins master sitting behind an elastic load balancer.

To add or remove Jenkins workers on-demand, the CPU utilization of the ASG will be used to trigger a scale out ( CPU > 80%) or scale in ( CPU < 20%) event.

![Jenkins on AWS](https://blog.sonatype.com/hs-fs/hubfs/3-2.png "Jenkins on AWS")

### References ###

- [Deploy a Jenkins Cluster on AWS in a Fully Automated CI/CD](https://dzone.com/articles/how-to-deploy-a-jenkins-cluster-on-aws-as-part-of "Deploy a Jenkins Cluster on AWS in a Fully Automated CI/CD")
- [Summary - CI/CD platform on AWS using Terraform, Packer, and Ansible](https://www.youtube.com/watch?v=Zl8Gol7oOgI "CI/CD platform on AWS using Terraform, Packer, and Ansible")
- [Detailed - CI/CD platform on AWS using Terraform, Packer, and Ansible](https://www.youtube.com/watch?v=j783Ozvtb1w&list=PLbHVi6Q_JON4NxvZVzrMemfqh6BRUNEAq "CI/CD platform on AWS using Terraform, Packer, and Ansible")
