# Intro to Docker

-

### Overview

* What is Docker?
* Getting Started

-
-

# What is Docker?

-

### What is Docker?

"Docker is a full development platform for creating containerized apps".

-
-

# Getting Started

-

#### Download Docker Desktop

1. Go to dockerHub (https://hub.docker.com/).
2. Create an account (https://hub.docker.com/signup).
3. Login with newly created account.
4. Download docker desktop (https://download.docker.com/mac/stable/Docker.dmg).

-

### Install Docker Desktop

1. Double click on the dmg file downloaded in previous step.
2. Follow the instructions on the wizard to install application.

-

#### Validate Docker Desktop Installation

1. Launch Docker Desktop
2. Login to Docker Desktop
3. Open Terminal
4. Type `docker version`. You should see information related to the docker installation.
5. Type `docker run hello-world`. This will download and execute a docker image, thus validating you are able to access docker hub and download images from the the hub.

-

#Doing stuff with Docker
  
-
# Unit: 1 Containers

###Configuration

-
### In Terminal, type : `docker version`

```
zcw$ docker version 
```
```shell
Client:
    Version: 17.09.0-ce 
    API version: 1.32
    Go version: go1.8.3 
    Git commit: afdb6d4
    Built: Tue Sep 26 22:40:09 2017 
    OS/Arch: darwin/amd64


Server:
    Version: 17.09.0-ce
    API version: 1.32 (minimum version 1.12) 
    Go version: go1.8.3
    Git commit: afdb6d4
    Built: Tue Sep 26 22:45:38 2017
```
-

### In Terminal, type : `docker info`

```
zcw$ docker info
```
```shell
Containers: 9 
    Running: 0
    Paused: 0 
    Stopped: 9 
    Images: 21
Server Version: 17.09.0-ce 
Storage Driver: overlay2
    Backing Filesystem: extfs
    Supports d_type: true 
Logging Driver: json-file 
Cgroup Driver: cgroupfs
Plugins:
    Volume: local
    Network: bridge host ipvlan macvlan null overlay
```
-
 
### In Terminal, type : `docker`

```
zcw$ docker
```
```shell
Management Commands: 
   checkpoint Manage checkpoints
   config Manage Docker configs
   container Manage containers
   image Manage images
   network Manage networks
   node Manage Swarm nodes
   plugin Manage plugins
   secret Manage Docker secrets
   service Manage services
   stack Manage Docker stacks
   ...    

 
```
-

### In Terminal, type : `docker` (continued)

```
zcw$ docker
```
```shell
Commands: 
   attach   Attach local standard input, output, and error streams to a running container
   build    Build an image from a Dockerfile
   commit   Create a new image from a container's changes
   cp       Copy files/folders between a container and the local filesystem
   create   Create a new container
   deploy   Deploy a new stack or update an existing stack
   diff     Inspect changes to files or directories on a container's filesystem 
   events   Get real time events from the server
   exec     Run a command in a running container
   export 
   history  Export a container's filesystem as a tar archive Show the history of an image
   images List images
 
 
   
```
-

### Docker command line structure

Previous versions of docker used :

    syntax : docker <mgt-command> (options) 
    example: docker run

Newer versions :

    syntax : docker <mgt-command> <sub-command> (options) 
    example : docker container run
    
    
    
-
 
### What we covered
<div style="text-align: left"> 
<ul>
		<li class="fragment fade-up">
**Command**: `docker version` 
<ul>
		<li class="fragment fade-up">Verified cli can talk to engine
</li>
</ul>
    </li>
</ul>
<br>
<ul>
		<li class="fragment fade-up">  
**Command**: `docker info`
<ul>
		<li class="fragment fade-up">Most config values of engine
</li></ul></li></ul>

<ul>
		<li class="fragment fade-up">
**Docker command line structure**    
<ul>
		<li class="fragment fade-up">Old (Still works): `docker <mgt-command> (options)`</li>
<li class="fragment fade-up">New: `docker <mgt-command> <sub-command> (options)`</li>
    </ul></li></ul>
</div>
-
 
## Unit: 1 Containers
#### Web Servers

-
## Image vs. Container

<ul>
		<li class="fragment fade-up">An **image** is the application we want to run</li>
<li class="fragment fade-up">A **container** is an instance of that image running as a process</li>
<li class="fragment fade-up">You can have many containers running off the same **image**</li>
<li class="fragment fade-up">In this unit our **image** will be the **Nginx** server</li>
<li class="fragment fade-up">Docker’s default image “registry” is called **Docker Hub** (https://hub.docker.com)</li>
</ul>
 
-
 
## Let’s start a new container


```
zcw$ docker container run --publish 80:80 nginx
```

```shell
   Unable to find image 'nginx:latest' locally
   latest: Pulling from library/nginx [...]
```

Then, open up your browser of choice and go to http://localhost:80 (refresh your screen a few times and look at the logs)

<ul>
		<li class="fragment fade-up">Downloaded **image** ‘nginx’ from **Docker hub**</li>
		<li class="fragment fade-up">Started a new **container** from that **image**</li></ul>

-
  
  ### Option: `--publish 80:80`
<ul>
		<li class="fragment fade-up">Opened port **80** on the host **container** IP</li>
		<li class="fragment fade-up">Routes that traffic to the **container** IP, port **80**</li></ul>

-
 
### Let’s start a new **DETACHED** container

```
zcw$ docker container run --publish 80:80 --detach nginx
```
```shell  b2096a61e9464c2a60bb702545e7d5d4466e0f1f06ea178431d9ff6aec4f8532
```

Then, open up your browser of choice and go to http://localhost:80



-

### Lets list all of our containers

```
zcw$ docker container ls
```
```shell
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
b2096a61e946 nginx "nginx -g 'daemon ..." About a minute ago Up About a minute 0.0.0.0:80->80/tcp suspicious_jones```

-
 
### Let’s stop our containers

```
zcw$ docker container stop b2
```

```shell
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
b2096a61e946 nginx "nginx -g 'daemon ..." About a minute ago Up About a minute 0.0.0.0:80->80/tcp suspicious_jones
```

To stop the container, you only have to provide enough of the container id for it to be unique.

-

### Let’s list all of our containers

```
zcw$ docker container ls
```
```shell
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES

```

Since no containers are currently running, none will be listed.


-

### Let’s list all of our containers

```
zcw$ docker container ls -a
```
```shell
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
b2096a61e946 nginx "nginx -g 'daemon ..." 5 minutes ago Exited (0) 2 minutes ago  suspicious_jones
1d383263da25 nginx "nginx -g 'daemon ..." 22 minutes ago Exited (0) 5 minutes ago  unruffled_mccarthy

```

Adding the `-a` flag will list all the containers, even the ones currently not running.


-

### Let’s start a new DETACHED container

```
zcw$ docker container run --publish 80:80 --detach --name ZCW nginx
```
```shell
83831fe7c024246e2ffb4f6caa0fdf7d7f35d5c632133b4d09908466d44e25cc
```

```
zcw$ docker container ls
```
```shell
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES 
83831fe7c024 nginx "nginx -g 'daemon ..." 33 seconds ago Up 34 seconds 0.0.0.0:80->80/tcp ZCW
``` 

-

### Let’s look at our logs

(Before doing the next step, you will need to refresh a few times on http://localhost:80 to generate logs) 

```
zcw$ docker container logs ZCW
```
```shell
172.17.0.1 - - [13/Dec/2017:22:28:33 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36" "-"
172.17.0.1 - - [13/Dec/2017:22:29:21 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36" "-"
172.17.0.1 - - [13/Dec/2017:22:29:23 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36" "-"
172.17.0.1 - - [13/Dec/2017:22:29:25 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36" "-"
```  

-

### Let’s look at our processes

```
zcw$ docker container top ZCW
```

```shell
PID    USER TIME COMMAND
15287  root 0:00 nginx: master process nginx -g daemon off;
15315  101  0:00 nginx: worker process
```

-

### Let’s clean up

```
zcw$ docker container ls -a
```
```shell
CONTAINER ID IMAGE COMMAND                CREATED        STATUS                     PORTS              NAMES
83831fe7c024 nginx "nginx -g 'daemon ..." 6 minutes ago  Up 6 minutes               0.0.0.0:80->80/tcp ZCW
b2096a61e946 nginx "nginx -g 'daemon ..." 14 minutes ago Exited (0) 11 minutes ago                     suspicious_jones
1d383263da25 nginx "nginx -g 'daemon ..." 31 minutes ago Exited (0) 14 minutes ago                     unruffled_mccarthy

```
```
zcw$ docker container rm 838 b20 1d3
```

(Some docker commands can be chained together to do multiple actions)


-

### Let’s clean up : We have an error
```
zcw$ docker container rm 838 b20 1d3
```

```shell
Error response from daemon: You cannot remove a running container 83831fe7c024246e2ffb4f6caa0fdf7d7f35d5c632133b4d09908466d44e25cc.
```
You can not remove a running container , you must either stop the container , or use the `-f` flag to force the removal.

`zcw$ docker container rm 838 -f`
 

-

### What happens in ‘docker container run’
<ul>
		<li class="fragment fade-up">Looks for the **image** locally in image cache , to see if it's there</li>
<li class="fragment fade-up"> Then looks in remote **image** repository ( defaults to Docker Hub )
</li>
<li class="fragment fade-up">Downloads the latest version (Nginx:latest by default)
</li>
<li class="fragment fade-up"> Creates new **container** based on that **image** and prepares to start
</li>
<li class="fragment fade-up">Gives it a virtual **IP** on a private network inside docker engine
</li>
<li class="fragment fade-up">Opens up port **80** on host and forwards to port **80** in **container**
</li>
<li class="fragment fade-up">Start **container** by using the CMD in the image **Dockerfile**
 </li></ul>

-

### Example of Changing the Defaults

```
docker container run --publish 8080:80 --name ZCW -d nginx:1.11 nginx -T
```

<ul>
		<li class="fragment fade-up">8080:80 (change host listening port) 
		</li>
<li class="fragment fade-up">Nginx:1.11 (change version of image)		</li>
<li class="fragment fade-up">Nginx -T (change CMD run on start)</li></ul>


-

### Containers aren’t Mini-VM’s
<ul>
		<li class="fragment fade-up"> They are just processes </li>
<li class="fragment fade-up"> Limited to what resources they can access </li>
<li class="fragment fade-up"> Exit when process stops</li></ul>
 

-

### Let’s start up a mongo database
```
zcw$ docker container run --name mongo -d mongo
```

```shell
Unable to find image 'mongo:latest' locally 
latest: Pulling from library/mongo
c4bb02b17bb4: Pull complete 
3f58e3bb3be4: Pull complete 
a229fb575a6e: Pull complete 
633f2d5861aa: Pull complete 
7f55dbb7ff1d: Pull complete
[...]
``` 

-

### Let’s list out all our running containers
```
zcw$ docker ps
```
```shell
CONTAINER ID IMAGE COMMAND                CREATED       STATUS       PORTS     NAMES 
103411b8f3f4 mongo "docker-entrypoint..." 8 minutes ago Up 8 minutes 27017/tcp mongo
 ```

-

### This is a process on the host
```
zcw$ docker top mongo
```
```shell
USER PID %CPU %MEM    VSZ   RSS TT STAT    STARTED TIME COMMAND 
931  0.0 0.1  5117464 23524 ??  S  10:56AM 0:37.56
```
This is not a VM, this is actually a process on the local system.
 

-

### Let’s list the processes in our containers:
```
$ docker top mongo
```
```shell
PID  USER TIME COMMAND
2405 999  0:01 mongod --bind_ip_all
``` 

-

### Lab: Manage Multiple Containers (Part 1)
- http://docs.docker.com and `--help` are your friends
- Run a **nginx**,
- Run it in `--detach` ( or `-d`), name them with `--name`
- **Nginx** should listen on **80:80**
 

-

### Lab: Manage Multiple Containers (Part 2)
- http://docs.docker.com and `--help`are your friends
- Run a **http**(**apache**) server
- Run it `--detach` ( or `-d`), name them with `--name`
- **http** should listen on **8080:80**
 

-

### Lab: Manage Multiple Containers (Part 3)
- http://docs.docker.com and `--help` are your friends
- Run **mysql**
- Run it `--detach` ( or -d), name them with `--name`
- **Mysql** should listen on **3308:3306**
- When running **mysql**, use the `--env` option (or `-e`) to pass in
`MYSQL_RANDOM_ROOT_PASSWORD=yes`
- Use docker container **logs** on **mysql** to find the random password it created
on startup
 

-

### Lab: Manage Multiple Containers (Part 4)
- http://docs.docker.com and `--help` are your friends
- Clean it all up with `docker container stop` and `docker container rm`
(both can accept multiple names of ID’s)
- Use `docker container ls` to ensure everything is correct before and after clean up
 

-

Next...

## Unit: 1 Containers
CLI Monitoring
  