# Twelve Factor App
In modern era, software is commonly delivered as a service: called web apps or software as a service. The twelve factor app is a methodology for building software-as-a-service-apps thats:
* User declarative formats for setup automation, to minimize time and cost for new developers joining the project
* Have a clean contract with the underlying operating system, offering maximum protability between execution environments
* Are suitable for development on modern cloud platforms, obviating the need for servers and system administration
* Minimize divergence between development and production enabling continous development for maximum agility
* And can scale up without significant changes to tooling architecture, or development practices.

The twelve-factor methodology can be applied to apps written in any programming language, and which use any combination of backing services(database, queue, memory cache, etc).

## Background
The contributors to this document have been directly involved in the development and deployment of hundreds of apps, and indirectly witnessed the development, operation and scaling of hundreds of thousands of apps via our work on the Heroku platform.

This document synthesizes all of out experience and observations on a wide variety of software-as-a-service apps in the wild. It is a triangulation on ideal pratices for app development, paying particular attention to the dynamics of the organic growth of an app over time, the dynamic of collaboration between developers working on the app's codebase and avoiding the cost of software erosion.

Our motivation is to raise awareness of some systemic problems we've seen in modern application development, to provide a shared vocabulary for discussing those problems, and to offer a set of board conceptual solutions to those problems with accompanying terminology. The format is inspired by Martin Fowler's book Patterns of Enterprise Application Architecture and Refactoring.


## I. Codebase
> One codebase tracked in revision control, many deploys. 

A twelve-factor app is always tracked in a version control system, such as Git, Mercurial, or Subversion. A copy of the revision tracking database is known as a _code repository_, often shortend to _code repo_ or just _repo_

A codebase is any single repo(in a centralized revision control system like Subversion), or any set of repos who share a root commit (in a decentralized revision control system like Git).

