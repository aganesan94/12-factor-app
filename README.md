# 12-factor-app

Reference : https://12factor.net/

## Why This is important?

- Traditionally apps were built and deployed on a server, in other words married to an infrastructure
- Single Point of Failure
- With SAAS providers coming on board the goal is to ensure that apps are built for SAAS.
- Uptime of 99.999% is the need of the hour
- Portable Apps
- CI/CD
- Scalability
- Deployment on Modern Cloud Platforms

## The Heroku Touch

- Heroku put forth an idea to build modern applications the 12 factor way (12factor.net)

### The 12 Factors

#### 1- Codebase
*One codebase tracked in revision control, many deploys*
##### Problem to Solve
- Single code base, developers working together at the same time, creating conflicts and merge issues.
- Single code base, all related services in the same code base
##### Solution &  Practice
- Use git
- Mulitiple app services, sharing same code is a violation
- Each code base has its own build and deployment lifecycle and can be done mutually exclusively.
---

#### 2- Dependencies
*Explicitly declare and isolate dependencies*
##### Problem to Solve
- Apps have dependencies
- As apps grow, dependencies grow
- Different dependencies used by different developers
- Different apps on a server may require different dependencies
##### Solution &  Practice
- *Explicitly declare* and *isolate* dependencies
- Apps should never rely on implicit existence of system-wide packages
- Isolation of dependencies is a must per app during local development. For example use of Python venv
- Isolation of dependencies during build and deployment time is a must.
- Use of docker or containerization to isolate apps
---

#### 3- Config
*Store config in the environment*
##### Problem to Solve
- Hard coding of information inside the code (Example: Database URI, Redis Cluster to point to) etc
- Causes inconsistencies when deploy to different build and deployment environments.
##### Solution &  Practices
- Store configuration parameters as environment variables or config files
- Config files can be read when built or deployed
---

#### 4- Backing Services
*Treat backing services as attached resources*
##### Problem to Solve
- A backing service is one which sends notifications, store stateful information or can be asynchronous
- Trying to bake the functionality of a backing service via an app, does not allow the app to scale.
##### Solution &  Practices
- Ensure apps are built in such a way that backing services are configurable. For example, if one redis cluster dies down
  the app should be configured to use the next redis cluster.
- Apps should not have any configuration pre-built to determine which backing service to point to.
---

#### 5- Build, Release, Run
*Strictly separate build and run stages*
##### Problem to Solve
- Inconsistency in build processes
##### Solution &  Practices
- Seperate build, release and run phase.
- Build: Determine how to build an artifact out of the source (.exe, .sh, .zip, .deb, docker-image).
- Build + Config = Release.
- Run = Run the release.
---

#### 6- Processes
*Execute the app as one or more stateless processes*
##### Problem to Solve
- Apps storing session based information
- Even if load balancers are placed before apps and are session aware, the sticky session
  creates a situation where in if the process crashes the user session is lost.
##### Solution &  Practices
- Apps running as processesto be stateless to be able to handle traffic and load when scaling in and out.
- Apps running as processesshould store state specific information to a cluster (like the redis cluster)
- Apps running as processes should not store any specific user information in local memory
---

#### 7- Port Binding
*Export services via port binding*
##### Problem to Solve
- Binding to a specific port, restricts its usage on a server
##### Solution &  Practices
- Run Self contained apps which expose ports to be bound to
---

#### 8- Concurrency
Scale out via the process model
##### Problem to Solve
- Apps are scaled vertically in the traditional approach. This causes bottlenecks
- Apps are not concurrently deployed when required to scale in and out.
##### Solution &  Practices
- Scale apps horizontally as much as possible and not vertically.
- Scaling in and out more instances of the app when required 
---

#### 9- Disposability
*Maximize robustness with fast startup and graceful shutdown*
##### Problem to Solve
- Scale in and Scale out depending on business/technical need is not a criteria in traditional apps
##### Solution &  Practices
- Reduce start up time
- Scale-in and Scale-out i.e dispose when not required.
- Shut down processes gracefully on signal to avoid data leaks or loss.
---

#### 10- DEV PROD PARITY
*Keep development, staging, and production as similar as possible*
##### Problem to Solve
- Different convention for build, deployment in dev vs staging vs prod
##### Solution &  Practices
- Keep development, staging, and production as similar as possible
---

#### 11- Logs
*Treat logs as event streams*
##### Problem to Solve
- Run a logs to a file, if container is killed there is no trace of logs
##### Solution &  Practices
- Use ELK, Splunk, Fluentd
---

#### 12- Admin
*Run admin/management tasks as one-off processes*
##### Problem to Solve
- Apps and Admin processes together complicate release cycles.
##### Solution &  Practices
- Admin processes (DB migration, scripts migration) should be kept separate.
---