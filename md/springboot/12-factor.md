#The Cloud Native Application
increased focus on flexibility and automation

-
-

## The Promise of a Platform?

Platforms are ways developers impose constraints on how to build an application.

* Practices
	- models for developing solutions
* Ideas
	- the nature in which we impose constraints
* Constraints
	- restrictions we set to ensure best practices are taken

-

## Examples of Constraints

* Software components are to be built as independently deployable services.
* All business logic in a service is encapsulated with the data it operates on.
* There is no direct access to a database from outside of a service.
* Services are to publish a web interface that allows access to its business logic from other services.

-
-

### Distributed Issues of Cloud Applications

* depend heavily on communication
* require state be held accross multiple servers
* creates many points of failure

Cloud Native seeks to overcome these issues using the 12 Factor Principle of Cloud Applications

-
-

## The Twelve Factors
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Codebase</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Dependencies</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Config Store</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Backing Services</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Build, Release, Run</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Processes</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Port Bindings</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Concurrency</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Disposability</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Dev/Prod Parity</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Logs</li></div>
<div style="float:left;width:40%;padding-left:10%"><li style="float:left">Admin Processes</li></div>

-

# Codebase
One codebase tracked in revision control per service, many deploys

-

# Dependencies
Explicitly declared packages of code a service relies on

-

# Config
Environment variables necessary for the service to function

-

# Backing Services
Services consumed for normal use of platform

-

# Build, Release, Run
Separated environments for each stage of developing

* Build: takes source code and dependencies and packages them for release
* Release: takes built source and distributes code to wherever it needs to be
* Run: executes a service to be consumed by the overall platform

-


# Processes
Each service should contain no state and share no information with other servers

-

# Port Bindings
Services are completely self-contained.

Each application will expose access to itself over an HTTP port.

-

# Concurrency
Applications should be able to scale out processes or threads for parallel execution of work in an on-demand basis

-

# Disposability
Servers running a service are disposable

-

# Dev/Prod Parity
Development and production should be as similar as possible

-

# Logs
Services write logs as an ordered event stream to stout.

-

# Admin Processes
Admin processes should be run in the execution environment of an application.

