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






