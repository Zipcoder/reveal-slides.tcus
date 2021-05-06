
# Intro to Docker
## Unit 5: Docker Swarm
  

-

### Unit 5 - Docker Swarm: Overview
How to we easily deploy and maintain our numerous containers across many servers, instances etc.
● How do we automate container lifecycle?
● How do we easily scale out/in/up/down?
● How can we ensure our containers are re-created if they fail?
● How can we replace containers without downtime (blue/green deploy)?
● How can we create cross-node virtual networks?
● How can we ensure only trustes servers run our containers?
● How can we store secrets, keys, passwords and get them to the right
 container (and only that container)?


-

### Swarm Mode: building Orchestration
● Swarm Mode is a clustering solution built inside Docker
● Not enabled by default, new commands once enabled (prevents interference
in case other orchestration is being used, etc)
○ docker swarm
○ docker node
○ docker service
○ docker stack
○ docker secret
 
 Swarm Mode: building Orchestration
 
  Swarm Mode: building Orchestration
 
 Swarm Mode: building Orchestration
 
 Swarm Mode: building Orchestration
 

-

### Unit 5 - Docker Swarm: Agenda
● Managing Containers
● Scaling services
● Creating a N-Node cluster in swarm
● Creating and managing Environment Variables ○ Secrets
 

-

# Container Orchestration
 

-

### Overlay Networking
● Just choose --driver overlay when creating network
● Fore container-to-container traffic inside a single Swarm instance
● Optional IPSec (AES) Encryption on network creation
● Each service can be connected to multiple networks
○ (eg front-end, back-end, etc)
 

-

### Routing Mesh
Routs ingress (incoming) packets for a Service to proper Task Spans all nodes in Swarm
Uses
 
   

-

### Swarm Stacks
 

-

### Secrets and Swarm
 

-

### Secrets in Swarm Services
 
   

-

### Secrets for Swarm Stacks
 

-

### Secrets and Docker Compose
 

-

### App Lifecycle
 

-

### Service Updates
 

-

### Dockerfile Healthchecks
 

