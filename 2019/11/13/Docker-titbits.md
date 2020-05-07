---
path: "./2019/11/13/Docker-titbits.md"
date: "2019-11-13"
title: "Docker Titbits"
description: "Curated information on Docker"
lang: "en-us"
---

### Image Names ###
- Docker container and image names should be alpha-numeric [A-Za-z_-].
The names are used to create files on the file system... some file systems don't
play well with non alpha-numeric characters in names.

### Useful Commands ###
- Host ip.
Host could be viewed by running the following command as root user:
```
C:\WINDOWS\system32>docker-machine ip default
192.168.99.100
```

- List machines
```
C:\WINDOWS\system32>docker-machine ls
NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER     ERRORS
default   *        virtualbox   Running   tcp://192.168.99.100:2376           v18.09.7
```

- List processes
```
C:\WINDOWS\system32>docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                              NAMES
52c60eacd03d        jenkinsci/blueocean   "/sbin/tini -- /usr/…"   About an hour ago   Up About an hour    0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins-blueocean
```

- SSH into a Container
  * Use docker ps to get the name of the existing container.
  * Use the command docker exec -it <container name> /bin/bash to get a bash shell in the container.
  * Generically, use docker exec -it <container name> <command> to execute whatever command you specify in the container.
```
docker exec -u 0 -it <container_name_or_id> /bin/bash
```
Here, the -u 0 flag specifies that the root user with id 0 be used to run /bin/bash

### Notes on docker `build` command ###

Docker builds images in layers.

```
FROM node:10-alpine

LABEL maintainer="https://github.com/poshjosh <posh.bc@gmail.com>"

RUN mkdir ./config && ls -a

RUN ls -a

CMD ["/bin/bash"]
```

At step 3 i.e (`RUN mkdir ./config && ls -a`) the output of `ls -a` will be:

```
.
..
config
```

At step 4 i.e (`RUN ls -a`) the output of `ls -a` will be:

```
.
..
```

Actual command and output
```
docker build -t posjosh/jelly-bean .
Step 1/5 : FROM node:10-alpine
 ---> d9f9ac153d4e
Step 2/5 : LABEL maintainer="https://github.com/poshjosh <posh.bc@gmail.com>"
 ---> Using cache
 ---> a4d5e3f83375
Step 3/5 : RUN mkdir ./config && ls -a
 ---> Running in 031276dbcdc3
.
..
config
Removing intermediate container 031276dbcdc3
 ---> 4d084c8280d8
Step 4/5 : RUN ls -a
 ---> Running in dfa696788d39
.
..
---> f8a989e7962b
Step 5/5 : ENTRYPOINT ["./docker-entrypoint.sh"]
---> Running in d159420d7aae
Removing intermediate container d159420d7aae
---> e117d69b8a7d
Successfully built e117d69b8a7d
```

### Passing environment variables to docker run ###

```
$ docker run  --name <CONTAINER_NAME> -e JAVA_OPTS='-Xmx3g -Xms3g' <IMAGE_NAME>
```

### Useful for Docker java ###

Remember java syntax for executing a jar file?

```
java <JAVA SYSTEM PROPERTY> -jar <path/to/my/jar> <COMMAND LINE ARGS>
java -Dserver.port=7788 -jar <path/to/my/jar> --server-port=7788
```

Use the following command to unpack all jar files in target folder of workspace directory to the current directory:

```
sh "find ${WORKSPACE}/target -type f -name '*.jar' -exec jar -xf {} ';'"
```

### Don'ts ###

Avoid running containers with the `--privileged` flag as it gives all the
capabilities to the container and also access to the host’s devices
(everything that’s under the /dev folder).

If you run an image that does not come from a trusted source, be careful if it
requires the `--privileged` flag. Of course, there are cases when this flag
is needed, for instance when running containers on devices like Raspberry Pi.
It is indeed used to access the GPIO interfaces and to be able to interact
with the external world (and make an LED blink).
