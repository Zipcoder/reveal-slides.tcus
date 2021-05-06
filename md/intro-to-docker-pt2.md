# Intro to Docker
## Unit 2: Container Images
  

-

### Unit 2 - Container Images : Agenda
  ● Define what an Image is
● Docker HUB
● Image Cache
● Tagging
● Docker File ○ Building
○ Extending
 

-

### Unit 2 - Container Images : Overview
  ● All about images, the building blocks of containers
● What's in an image (and what isn't)
● Using Docker Hub registry
● Managing our local image cache
● Building our own images


-

### What’s an Image?
● Application binaries and dependencies
● Metadata about the image data and how to run the image
● Not a complete OS. No Kernel... the host provides the Kernal
● Small as one file (your application binary) like in golang
● Or something like Ubuntu
 

-

### Docker Hub
● Basics of Docker Hub (hub.docker.com)
○ Github for docker images
○ Create an account on hub.docker.com
● Find official and other good public images
○ Official images are the only ones without forward slashes on them
● Download images and basics of image tags
 

-

### Union file system
● Unionfs is a filesystem service for Linux, FreeBSD and NetBSD which implements a union mount for other file systems.
● It allows files and directories of separate file systems, known as branches, to be transparently overlaid, forming a single coherent file system.
● Contents of directories which have the same path within the merged branches will be seen together in a single merged directory, within the new, virtual filesystem.
 

-

### Docker Images and Layers
● Image layers
● Union file system
● History and inspect commands
● Copy on write
○ How containers run as expansions on top of images
 

-

### Creating and Storing Container Images
● Run : docker image ls
● Run : docker history nginx:latest
○ This is not a history of what’s in the container
○ This is a history of the image changes over time
○ Every Image starts off as a basic layer called a scratch file
● Run : docker history mysql
○ Look at the difference
● Some points
○ Every layer gets its own unique sha
○ Docker uses this to check to see what changes are between each version
 

-

### Docker inspect
● Run : docker image inspect nginx
○ This will show you all the default commands in the image
○ Remember most of these can be changed after the fact
 

-

### Container Images
● Images are made up of file system changes and metadata
● Each layer is uniquely identified and only stored once on a host
● This saves storage space on hos and transfer time on push/pull
● A container is just a single read/write layer on top of image
● docker image history and inspect command
 

-

### Image Tagging and Pushing to docker hub
● Run : docker image tag --help
● Run : docker image ls
○ Images don’t have a name
■ They have an ID and a Tag
■ They exist inside of a repository and are listed by their Tag and Image
● Tags are a pointer to an specific image commit
○ Let’s navigate to docker hub and look at images and how they are tagged
● Run : docker pull nginx:mainline
● Run : docker image ls
 

-

### Image Tagging and Pushing to docker hub
● Run : docker image tag --help
● Run : docker image ls
○ Images don’t have a name
■ They have an ID and a Tag
■ They exist inside of a repository and are listed by their Tag and Image
● Tags are a pointer to an specific image commit
○ Let’s navigate to docker hub and look at images and how they are tagged
● Run : docker pull nginx:mainline
● Run : docker image ls
 

-

### Tagging images
● docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
● Run : docker image tag nginx <your-username>/nginx
● Try to run : docker image push <your-username>/nginx
○ Since you are not logged in you will not be able to
● Create an account on docker hub
● Run : docker login
● Run : cat .docker/config.json
● Run : docker image push <your-username>/nginx
 

-

### Retagging Images
● Run : docker image tag <your-username>/nginx <your-username>/nginx:testing
● Run : docker image ls
○ Pay attention to the image id
● Run : docker image push <your-username>/nginx:testing
 

-

### Dockerfile
● Navigate to the dockerfile-sample-1 in the git repo
● Open up the file ‘Dockerfile’ and review it
● Run : docker image build -t localimage .
● Run : docker image ls
● Let’s make a update and the expose port 8080 to the Dockerfile
● Rebuild the file
○ Notice the Using cache tag line in the build process
○ Its critically important the ordering of your flies
■ Keep the things you change the least at the top
 

-

### Copying files to build
● Run: docker container run -p 80:80 --rm nginx
● Open up browser and webhost
● Run : docker image build -t nginx-with-html .
● Navigate to browser
 

-

### Push to docker hub
● Let’s retag and send to docker hub
● Run :docker image tag nginx-with-html:latest <your-username>/nginx-with-html:latest
● Run : docker push <your-username>/nginx-with-html:latest
 

-

### Lab : Build your Own Image
● Take existing Node.js app and Dockerize it
● Make Dockerfile.
○ Build it
○ Test it
○ Push it
○ Run it
● Expect this to be iterative. You seldom get it right the first time
● Details in dockerfile-assignment-1/Dockerfile
● Use the Alpine version of the official ‘node’ 6x image
● Expected result is the web site at http://localhost
● Remove from local cache, run again from Hub
  

-

### Unit 2: Container Images
Docker HUB
  
  Docker Hub
To access Docker HUB, browse to: hub.docker.com (ie, nginx official) Official images, advantages, versions etc.
 

-

### 2:2
Commands:
● docker search
● docker pull
● docker login
● docker push
Download images: hub.docker.com (ie, nginx official)
 
  docker search
docker search [OPTIONS] TERM
 
  docker pull
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
 
  docker login
docker login [OPTIONS] [SERVER]
 
  docker push
docker push [OPTIONS] NAME[:TAG]
 
  Image Cache
docker image ls
docker pull nginx
docker pull nginx:1.11.9
(when testing, specify the version) docker history nginx:latest
docker image inspect nginx
 

-

# Unit 2: Container Images
#### Tagging
  

-

## Tagging
  

-

### Tagging
<repository>/<tag_name> (by default is “latest”> docker image tag --help
docker pull mysql/mysql-server
docker pull nginx:mainline
docker image tag nginx doliothesleuth/nginx
docker image image push doliothesleuth/nginx (if denied, then:) docker login
(then push again) (to add a verion to the tag, use :<tag_version>)
Dockerfile:
 

-

# Unit 2: Container Images
### Docker File
  

-

### Docker File
- A recipe for creating your image
File’s name is alway “Dockerfile”, contains the recipe for building the image. docker image build -t <image_name> . (“.” if called from inside the directory that contains the desired Dockerfile)
Tips: things that change the least at top of file, things that change the most towards the bottom
 

-

### Docker File: Build
Building:
“FROM <repo>:<tag>”
“EXPOSE <portnumbers>”
“RUN apk add --update tini”
“WORKDIR <path the workdir>”
“COPY <host files or dir> <destination>” “CMD []“
 