![codebase_deploys](https://12factor.net/images/codebase-deploys.png)

There is always a one-to-one correlation between the codebase and the app:
* If there are multiples codebases, it's not an app - it's a distributed system. Each component in a distributed system in an app, and each individually comply with twelve-factor.
* Multiple apps sharing the same code is a violation of twelve-factor. The solution here is to factor shated code into libraries which can be included through dependency manager.

There is only one codebase per app, but there will be many deploys of the apps. A deploy is a running instance of the app. This is typically a production site and one or more staging sites. Additionally, every environment, each of which also qualifies as a deploy.

The codebase is the same across all deploys, although different versions may be active in each deploy. For example, a develioper has some commits not yet deploye to staginig; staging has some commits not yet to deployed to production. But they all share the same codebase, thus making them identifiable as different deploys of the same app.

## II. Dependencies
>Explicitly declare and isolate dependencies

Most programming languages offer a packaging system for distributing support libraries, such as CPAN for perl or Rubygems for Ruby, Libraries installed through a packaging system can be installed system wide(knowns as "site packages") or scoped into the directory containting the app(known as "vendoring" or "bundling").

A twelve-factor app never relies on implicit existence of system wide packages. It declare all dependencies, completely and exactly, via a dependency delcartation manifest. Furthermore, it uses a dependency isolation tool during execution to ensure that no implicit dependencies "leak in" from the surrounding system. The full and explicit dependency specification is applied uniformly to both production and development.

For example, Bundler for Ruby offers the Gemfile manifest format for dependency declaration and bundle exec for dependency isolation. In python there are seperate tools for these steps - pip is used for declaration and Virtualenv for isolation. Even C has Autoconf for dependency declaration and static linking can provide dependency isolation. No matter what the toolchain, dependency declaration and isolation must always be used together - only one or the other is not sufficient to satisfy twelve-factor.

One benefit of explicit dependency declaration is that its simplifies setup for developers new to the app. The new developer can check out the app's codebase onto their development machine, requiring only the language runtime and dependency manager installed as prerequisities. They will be able to set up everything neeeded to run the app's code with a deterministic build command. For example, the build command for Ruby/Bundler is bundle install, while for Clojure/Leiningen it is lein deps.

Twelve-factor apps also do not relu on the implicit existence of any system tools. Examples include shelling out to ImageMagick or curl. While these tools may exist on many or even most systems, there is no guarantee that they will exists on all systems wherer the app may run in the future, or whether the version found on a future system wil be compatible with the app. If the app needs to shell out to a system tool, that tool should be vendored into the app.

## III. Config
> Store config in the environment

An app's config is everything that is likely to vary between deploys(staging, production, developer environments, etc.). This includes:
* Resource handles to the database, Memcached, and other backing services.
* Credentials to external services such as Amazon S3 or Twitter
* Per-deploy values suchs as the canonical hostname for the deploy.

Apps sometimes store config as constants in the code. This is a violation of twelve-factor, which required strict separation of config from code. Config varies substantially across deploys, code does not.

A litmus test for whether an app has all config correctly factored out of the code is whether the codebase could be made open source at any moment, without compromising any credentials.

Note that this definition of "config" does **not** include internal application config, such as `config/routers.rb` in Rails, or how code modules are connected in Spring. This type of config does not vary between deploys, and so is best done in the code.

Another approach to config is the use of config files which are checked into revision control, such as `config/database.yml` in Rails. This is huge improvement over using constants which are checked into the code repo, but still has weaknesses: it's easy to mistakenly check in a config file to the repo; there is a tendency for config files to be scattered about in different places and different formats, making it hard to see and manage all the config in one place. Further, these format tend to be language-or framework-specific.

**The twelve-factor app stores config in environment variables**(often shortent to _env vars_ or _env_). Env vars are easy to change between deploys without changing any code; unlike config files, there is little chance of them being checked into the code repo accidentally; and unlike custom config files, or other config mechanisms such as Java System properties, they are a language-and-OS-agnostic standard.

Another aspect of config management is grouping. Sometimes apps batch config into named groups(often called "environments") named after specific defploys, such as the `development`, `test`, and `production` environments in Rails. This method does not scale cleanly: as mose deploys of the app are created, new environment names are necessary, such as `staging` or `qa`. As the project grows further, developers may add their own special environment like `joe-staging`, resulting in a combinatorial explosion of config which makes managing deploys of the app very brittle.

In a twelve-factor app, env vars are granular controls, each fully orthogonal to other env vars. They are never grouped together as "environments", but instead are independently managed for each deploy. This is a model that scales up smoothly as the app naturally expands into more deploys over its lifetime.

## IV. Backing services
> Treat backing services as attached resources

A backing service is any service the app consumes over the network as part of its normal operation. Examples include datastores(such as MySQL or CloudDB), messaging/queuing systems(such as RabbitMQ or Beanstalkd), SMTP services for outbound email(such as Postfix), and caching systems(such as Memcached).

Backing services like the database are traditionally managed by the same systems administrators who deploy the app's runtime. In addition to these locally-managed services, the app may also have services provided and managed by third parties. Example include SMTP services (such as Postmark), metrics-gathering services (such as New Relic or Loggly), binary asset services(such as Amazon S3), and even API-accessible consumer services (such as Twitter, Google Map or Last.fm).

**The code for a twelve-factor app makes no distinction between local and third party services**. To the app both are attached resources, accessed via a URL or other locator/credentials stored in the config. A deploy of the twelve-factor app should be able to swap out a local MySQL database with one managed by a third party(such as Amazon RDS) without any changes to the app's code. Likewise, a local SMTP server could be swapped with a third-party SMTP service(such as Postmark) without code changes. In both cases, only the resource handle in the config needs to change.

Each distinct backing service is a resource. For example, a MySQL database is a resource; two MySQL databases(used for sharing at the application layer) qualify as two distinct resources. The twelve-factor app treats these databases as attached resources, which indicates their loosing coupling to the deploy they are attached to.
![example2](assets/backing-serivces.png)

Resources can be attached to and detached from deploys at will. For example, if the app's database is misbehaving due to a hardware issue, the app's administrator might spin up a new database server restored from recent backup. The current production database could be detached, and the new database attached - all without any code changes.

## V. Build, release, run
> Strictly separate build and run stages

A codebase is transformed into a (non-development) deploy through three stages:

* The _build stage_ is a transform which converts a code repo into an executable bundle known as a _build_. Using a version of the code at a commit specified by the deployment process, the build stage fetched vendors dependencies and compiles binaries and assets.
* The _release stage_ takes the build produced by the build stage and combines it with the deploy's current config. The resulting _release_ contains both the build and the config and is ready for immediated execution in the the execution environment.
* The _run stage_ (also known as "runtime") runs the app in the execution environment, by launching some set of the app's processes against a selected releases.

![exmaple3](assets/build_release_run.png)

**The twelve-factor app uses strict separation between the build, release, and run stages**. For example, it is impossible to make changes to the code at runtime, since there is no way to propagate those changes back to the build stage.

Depployment tools typically offer release management tools, most notably the ability to roll back to a previous release. For example, the Capistrano deployment tool stores releases in a subdirectory names `releases`, where the current release is a symlink to the current release directory. Its `rollback` commands make it easy to quickly roll back to a previous release.

Every release should have a unique release ID, such as a timestamp of the release(such as `2019-04-06-20:32:17`) or an incrementing number (such as `v100`). Releases are an append-only ledger and a release cannot be mutated once it created. Any change must create a new release.

Builds are initiated by the app's developers whenever new code is deployed. Runtime execution, by constrast, can happen automatically in case such as a server reboot, or a crashed process being restarted by the process manager. Therefore, the run stage should be kept to as few  moving parts as possible, since problems that prevent an app from running can cause it to break in the middle of the night when no developers are on hand. The build stage can be more complex, since errors are always in the foreground for a developer who is driving the deploy.

## VI. Processes
> Execute the app as one or more statelss processes

The app is executed in the execution environment as one or many processes.

In the simplest case, the code is a stand-alone script, the execution environment is a developer's local laptop with an installed language runtime, and process is lanunched via the command line ( for example, `python my_script.py`). On othe end of the spectrum, a production deploy of a sophisticated app may use many process types, instantiated into zero or many running processes.

**Twelve-factor processes are statelss ans sharing nothing**. Any data that needs to persist must be stored in a stateful backing service, typically a database.

The memory space or filesystem of the process can be used as a brief, single transaction cache. For example, downloading a large file, operating on it, and storing the result of the operation in the database. The twelve-factor app never assumes that any cached in memory or on disk will be available on a future request or job - with many processes of each typr running, chances are high that a future request will be served by a different process. Even when running only one process, a restart(triggered by code deploy, config change, or the execution environemt relocating the process to a different phyiscal location) will usually wipe out all local(e.g. memory and filesystem) state.

Asset packagers like django-assetpackager use the filesystem as a cache for compiled asset. A twelve-factor app prefers to do this compiliing during the build stage. Asset packagers such as Jammit and the Rails assets pipeline can be configured to package assets during the build stage.

Some web systems rely on "sticky session" - that is caching user session data in memory of the app's process and expecting future requests from the same visitor to be routed to the same process. Sticky session are a violation of twelve-factor and should never be used or relied upom. session state data is a good candidate for a datastore that offer time expiration such as Memcached or Redis.

## VII. Port binding
> Export services via port binding

Web apps are sometimes executed inside a webserver container. For example, PHP apps might run as a module inside Apache HTTPD, or Java apps might run inside Tomcat.

**The twelve-factor app is completely self-contained** and does not rely on runtime injection of a webserver into the execution environment to create a web-facing service. The web app **exports HTTP as a service by binding to a port**, and listening to requests coming in on that port.

In a local development environment, the developer visit a service URL like `http://localhost:5000/` to access the service exported by their app. In development, a routing layer handles routing requests from a public-facing hostname to the port-bound web processes.

This is typically implemented by using dependency declaration to add a webserver library to the app, such as Tornado for Python, Thin for Ruby, or Jetty for Java and other JVM-based languages. This happen entirely in user space, that is, within the app's code. The contract with the execution environment is binding to a port to serve requests.

HTTP is not only service that can be exported by port binding. Nearly any kind of server software can be run via a process binding to a port and awaiting incoming requests. Example includes ejabberd(speaking XMPP), and Redis (speaking the Redis protocol).

Note also that the port binding approach means that one app can become the backing service for another app, by providing the URL to the backing app as a resource handle in the config for the consuming app.

## VIII. Concurrency
> Scale out via the process model

Any computer program once run, is represented by one or more processes. Web apps have taken a variety of process-execution forms. For example, PHP processes run as child processes of Apache, started on demand as needed by request volumne. Java processes take the opposite approach, with the JVM providing one massive uberprocess that reversex a large block of system resources (CPU and memory) on startup, with concurrency managed internally via threads. In both cases, the running process(es) are minimally visible to the developers of the app.
![concurrency](assets/concurrency.png)

**In the twelve-factor app, processes are a first class citizen**. Processes in the twelve-factor app take strong cues from the unix process model for running service daemons. Using this model, the developer can architect their app to handle diverse workloads by assigning each type of work to a _process type_. For example, HTTP requests may be handled by a web process, and long-running background task handled bt a worker process.

This does not exclude individual processes from handling their own internal multiplexing via threads inside the runtime VM, or the async/evented model found in tools such as EventMaching, Twisted or Node.js. But an individual VM can only grow so large (vertical scale), so the application must also be able to span multipl processes running on multiple physical machines.

The process model truly shines when it come time to scale out. The share-nothing, horizontally partitionalbe nature of twelve-factor app processes means that adding more concurrency is a simple and reliable operation. The array of process types and number of processes of each type is knowns as the _process formation_.

Twelve-factor app processes should never daemanize or write PID files. Instead, rely on the operating system's process manager (such as systemd, a distributed process manager on a cloud platform, or a tool like Foreman in development) to manage output streams, respond to crashed processes, and handle user-initated restarts and shutdowns.

## IX. Disposability
>Maximize robustness with fast startup and graceful shutdown

**The twelve-factor app's process are disposable, meaning they can be started or stopped at a moment's notice**. This facilitates fast elastic scaling, rapid deployment of code or config changes and robustness of production deploys.

Process should strive to **minimize startup time**. Ideally, a process takes a few seconds from the time the launch command is executed until the process is up and ready to receive requests or jobs. Short startup time provides more agility for the release process and scaling up; and it aids robustness, because the process manager can more easily movev processes to new physical machines when warranted.

Processes **shut down gracefully when they receive a SIGTERM** signal from the process manager. For a web process, gracefut shutdown in achieved by ceasing to listen on the service port(thereby refusing any new requests), allowing any current requests to finish, and then exiting. Implicit in this model is that HTTP requests are short (no more than a few seconds), or in the case of long polling, the clinent should seamlessly attempt to reconnect when the connection is lost.

For a worker process, graceful shutdown is achieved by returning the current job to the work queue. For example, on RabbitMQ the worker can send a `NACK`; on Beanstalkd the job is returned to the queue automatically whenever a worker disconnects. Lock-based systems such as Delayed Job need to be sure to release their lock on the job record. Implicit in this model is that all jobs are reentrant, which typically is achieved by wrapping the result in a transaction, or making the operation idempotent.

Processes should also be **robust against sudden death**, in the case of a failure in the underlying hardware. While this is a much less common occurence than a graceful shutdown with `SIGTERM`, it can still happen, A recommended approach is use of a robust queueing backend such as Beanstalkd, that return jobs to the queue when clients, disconnect or time out. Either way, a twelve-factor app is architected to handle unexpected non-graceful terminations. Crash-only design takes this concept to its logical conclusion.











