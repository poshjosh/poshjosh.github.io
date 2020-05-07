---
path: "./2020/03/10/Jenkins-on-AWS_Best-practices.md"
date: "2020-03-10"
title: "Jenkins on AWS - Best practices"
description: "Easy to understand tutorial: Jenkins on AWS Best practices"
lang: "en-us"
---

I've had some experience in this realm for a while now, but I'm having a little trouble with one issue in particular. Before I divulge, I'll present my thoughts on best practice and and what I've been able to implement:

- Terraform everything (in accordance to terragrunt's "style guide" i.e. organization)

- THIS IS A BIG ONE: for the jenkins master task, make sure to use the following args to make sure jenkins jobs aren't super slow as hell to start:

```
-Djava.awt.headless=true -Dhudson.slaves.NodeProvisioner.initialDelay=0 -Dhudson.slaves.NodeProvisioner.MARGIN=50 -Dhudson.slaves.NodeProvisioner.MARGIN0=0.85
```
THIS IS A GAME CHANGER (more-so on k8s clusters when the ecs plugin isn't used... hint, it's shit).

- Create an EFS (in a separate terraform module) and mount it to the jenkins ECS cluster at /var/jenkins_home. Makes jenkins much more reliable through outages and easier to upgrade.

- Run a logging agent (via docker container) like logspout or newrelic or whatever IN USER_DATA and not as a task - that way you get logs if there are issues during user_data/cloud_init... this I'm actually not sure about. Running a container outside the context of an ECS task means the ECS agent can't really track it and allocate mem/cpu properly... but it does help with user_data triage.

- Use pipelines and git plugins to drive jobs. All jenkins jobs should be in source control!

- Make sure you setup docker cleanup jobs on DAY 1! If you hace limited access to your cluster and you run out of disk due to docker cache, networks, volumes, etc... you're screwed till the admin ssh's in and runs a prune. Get a docker system prune going or the equivalent for each docker resource with appropriate filters... i.e. filter for anything older than a few days and is dangling.

- Use Jenkins Global Libraries to make Jenkinsfiles cleaner (I always just use vars instead of groovy/java style packages because it's easier and less ugly)

- Jenkinsfiles should mostly call other bash files, make files, python scripts to generate and load prop files, etc. The less logic you put in a Jenkinsfile (which is just modified groovy) the better. String interpolation, among other things, is a fuckery that we don't have time to triage.

- (out-of-scope) Move to using k8s/EKS instead of ECS asap because the ECS plugin for jenkins is absolute shit and it doesn't use priority correctly (sorry whoever developed it and... oh wait abandoned it and hasn't merged anything for years... for for real it's cool, just give admin to someone else).

- (cultural) Stop calling them slaves. "Hey @eng, we're rotating slaves due to some cache issues. If you have been affected by race conditions in that past, our new update and slave rotation should fix that. Our update may have killed your job that was running on an old slave, just wait a few and the new slaves will be ready" <--This just doesn't look good.

Run your agents in a different cluster/ASG

- Run the Jenkins agent at least as a different user than the controller if you're running on the same server. They should have different levels of permissions.

- Someone laid it out on another comment, but create both users, put them in the docker group, give them different home directories, and never run as root.

- Also, if you put your agents in a separate ECS cluster and a different ASG, you can automatically prune and clean up your unneeded build artifacts during the natural course of scaling.

You will need to run the agent as privileged

- Unless you don't want to build docker images or invoke new processes within the agent.

- Mount the hosts home directory and docker sock, and on the host create cgroups for net_cls, systemd.... And I can't remember the others. Basically, Jenkins agents will need to create new processes, so they must have extended access to the host. They need to be able to create and manage their processes. And to do that those cgroups need to exist. There like 5 of them, and I can't remember what they are since I threw them in an ansible task and forgot about it.

Snapshot the master nightly

- There is a way on the AWS console to set up nightly snapshots of an EBS volume. Make sure the master home volume is on its own EBS and backed up nightly.

- You will need that snapshot if you want to move to a new AZ or region. Just spin up a new home volumes from snapshot, mount, and go.

- You will only ever have 1 master instance.

### References ###

- [Reddit.com - Best Practices Jenkins on AWS ECS](https://www.reddit.com/r/devops/comments/9rpfbq/lets_talk_bestpractice_jenkins_on_aws_ecs/ "Best Practices Jenkins on AWS ECS")
