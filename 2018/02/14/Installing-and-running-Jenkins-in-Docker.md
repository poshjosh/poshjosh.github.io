---
path: "./2019/02/14/Installing-and-running-Jenkins-in-Docker.md"
date: "2019-02-14T13:45:56"
title: "Installing and running Jenkins in Docker"
description: "Easy to understand tutorial: Installing and running Jenkins in Docker"
lang: "en-us"
---

#### Background ####

Jenkins can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software. Read a brief [Introduction to Jenkins](/2018/02/05/Introduction-to-Jenkins), if you haven't done that already.

### Downloading and Running Jenkins in Docker ###

- Create a bridge network in Docker.

Open command prompt and type the following docker network create command:

```
C:\WINDOWS\system32>docker network create jenkins
22adcb99b9197d6ebdec8b601bb165e8dAycum786c29091ee612c1489f2lo09w
```

- Create volume for Docker client TLS certificates.

The volume will be used to share the Docker client TLS certificates needed to connect to the Docker daemon:

```
C:\WINDOWS\system32>docker volume create jenkins-docker-certs
jenkins-docker-certs
```

-  Create volume for persisting jenkins data.

Use the following command:
```
C:\WINDOWS\system32>docker volume create jenkins-data
jenkins-data
```

- Download and run docker-in-docker image.

In order to execute Docker commands inside Jenkins nodes, download and run the docker:dind Docker image using the following docker container run command:
```
C:\WINDOWS\system32>docker container run --name jenkins-docker --rm --detach --privileged --network jenkins --network-alias docker --env DOCKER_TLS_CERTDIR=/certs --volume jenkins-docker-certs:/certs/client --volume jenkins-data:/var/jenkins_home docker:dind

unable to find image 'docker:dind' locally
dind: Pulling from library/docker
c9b1b535fdd9: Pull complete
.
.                                                                                                          sha256:a4f33d003b7ec9133c2a1ff61f4e80305b329c0fa8b753140b9ab2808f28328c
Status: Downloaded newer image for docker:dind
22adcb99b9197d6ebdec8b601bb165e8dAycum786c29091ee612c1489f2lo09w
```

-Download and run the jenkinsci/blueocean image.

_I changed my port binding from 8080:8080 to 8090:8090. This result in some problems in initial startup via the web browser in windows 10. So I had to revert back to 8080._
```
C:\WINDOWS\system32>docker container run --name jenkins-blueocean --rm --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro --publish 8080:8080 --publish 50000:50000 jenkinsci/blueocean

Unable to find image 'jenkinsci/blueocean:latest' locally
latest: Pulling from jenkinsci/blueocean
e7c96db7181b: Already exists
f910a506b6cb: Already exists
.
.
f58b5d821a07: Pull complete
Digest: sha256:bbo8uwhadcc99b9197d6ebdec8b601bb165e8dAycum786c29091ee612c1489f2lo09w
Status: Downloaded newer image for jenkinsci/blueocean:latest
cef7d6ebdec8b601bb165e8dAycum786c29091ee612c1489f2lo09w
```

- Access jenkins
Browse to http://localhost:8080

- Access the initial admin password.

To access the required initial password, run following command.
```
docker container logs jenkins-blueocean
```

Some where in the logs you will find the password between some lines of asterix as shown:
```
*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

2c4b5c8dBbAx01a7a8dc1p0o33337e

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************
```

After entering the password you will be prompted to install plugins.

*Save this password - you may need it if one or more errors occur during installation*

### If the process of installing plugins returns the following error: ###
```
An error occurred during installation no such plugin cloudbees-folder
```

Then stop the docker jenkins-blueocean container and re-run it, but this time without mapping the jenkins-data as we did previously via the following expression:
```
--volume jenkins-data:/var/jenkins_home
```

Here is the stop command followed by the run command
```
C:\WINDOWS\system32>docker container stop jenkins-blueocean
C:\WINDOWS\system32>docker container run --name jenkins-blueocean --rm --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-docker-certs:/certs/client:ro --publish 8080:8080 --publish 50000:50000 jenkinsci/blueocean
```

- Access jenkins
Browse to http://localhost:8080

Use command ```docker container logs jenkins-blueocean``` to view the password as done earlier.

*Save this password*

After successfully browsing to jenkins.. you have to re-run the container again with the mapping of the jenkins-data.

```
C:\WINDOWS\system32>docker container stop jenkins-blueocean
C:\WINDOWS\system32>docker container run --name jenkins-blueocean --rm --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro --publish 8080:8080 --publish 50000:50000 jenkinsci/blueocean
```

Enter the first password you saved.

The plugins should install successfully now.

*You no longer need to keep the saved passwords*

Note: Info by Jenkins
```
You have skipped the setup of an admin user.

To log in, use the username: "admin" and the administrator password you used to access the setup wizard.
```
------------------

### If you use docker in windows via Docker ToolBox and browser complains that it can't reach the server ###

Don't check http://localhost:8080 in your browser. This is because, that is not the actual host. The actual host is running in Vmware and the IP address of the actual host could be viewed by running the following command as root user:
```
C:\WINDOWS\system32>docker-machine ip default
192.168.99.100
```

Now access jenkins via ```192.168.99.100:8080```


List machines
```
C:\WINDOWS\system32>docker-machine ls
NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER     ERRORS
default   *        virtualbox   Running   tcp://192.168.99.100:2376           v18.09.7
```

List processes
```
C:\WINDOWS\system32>docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                              NAMES
52c60eacd03d        jenkinsci/blueocean   "/sbin/tini -- /usr/…"   About an hour ago   Up About an hour    0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins-blueocean
```

- SSH into a Container

  - Use docker ps to get the name of the existing container.
  - Use the command docker exec -it <container name> /bin/bash to get a bash shell in the container.
  - Generically, use docker exec -it <container name> <command> to execute whatever command you specify in the container.

```
docker exec -u 0 -it <container_name_or_id> /bin/bash
```
Here, the -u 0 flag specifies that the root user with id 0 be used to run /bin/bash

When we ran a Dockerfile which attempted to download maven:3-alpine, I encountered the following exception:
```
C:\WINDOWS\system32>docker pull maven:3-alpine
error during connect: Post https://docker:2376/v1.39/images/create?fromImage=maven&tag=3-alpine: dial tcp: lookup docker on 127.0.0.11:53: no such host
script returned exit code 1
```

The problem was the dns of ```127.0.0.11```. So change it via the following run command, with google public dns of 8.8.8.8 incorporated:
```
C:\WINDOWS\system32>docker container run --name jenkins-blueocean --rm --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro --publish 8080:8080 --publish 50000:50000 --dns 8.8.8.8 --dns 127.0.0.11 jenkinsci/blueocean
```

I got a warning this time, as follows:
```
WARNING: Localhost DNS setting (--dns=127.0.0.11) may fail in containers.
8de9587bb1c053a63175661608753d581306d5ac53a192069eb6738cd9636c8f
```

So we stop the container and re-run with 8.8.8.8 and 8.8.4.4, both google public dns:
```
C:\WINDOWS\system32>docker container run --name jenkins-blueocean --rm --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro --publish 8080:8080 --publish 50000:50000 --dns 8.8.8.8 --dns 8.8.4.4 jenkinsci/blueocean
```

### References ###

- [Jenkins.io - Build Java Maven app](https://jenkins.io/doc/tutorials/build-a-java-app-with-maven/)

- [Jenkins.io - Installing](https://jenkins.io/doc/book/installing/#downloading-and-running-jenkins-in-docker)
