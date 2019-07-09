#The Cloud Native Application

-
-
#The Cloud Native Application
increased focus on flexibility and automation

-
-

#The Promise of a Platform?

Platform is an `overused` word today

Platforms are best summarized by the nature in which they impose constraints on how developers build applications.

They automate tasks, making development teams more agile.

-

#The Promise of a Platform?

When we build platforms, we are creating a tool that automates a set of repeatable practices. `Practices` are formulated from a set of `constraints` that translate valuable `ideas` into a plan.

-
-
# Ideas
What are our core ideas of the platform and why are they valuable?

example:

We can create a set of constraints by using ideas as the foundation for a platform

-
-
# Constraints
What are the constraints necessary to transform our ideas into practices?

examples:

* Software components are to be built as independently deployable services.
* All business logic in a service is encapsulated with the data it operates on.
* There is no direct access to a database from outside of a service.
* Services are to publish a web interface that allows access to its business logic from other services.

-
-

# Practices
How do we automate constraints into a set of repeatable practices?

Practices that are derived from constraints should be stated as a collection of promises.
This allows for expectation to be maintained


-

# Practices
examples:
* A self-service interface is provided to teams that allows for the provisioning of infrastructure required to operate applications.
* Applications are packaged as a bundled artifact and deployed to an environment using the self-service interface.
* An application is provided with credentials to a database as environment variables, but only after declaring an explicit relationship to the database as a service binding.

-

# Practices
Each of the practices listed above takes on the form of a promise to the user. The intent of the ideas are realized as constraints imposed on the application. 

**Cloud native applications are built on a set of constraints that reduce the time spent doing undifferentiated heavy lifting.**

-
-
# Patterns
software architectures are beginning to move away from large monolithic applications.

`ultra-scalability without sacrificing performance and availability`

-

# Patterns
By breaking apart components of a monolith, teams are provided with more control over how features make their way to production.

By increasing isolation between components, software teams can focus on building smaller, more singularly focused services with independent release cycles.

-

# distributed issues
As applications become more distributed the chance of failure in the way application components communicate becomes an important concern.

operational failures become an inevitable result.

However Cloud native application architectures provide the benefit of ultra-scalability while still maintaining guarantees about overall availability and performance of applications.

-
-

# Scalability
To develop software faster, we are required to think about scale at all levels.

In most cases `Scale` is a function of cost that produces value.

The level of unpredictability that reduces that value is called `risk`.

risk increase when demand that developers deliver features to production at a faster pace.

-

# Scalability
Communication between operations and developers can affect our ability to scale.

Putting emphasis on the experience of an operations team inside the software development process, one that fosters shared learning and improvement. Leads to the minimization of risks.

-
-

# Reliability
expectations that are created between teams (development, operations, user experience, etc.) are contracts.

A service agreement between teams is made explicit in order to guarantee that behaviors are consistent with the expected cost of operations. 

Service agreements between teams are created in order to reduce the risk of unexpected behavior in the overall functions of scale that produce value for a business.

-

# Reliability
The goal for a software business is to reliably predict the creation of value through cost

-
-

# Agility
Developing distributed systems is a complex undertaking

The move toward a more distributed application architecture for companies is fueled by the need to deliver software faster and with less risk of failure.

** need more **

-
-

# Splitting the Monolith
Monoliths tend not to be very reliable.

When components share resources on the same virtual machine, a failure in one component can spread to others!

The risk of making a breaking change in a monolith increases with the amount of effort by teams needing to coordinate their changes.

-

# Splitting the Monolith
By splitting up a monolith into smaller, more singularly focused services, deployments can be made with smaller batch sizes on a team’s independent release cycle.

-
-

# The Twelve Factors
The twelve-factor methodology is a popular set of application development principles compiled by the creators of the Heroku cloud platform.

The Twelve-Factor App is a website that was originally created by Adam Wiggins, a co-founder of Heroku, as a manifesto that describes SaaS applications designed to take advantage of the common practices of modern cloud platforms.

the 12 individual factors that distill these core ideas into a collection of opinions for how applications should be built.

-
# The Twelve Factors
* `Codebase` One codebase tracked in revision control, many deploys
* `Dependencies` Explicitly declare and isolate dependencies
* `Config` Store config in the environment
* `Backing Services` Treat backing services as attached resources
* `Build, Release, Run` Strictly separate build and run stages
* `Processes` Execute the app as one or more stateless processes

-
# The Twelve Factors
* `Port Bindings` Export services via port binding
* `Concurrency` Scale out via the process model
* `Disposability` Maximize robustness with fast startup and graceful shutdown
* `Dev/Prod Parity` Keep development, staging, and production as similar as possible
* `Logs` Treat logs as event streams
* `Admin Processes` Run admin/management tasks as one-off processes

-

# Codebase
One codebase tracked in revision control, many deploys
<img src="img/codebase.png" alt="codebase" width="50%"/>

-

# Dependencies
Application dependencies should be explicitly declared, and any and all dependencies should be available from an artifact repository that can be downloaded using a dependency manager, such as Apache Maven.

Twelve-factor applications never rely on the existence of implicit systemwide packages

-

# Config
Application code should be strictly separated from configuration. The configuration of the application should be driven by the environment.

<img src="img/config.png" alt="config" width="50%"/>

-

# Backing Services
A backing service is any service that the twelve-factor application consumes as a part of its normal operation.
Examples:
* databases
* API-driven RESTful web services
Backing services are considered to be resources of the application. Which are attached to the application for the duration of operation

-

# Build, Release, Run
The twelve-factor application strictly separates the build, release, and run stages.

-

# Build stage
The build stage takes the source code for an application and either compiles or bundles it into a package. The package that is created is referred to as a `build`.

-

# Release stage
The release stage takes a build and combines it with its config. The release that is created for the deploy is then ready to be operated in an execution environment. 

Each release should be added to a directory that can be used by the release management tool to rollback to a previous release.

-

# Run stage
The run stage, commonly referred to as the runtime, runs the application in the execution environment for a selected release.

`By separating each of these stages into separate processes, it becomes impossible to change an application’s code at runtime.`

-

# Processes
Twelve-factor applications are created to be stateless in a share-nothing architecture

The only persistence that an application may depend on is through a backing service, like a database.

-

# Port Bindings
Twelve-factor applications are completely self-contained.

they do not require a web server to be injected into the execution environment at runtime in order to create a web-facing service.

Each application will expose access to itself over an HTTP port.

-

# Concurrency
Applications should be able to scale out processes or threads for parallel execution of work in an on-demand basis.

Applications should distribute work concurrently.

apps also should be able to scale out horizontally and handle requests load-balanced.

-

# Disposability
The processes of a twelve-factor application are designed to be disposable.

Applications should start within seconds and begin to process incoming requests.

Short startups reduce the time it takes to scale app instances.

-

# Dev/Prod Parity
The twelve-factor application should prevent divergence between development and production environments.
`Time gap` Developers should expect development changes to be quickly deployed into production
`Personnel gap` Developers who make a code change are closely involved with its deployment
`Tools gap` Each environment should mirror technology and framework choices in order to limit unexpected behavior due to small inconsistencies

-

# Logs
Twelve-factor apps write logs as an ordered event stream to stout.

Applications should not attempt to manage the storage of their own logfiles.

-

# Admin Processes
sometimes developers need to run administrative tasks.

example: database migrations

Admin processes should be run in the execution environment of an application.

