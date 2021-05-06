
# Intro to Docker
## Unit 3: Container Life Cycle
  

-

### Unit 3-Container Life Cycle : Agenda
 ● Container Lifetime
● Persistent Data
○ Volumes
○ Bind mounting
 

-

### Unit 3-Container Life Cycle : Overview
● Defining the problem of persistent data
● Key concepts with containers: immutable, ephemeral
● Learning and using Data Volumes
● Learning and using Bind Mounts
 

-

### 3: Container Lifetime & Persistent Data
Containers are usually immutable and ephemeral, giving us an "immutable infrastructure", where we only re-deploy containers, which never change.
This is the ideal scenario, however in order to accommodate databases or other unique data, Docker gives us features to ensure "separation of concerns". This collection of features are known as as "persistent data", handled in two ways: Volumes and Bind Mounts.
Volumes make a special location outside of the container Union File System, while Bind Mounts link a container path to a host path.
 

-

### The Twelve Factors
1. Codebase
2. Dependencies
3. Config
4. Backing services
5. Build, release, run
6. Processes
7. Port binding
8. Concurrency
9. Disposability
10. Dev/prod parity
11. Logs
12. Admin processes
 

-

### The Twelve Factors (continued)
1. Codebase
One codebase tracked in revision control, many deploys
2. Dependencies
Explicitly declare and isolate dependencies
3. Config
Store config in the environment
4. Backing services
Treat backing services as attached resources
 

-

### The Twelve Factors (continued)
5. Build, release, run
Strictly separate build and run stages
6. Processes
Execute the app as one or more stateless processes
7. Port binding
Export services via port binding
8. Concurrency
Scale out via the process model
 

-

### The Twelve Factors (continued)
9. Disposability
Maximize robustness with fast startup and graceful shutdown
10. Dev/prod parity
Keep development, staging, and production as similar as possible
11. Logs
Treat logs as event streams
12. Admin processes
Run admin/management tasks as one-off processes
 

-

# Unit 3: Container Life Cycle
### Persistent Data
  

-

### 3: Persistent Data: Volumes
● This usage is achieved by using the VOLUME command in the Dockerfile
● Can also be implemented as an override using the –v option in the docker
run command. (docker run -v /path/in/container )
● Bypasses Union File System and stores in alternative location on host
● Includes its own management commands under docker volume
● Connect to none, one, or multiple containers at once
● Not subject to commit, save, or export commands
● By default they only have a unique ID, but you can assign name, making it a
"named volume".
 

-

### Creating a Data Volume:
This method uses a container as a shared data volume and called data only container.
zcw$ docker run --name datavolume -v /var_volume1 -e POSTGRES_USER=pgsql -e POSTGRES_PASSWORD=mypass postgres
zcw$ docker run --name client1 --volumes-from datavolume postgres zcw$ docker run --name client2 --volumes-from datavolume postgres
(creates two postgres containers that share the same data volume container)
* Remember to use the same base image


-

### 3: Persistent Data: Bind Mounting
● Maps a host file or directory to a container file or directory
● Essentially, it’s two locations pointing to the same file(s)
● Like Data Volumes, skips Union File System, and host files overwrite any in
container
● Can't be enacted by the Dockerfile, must be used with container run:
... run -v /Users/<username>/stuff:/path/container (mac/linux) ... run -v //c/Users/<username>/stuff:/path/container (windows)


-

### Mount a host directory as a data volume:
By using the –v option in the run command, you can create a directory or map an already existing one to a container.
Example:
zcw$ docker run --name myname -v Shared:/SharedData -i -t ubuntu /bin/bash
(This creates a container with a shared folder with the host Shared Folder and will be mounted as /SharedData in the container, and if the folder didn’t exist in the host it will be automatically created.)

  Mount a host directory as a data volume:
The other option is giving a name to the container, opening its command shell with bash shell.
zcw$ docker inspect myname ...
"Mounts": [
{
"Source": Shared", "Destination": "/SharedData", "Mode": "",
"RW": true
} ]
 ...

  Mount a host directory as a data volume:
zcw$ ls ...
SharedData ...
If anything is written in the SharedData Volume, it will be written in the Shared volume
 

-

### Lab: Working with Named Volumes
● Task: Performing a PostGres Database Upgrade using Named Volumes, starting with version 9.6.1 (See docker hub if needed)
● Database upgrade with containers
● Create a Postgres container with name volume sql-data using version 9.6.1
● Use Docker hub to learn the VOLUME path and versions needed top run it
● Check the logs, and stop the container.
● Create a new postgres container with the same named volume using version 9.6.2
● Check the logs to validate that the container is using the newer version.
(*this only works with patch versions, most SQL databases require manual commands to upgrade to major/minor versions--it’s a limitation of databases not containers)
 

-

### Lab: Working with Bind Mounts
● Use a Jekyll "Static Site Generator" to start a local web server
● Don't have to be a web developer: this is an example of bridging the gap between local file access
and apps running in containers.
● Source code provided in bindmount-lab
● Edit it with your editor of choice
● The Container detect changes with the host files and updates the same on the web server
● Start container using: docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve
(The repo for Jekyll server in a docker container)
● Refresh browser to see changes
● Change the file in the _posts\ directory and refresh browser to see the changes.
● Look at logs to observe the operations
 
