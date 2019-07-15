# Getting Started Deploying Spring Apps

## Prerequisities
- A Spring app that runs locally on your workstation.
- Intermediate to advanced Spring Knowledge
- The Cloud Foundry Command Line Interface (cf CLI).
- JDK 1.6, 1.7 or 1.8 for Java 6, 7 or 8 configured on your workstation

> Note: The Cloud Foundry Java buildpack uses JDK 1.8 but you can modify the buildpack and the manifest for your app to compile to an earilier version.

## Step 1: Declare App Dependencies
Be sure to declare all dependency tasks for your app in the build script of your chosen build tool.

The [Spring Getting Start Guide](https://spring.io/guides) demonstrate features and functionality you can add to your app, such as consuming RESTFul service or integrating data. These guides contain Gradle and Maven build script example with dependencies. You can copy that code for the dependencies into your build script.

## Step 2: Allocate Sufficient Memory
Use the `cf push -m` command to specify the amount of memory that should allocated to the application. Memory allocated this way is done in preset amounts of 64M, 128M, 256M, 512M, 1G, or 2G. For example:
> $ cf push -m 128M

When your app is running you can use the `cf app APP-NAME` command to see memory utilization.

## Step 3: Provide a JDBC Driver
The Java buildpack does not bundle a JDBC driver with your application. If your application accesses a SQL RDBMS, you must do the following:
- Include the appropriate driver in your application.
- Create a dependency task for the driver in the build script for your build tool or IDE.

## Step 4: Configure Service Connection for a Spring App
PWS provides extensive support for creating and binding a Spring application to services such as MySQL, PostgreSQL, MongoDB, Redis, and RabbitMQ. For more information about creating and binding a service connection for your app, refer to the [Configure Service Connection for Spring](https://docs.run.pivotal.io/buildpacks/java/configuring-service-connections/spring-service-bindings.html) topic.
## Step 5: Configure the Deployment Manifest
You can specify deployment options in a manifest file `manifest.yml` that the cf push command uses when deploying your app.

Refer to the [Deploying with Application Manifests](https://docs.run.pivotal.io/devguide/deploy-apps/manifest.html) topic for more information.

## Step 6: Log in and Target the API Endpoint
Run `cf login -a API-ENDPOINT`, endter your login credentials, and select a space and org. The API endpoints is api.run.pivotal.io.

## Step 7: Deploy your Application
From the root directory of your application, run
`cf push APP-NAME -p PATH-TO-FILE.jar` to deploy your application.
## Step 8: Test your Deployed App
You've deployed an to PWS!

Use the cf CLI or App Manager to review information and administer your app and your account. 

