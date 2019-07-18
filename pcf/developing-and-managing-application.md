# Developing and Managing Application

## App Design for the Cloud
Apps written in supported frameworks often run unmodified on Cloud Foundry if the app design follows a few simple guidelines. Following these guidelines facilitates app deployment to Cloud Foundry and other cloud platforms.

The following guidelines represent best pratices for developing modern apps for cloud platforms. 

### Avoid Writing to the Local File System
Apps running on Cloud Foundry should not write files to the local file system for the following reasons:
- **Local file system storage is short-lived**. When an app instance crasher or stops, the resources assigned to that instance are reclaimed by the platform including any local disk changes made since the app started. When the instance is restarted, the app will start with a new disk image. Although your app can write local files while it is running, the files will disappear after the app restarts.
- **Instance of the same app do not share a local file system**. Each app instances run in its own isolated container. Thus a file written by one instance is not visible to other instance of the same app. If the files are temporary, this should not be a problem. However, if your app needs the data in the files to presist across app restarts, or the data needs to be shared across all running instances of the app, the local file system should not be used. We recommend using a shared data service like a database or blobstore for this purpose.

For example, instead of using the local file system, you can use Cloud Foundry service such as the MongoDB document database or a relational database like MySQL or Postgres. Another option is to use cloud storage providers such as Amazon S3, Google Cloud Storage, Dropbox, or Box. If your app needs to communicate across different instances of itself, consider a cache like Redis or a messaging-based architecture with RabbitMQ.

If you must use a file system for your app because for example, you app interacts with other apps through a network attached file system, or because your app is based on legacy code that you cannot rewrite, consider using Volumne Service to bind a network attached file system to your app.

### Cookies Accesible across Apps
In an environment with shared domains, cookies might be accessible across apps.
Applications deployed to Cloud Foundry share the domain `cfapps.io`. Many tracking tools such as Google Analytics and Mixpanel use the highest available domain to set their cookies. For an application deployed to Cloud Foundry, a cookie set to use the highest domain has a `Domain` attribute of `.cfapps.io` in its HTTP response header. For example, an app at `my-app.cfapps.io` might be able to access the cookies for an app at `your-app.cfapps.io`.

You shoud decide whether or not you want your apps or tools that use cookies to set and store the cookies at the highest available domain.

### Port Considerations
Clients connect to apps running on Cloud Foundry by making requests to URLs associated with the app. Cloud Foundry allows HTTP requests to apps on ports 80 and 443.

Cloud Foundry also supports WebSocket handshake request over HTTP containing the `Upgrade` header. The Cloud Foundry routes handles the upgrade and initates a TCP connection to the app to from a WebSocket connection.

### Cloud Foundry Updates and Your App
For app management purposes, Cloud Foundry may need to stop and restart your app instances. If this occurs, Cloud Foundry performs the following steps:
1. Cloud Foundry sends a single `termination signal` to the root process that your start command invokes.
2. Cloud Foundry waits 10 seconds to allow your app to cleanly shut down any child processes and handle any open connections.
3. After 10 seconds, Cloud Foundry forcibly shuts down your app.
Your app should accept and handle the termination signal to ensure that it shutdown gracefully. To achieve this, the app is expected to follow the steps below when shutting down:
1. App receives termination signal
2. App closes listener so that it stop accepting new connections
3. App finishes serving in-flight requests.
4. App closes existing connections as their requests complete.
5. App shuts down or is killed.

## Ignore Unnecessary Files When Pushing
By default, when you push an app, all files in the app's project directory tree are uploaded to your Cloud Foundry instance, except version control and configuration files or folders with the following names:
- .cfignore
- _darcs
- .DS_Store
- .git
- gitignore
- .hg
- manifest.yml
- .svn

In addition to these, if API request diagnostics are directed to a log file and the file is within the project directory tree, it is excluded from the upload. You can direct these API request diagonstics to a log file using `cf config --trace` or the `CF_TRACE` environment variable.
If the app directory contains other files such as `temp` or `log`, or complete subdirectories that are not required to build and run your app, you might want to add them to a `.cfignore` file to exclude them from upload. Especially with a large app, uploading unnecessary files can slow app deployment.
To use a `.cfignore` file, created a text file name `.cfignore` in the root of your app directory structure. In this file specify the files or file types you wish to exclude from the upload. For example, these lines in a `.cfignore` file exclude the "tmp" and "log" directories.

---
tmp
log
---

The file types you will want to exclude vary based on the app framework you use. For examples of commonly-used `.gitignore` files

## Run Multiple Instance to increase Availability
Singleton apps may become temporarily unavailable for reasons that include: 
- During an upgrade, Pivotal Web Services(PWS) gracefully shuts down the apps running on each Diego cell and then restarts them on another Diego cell. Single app instances may become temporarily unavialable if the replacement instance does not become healthly within the cell's evacuation timeout, which defaults to 10 minutes.
- Unexpected faults in PWS system component or underlying infrastructure, such as container-host VMs or IaaS Availablity Zones, may cause lone app instances to disappear or become unroutable for a minutes or two.

To avoid the risk of an app becoming temporarily, unavialable, developers can run more than one instance of the app.

## Using Buildpacks
A buildpack consist of bundle of detection and configuration script that provide frameworks and runtime support for your apps. When you deploy an app that needs a buildpack, Cloud Foundry installs the buildpack on the Diego cell where the app runs.
