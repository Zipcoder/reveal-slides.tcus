# Intro to Docker
### Unit: 1 Containers
CLI Monitoring
  

-

### What’s going on in Containers:
● docker container top - process list in one container
● docker container inspect - details of one container config
● docker container stats - performance stats for all containers
 

-

### Let’s start a nginx container
● docker container run -d --name nginx nginx
● docker container top nginx
 

-

### Let’s start a mysql container
● docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql
● docker container top mysql
 

-

### Docker container inspect
● docker container inspect mysql
● This will return a JSON array of all the data involved in starting up the
container
 

-

### Docker container stats
● docker container stats
● This will give you a running play on the processes running in containers on
your machine
● This is not what you would use in production
● It’s great for when you are working on your local machine
 

-

### Getting a Shell inside Containers
● docker container run -it
○ starts new containers interactively
● docker container exec -it
○ run additional command in existing container
● Different Linux distros in containers
 

-

###  Container flags `-i` , `-t` or `-it`
● -t pseudo-tty
○ Simulates a real terminal, like what SSH does
● -i, --interactive
○ Keep STDIN open even if not attached
○ This allows us to keep the session open even when there are no commands
 

-

### Container additional commands
● usage : docker container run [OPTIONS] IMAGE [COMMAND] [ARGS...]
 ● docker container run -it --name proxy nginx bash
○ Here by placing bash after the image name ‘nginx’ we are overriding the default action of
this container
○ This will log you in as the root user of the container
○ Try using ls
○ Using the exit command to leave the container and end.
○ Containers only run as log as the command on startup


-

### Let’s pull down a full distribution
● docker container run -it --name ubuntu ubuntu
○ Once logged in run apt-get update
○ This is a stripped down version of Ubuntu , and would not contain all the things that comes
with a full distribution by default.
○ You can even install things normally apt-get install -y curl
○ Once you exit the container again... it will stop the container
○ If we restarted the container CURL would be installed
 

-

### Docker container start
● docker container start --help
● -a, --attach :: Attach STDOUT/STDERR and forward signals
● docker container start -ai ubuntu
 
  Docker container exec
● Let's say we want to look inside of an container already running a process
● docker container exec -it mysql bash
○ Will place you in a container inside of sql
○ In this shell we can jump directly into the mysql command line
○ Try ps aux
○ When you finally exit the process will continue
○ run docker ps
 

-

### Linux Alpine
● A small security focused distribution of linux
● Lets pull down a copy of alpine and take a look
○ docker pull alpine
○ docker image ls
● Let’s try :
○ docker container run -it alpine bash
○ The above will not work because bash is not part of the distribution
● Lets try:
○ docker container run -alpine sh
○ The above will work because sh is include in this image, although it has less features
available than bash.
 

-

# Unit: 1 Containers
### Networking Concepts
  

-

### Docker Networks: Concepts
● Review of docker container run -p
● For local dev/testing, networks usually “just work”
○ Dockers motto Batteries are included but removable
● Quick porr check with docker container port <container>
● Learn concepts of Docker Networking
 

-

### Docker Network Defaults
● Each container connected to a private virtual network “bridge” ○ This is the default docker system network
● Each virtual network routes through NAT firewall on host IP
○ The docker daemon configures the host ip address on its default interface so that
containers can get out to the internet
● All containers on a virtual network can talk to each other with -p
● Best practice is to create a new virtual network for each app
○ Network “zcw_web_app” for mysql and php/apache containers
○ Network “zcw_api” for mongo and nodejs containers
 

-

### Docker Networks Cont.
● “Batteries Included, But Removable”
○ Default work well in many cases, but easy to swap out parts to customize it
● Things you can change
○ Make new virtual networks
○ Attach containers to more than one virtual network (or none)
○ Skip virtual networks and use host IP (--net=host)
■ You lose contanerization benefits but it’s unavoidable ○ Use different Docker network drivers to gain new abilities
 

-

### `-p (--publish)`
● Publishing ports is always in HOST:CONTAINER format
● RUN: docker container run -p 80:80 --name webhost -d nginx
● RUN: docker container port webhost
○ 80/tcp -> 0.0.0.0:80
 

-

### Inspect `--format`
● docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost ○ Will return the ip address of the container ‘172.17.0.3’
● Run: ifconfig en0
○ Will return the ip address of local machine ‘10.0.0.92’
○ Notice that the two machines are not on the same network
○ There is an edge firewall that blocks calls in and out
○ Docker has a default bridge that maps to our local ethernet interface
○ Using the -p on docker will allow external traffic into the docker virtual network
○ Containers on the same network have access to each other, unless you use -p there will be
no incoming calls.
 

-

### Docker Networks: Concepts recap
● Review of docker container run -p
● For local/dev testing, networks usually “just work”
● Quick port check with docker container port <container>
● Learn concepts of Docker networking
 

-

# Unit: 1 Containers
### CLI Management
  

-

### Docker Networks : CLI Management
● Show networks docker network ls
● Inspect a network docker network inspect
● Create a network docker network create --driver
● Attach a network to a container docker network connect
● Detach a network from container docker network disconnect
 

-

### Docker Networks
● Run : docker network ls
● Run : docker network inspect bridge
○ Will list the containers that are attached to this network
● Three default networks
○ Host network - a special network that skips virtual networks but sacrifices security of a
container
○ Bridge network - default network for docker host
○ None network - it has not attachment
 

-

### Docker Networks
● Run : docker network create zcw_app_network
● Run : docker network ls
○ We can now see our new network with a driver of bridge ■ Bridge is the default network driver
● Run : docker network create --help
● Run : docker container run -d --name new_nginx --network zcw_app_network nginx
● Run : docker network inspect zcw_app_network
 

-

### Docker Networks :
● Docker network connect
○ Dynamically creates a NIC (networking interface card) in a container on an existing virtual
network
● Run : docker network ls
● Run : docker network inspect bridge
 

-

### Lab : CLI Testing
● Use different Linux distro containers to check curl cli tool versions
● Use two different terminal windows to start bash in both centos:7 and
ubuntu:14.04, using -it
● Use the docker container --rm options so you can save cleanup
● Ensure curl is installed and on latest version for that distro
○ ubuntu : apt-get update && apt-get install curl
○ Centos : yum update curl
● check curl --version
 
