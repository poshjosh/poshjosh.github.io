---
path: "./2019/11/22/Jenkins-titbits.md"
date: "2019-11-22"
title: "Jenkins titbits, and then some"
description: "Easy to understand tutorial: Jenkins titbits, and then some"
lang: "en-us"
---

### What is Jenkins? ###

Jenkins is used to automate all sorts of tasks related to building, testing, and delivering or deploying software. Read an [Introduction to Jenkins](/2018/02/05/Introduction-to-Jenkins), if you haven't already done so.

### Titbits ###
- You could mount your local system home dir to docker container home via ```-v “%HOMEDRIVE%%HOMEPATH%”:/home``` when running the container. This is no longer recommended as per [this post](http://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/) by [jpetazzo](https://github.com/jpetazzo).

- Rather than use docker in docker, you should run Docker (specifically: build, run, sometimes push containers and images) from your CI system, while this CI system itself is in a container, as per [this post](http://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/) by [jpetazzo](https://github.com/jpetazzo). To do this expose the unix socker via ```-v /var/run/docker.sock:/var/run/docker.sock```

Example of above 2 constructs in use:

```
docker run -u root --name jenkins-blueocean —-rm --detach -v jenkins-data:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v “%HOMEDRIVE%%HOMEPATH%”:/home -p 8080:8080 jenkinsci/blueocean
```

- If you mount a dir from your local machines to a dir in docker, as done above via ```-v “%HOMEDRIVE%%HOMEPATH%”:/home```, then in the Repository URL field when defining jenkins job, you could specify the directory path as if from you local drive, in this case prefixed by ```/home``` e.g ```/home/Documents/GitHub/[PROJECT_NAME_HERE]```

- When entering repository url, using backslash caused errors (Observed on Windows Machine)
Failed - C:\Users\USER\Documents\NetBeansProjects\project
Success - C:/Users/USER/Documents/NetBeansProjects/project

- If you encounter the error: docker is not a recognised tool.. See below for list of tools
[ant, hudson.tasks.Ant$AntInstallation, org.jenkinsci.plugins.docker.commons.tools.DockerTool, git, hudson.plugins.git.GitTool, gradle, hudson.plugins.gradle.GradleInstallation, jdk, hudson.model.JDK, jgit, org.jenkinsci.plugins.gitclient.JGitTool, jgitapache, org.jenkinsci.plugins.gitclient.JGitApacheTool, maven, hudson.tasks.Maven$MavenInstallation, hudson.plugins.sonar.SonarRunnerInstallation, hudson.plugins.sonar.MsBuildSQRunnerInstallation]

- If you specify ```*/master``` in branch and currently checkedout branch is ```*/dev``` then jenkins throws FileNotFoundException

- When you make changes to a local SCM e.g your project folder, you have to commit those changes before jenkins picks it up.

- When you make changes to a remove SCM e.g github repository, you have to push those changes before jenkins picks it up.

- Jenkins has its own repository.
If maven project A depends on project B. Then project A will fail unless you first install project B to the jenkins local repository usually done during the install phase.

- Problem of unrecognized docker command?
  * Problem: 'docker' is not recognized as an internal or external command, operable program or batch file.
  * Solution
    - Manage Jenkins -> Configure System -> Pipeline Model Definition
    - Set Docker Label to docker

### Example Pipeline Stages ###

- Create — checkout the head revision, compile the unit-test to the code, and build and archive the artifacts
- Integration test — run integration tests for these artifacts
- Live deploy — deploy artifacts to a live server
- Smoke test — run smoke tests for these deployed artifacts

### Sample Pipeline in declarative format with ###

This will work even if Jenkins itself is run in a container with mounted jenkins_home from host.

```groovy
pipeline {
    stages {
        stage('Build') {
            agent { //here we select only docker build agents
                docker {
                    image 'maven:latest' //container will start from this image
                    args '-v /root/.m2:/root/.m2' //here you can map local maven repo, this let you to reuse local artifacts
                }
            }
            steps {
                sh 'mvn -B -DskipTests clean package' //this command will be executed inside maven container
            }
        }
        stage('Test') { //on this stage New container will be created, but current pipeline workspace will be remounted to it automatically
            agent {
                docker {
                    image 'maven:latest'
                    args '-v /root/.m2:/root/.m2'
                }
            }
            steps {
                sh 'mvn test'
            }
        }
        stage ('Build docker image') { //here you can check how you can build even docker images inside container
            agent {
                docker {
                    image 'maven:latest'
                    args '-v /root/.m2:/root/.m2 -v /var/run/docker.sock:/var/run/docker.sock' //here we expose docker socket to container. Now we can build docker images in the same way as on host machine where docker daemon is installed
                }
            }
            steps {
                sh 'mvn -Ddocker.skip=false -Ddocker.host=unix:///var/run/docker.sock docker:build' //example of how to build docker image with pom.xml and fabric8 plugin
            }
        }
    }
}
```
### How to resolve some Docker DNS issues ###
Sometimes, Docker’s internet connectivity won’t be working properly, which can lead to a number of obscure errors with your applications. This could be because DNS lookups are failing in Docker images. To check Docker’s DNS, first, check that basic internet connectivity is working by pinging a public IP address. It should succeed, giving you output similar to this:
```
$ docker run busybox ping -c 1 192.203.230.10
PING 192.203.230.10 (192.203.230.10): 56 data bytes
64 bytes from 192.203.230.10: seq=0 ttl=53 time=113.866 ms
```

Sample output:
```
--- 192.203.230.10 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss
round-trip min/avg/max = 113.866/113.866/113.866 ms
```

If the output was successful as above, then move on to checking dns as follows:
```
$ docker run busybox nslookup google.com
```

Sample failure output:
```
Server:    8.8.8.8
Address 1: 8.8.8.8
nslookup: can't resolve 'google.com'
```

If your output is something like, it is not necessarily a failure response:
```
Server:         8.8.8.8
Address:        8.8.8.8:53

Non-authoritative answer:
Name:   google.com
Address: 216.58.223.238

*** Can't find google.com: No answer
```

To get a more specific response use ```-type=aaaa``` or ```-type=a```, as explained [here](https://stackoverflow.com/questions/52663711/how-should-i-interpret-a-cant-find-from-nslookup-inside-a-docker-busybox-c)
```
$ docker run busybox nslookup -type=a google.com
```

If you have a dns problem you could apply a temporary fix by running you docker containers etc with -dns 8.8.8.8 etc

A more permanent fix would be to change dockers dns/nameserver configuration, which is beyond the scope of this article.

### Concurrent Executions ###

When you allow concurrent executions, some measures need to be taken to guarantee that all steps executed within one pipeline are actually based on the same revision.

### Setting Docker container memory constraints ###

One thing you need to know about Java process memory allocation is that in reality it consumes more physical memory than specified with the -Xmx JVM option. The -Xmx option specifies only the maximum Java heap size. But the Java process is a regular Linux process and we are more interested in how much actual physical memory this process is consuming.

Or in other words – what is the Resident Set Size (RSS) value for running a Java process?

Theoretically, in the case of a Java application, a required RSS size can be calculated by:

RSS = Heap size + MetaSpace + OffHeap size

where OffHeap consists of thread stacks, direct buffers, mapped files (libraries and jars) and the JVM code itself.

There is a very good post on this topic: Analyzing java memory usage in a Docker container by Mikhail Krestjaninoff.

When using the --memory  option in docker run  make sure the limit is larger than what you specify for -Xmx.

### Best Practices ###

- Store your CICD configuration information in your source control management system
  * Use pipeline projects for all of our CICD pipelines, and any changes to the Jenksinfile in a repo is treated the same as any other change, requiring verification   etc.

- Use declarative pipeline as opposed to script

- Build and test each change before it’s merged, and don’t allow merges if the pipeline fails
  * Building each PR gives the author and reviewers an initial indication of code quality, and blocking merges prevents bad    changes from making it into production.
  * Use Jenkins’ Pipeline Multibranch Plugin. This plugin receives notifications of new branches and pull requests and automatically schedules a build for any that   have CICD information present.

- Pay attention to unit test code coverage and run unit tests as part of your pipeline.
  * Publish code coverage reports in Jenkins as part of our builds. Jenkins provides a trend coverage chart so you can see if coverage is starting to drop.
  * We aim for 90% line coverage or better, and fail PRs that drop below the coverage, or have Most code coverage packages offer a way to configure build thresholds,     but if yours doesn’t Jenkins coverage publishing plugins do (see the [Jenkins Cobertura plugin](https://wiki.jenkins.io/display/JENKINS/Cobertura+Plugin) as an   example).

- Write comprehensive end to end or integration tests, and run them as part of your pipeline.
  * Early in each project, we have a task to create an end-to-end test to use as a template, then require all stories to include appropriate tests before being closed.   It’s important that developers can easily run the tests locally, so part of setting up those tests is to provide any setup or scripts needed for a developer to run   and debug tests.
  * Our method of doing scrum also supports developing tests. We prefer writing stories using the template “As a I should be able to so that ". For example, "As an   admin I should be able to add a user to a group so that they can access the system".

- Run your CICD tests with the same binaries and environment they will use in production.
  * Our CICD pipelines build the docker image, publish it to an artifact repo, deploy a container to the same cloud service that will run the production service, then   run tests against the services running in that container.
  * We also make sure that development environments are able to replicate test runs via the same commands used during the Jenkins pipeline, so developers are able to   test their changes locally before submitting their PRs.

- Monitor your CICD pipelines.

- Have a plan in place for addressing problems in the CICD pipelines

- Have a plan in place for handling bad deployments

- If you’re using Jenkins, consider getting involved in its open source community

### References ###

- [Jenkins.io - What is Jenkins](https://jenkins.io/doc/)

- [Stackoverflow.com - Docker command not found](https://stackoverflow.com/questions/50916740/docker-command-not-found-in-local-jenkins-multi-branch-pipeline)

- [Stackoverflow.com - Building maven project with jenkins docker plugin](https://stackoverflow.com/questions/46446335/how-to-use-jenkins-docker-plugin-to-build-a-maven-project-inside-a-container)

- [Stackoverflow.com - NS busybox response interpretation](https://stackoverflow.com/questions/52663711/how-should-i-interpret-a-cant-find-from-nslookup-inside-a-docker-busybox-c)

- [Codefresh.io - Java Docker Pipeline](https://codefresh.io/docker-tutorial/java_docker_pipeline/)

- [Godaddy.com - CI CD Best Practices](https://uk.godaddy.com/engineering/2018/06/05/cicd-best-practices/)

- [Jenkins Cobertura plugin](https://wiki.jenkins.io/display/JENKINS/Cobertura+Plugin)
