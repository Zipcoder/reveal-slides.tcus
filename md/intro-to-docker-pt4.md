
# Intro to Docker
## Unit 4: Docker Compose
  

-

### Unit 4- Docker Compose: Agenda
● Creating docker-compose.yml files
● Compose Commands
● Adding Image Building
 

-

### Unit 4- Docker Compose: Overview
● Used to configure relationships between containers, as a container will often need to work with other containers.
● Used to save container run settings in an easy-to-read file
● Used to create one-line developer environment startups
● It is a two-part operation, consisting of:
○ YAML file with config for all relationships and dependencies (docker-compose.yaml).
Describes solutions for containers, networks, and volumes.
○ CLI tool , docker-compose, used for local dev/test automation using the YAML files.
 

-

### Docker Compose: YAML file (Format)
version: '3.1' # if no version is specified, then v1 is assumed. Recommend v2 minimum
services: # containers. same as docker run
servicename: # a friendly name. this is also DNS name inside network
image: # Optional if you use build:
command: # Optional, replace the default CMD specified by the image environment: # Optional, same as -e in docker run
volumes: # Optional, same as -v in docker run
servicename2:
volumes: # Optional, same as docker volume create networks: # Optional, same as docker network create
 

-

### Docker Compose: YAML file (example)
version: '2' # if no version is specified, then v1 is assumed. Recommend v2 minimum services:
wordpress:
image: wordpress
ports: #(same as using the “-p” option on the command line)
- 8080:80
environment: #(same as using the “-e” option on the command line)
WORDPRESS_DB_HOST: mysql WORDPRESS_DB_NAME: wordpress WORDPRESS_DB_USER: example WORDPRESS_DB_PASSWORD: examplePW
volumes:
- ./wordpress-data:/var/www/html
#(continued on next slide)
 
 Docker Compose: YAML file (example contin.)
#(continued from previous slide...)
mysql:
image: mariadb environment:
MYSQL_ROOT_PASSWORD: examplerootPW MYSQL_DATABASE: wordpress MYSQL_USER: example MYSQL_PASSWORD: examplePW
volumes:
- mysql-data:/var/lib/mysql
volumes: mysql-data:
  

-

# Unit 4: Docker Compose
### `docker-compose` and the CLI
  

-

### `docker-compose` CLI (basic commands)
Most common commands:
● docker-compose up: setup volumes/networks and start all containers
● docker-compose down: stop all containers and remove containers, volumes,
and networks (cleanup) Also:
● docker-compose ps: shows you which containers are running
● docker-compose top: lists all the services running inside
● docker-compose logs: view logs when run is used with the -d option


-

### Lab: Compose File for a Multi-Container Service
Build a basic compose file for a Drupal content management system (CMS) website. See Docker Hub to get the image info.
● Use Drupal image along with the Postgres image for the database. (Documentation on Hub as well)
● Use ports to expose Drupal on 8080 so you can access in browser via http://localhost:8080
● Be sure to set the environment variable POSTGRES_PASS for postgres. You may also choose to
set a value for POSTGRES_USER and POSTGRES_DB, though not necessary, as they will be "postgres" by default.
● Walk through Drupal installation in browser.
● Installation tip: Drupal will assume the DB host is localhost, however the proper name is the its
service name.
● Bonus action: Use volumes to store Drupal's unique data
 (*will likely be a small file, roughly 10 or so lines)


-

### Using Compose to Build
● Compose can also build your custom images
● If not found in cache, it will build them with docker-compose up.
● Use docker-compose build to rebuild the image.
● Useful for complex builds that have numerous variables or build arguments.
 

-

### Lab: Compose for Run-Time Image Building and Multi-Container Development
● This exercise covers building a custom Drupal image for local testing
● Compose, in this scenario is useful for testing apps, such as:
○ A user learning the Drupal admin UI
○ a software tester evaluating the performance of an app
● Start with Compose file from previous lab
● Make your Dockerfile and docker-compose.yml in the directory
compose-lab-2
● Use the README.md in the directory for details
  
