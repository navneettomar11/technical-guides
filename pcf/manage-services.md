# Services and Services Instances
Cloud Foundry offers a marketplace of services, from which users can provision reserved resources on-demand. Examples of resources services provide include databases on a shared or dedicated server, or accounts on a SaaS application. These resource are known as service instance and the system that deliver and operate these resources are known as Services. Think of a servce as a factory that delivers service instances.

## User-Provided Service Instances
Cloud Foundry enables users to leverage services that are not available in the marketplace using a feature called User-Provided Service Instance(UPSI).

# Service Instance Credentials
Cloud Foundry enables to provision credentials needed to interface with a service instance. You can application binding to automatically deliver these credentials to your Cloud Foundry app. For external and local clients, you can use service keys to generate credentials to communicate directly with a service instance.

## Application Binding
Service instance  credentials can be delivered automatically to applications running on Cloud Foundry in an environment variable.

# List Marketplace Services
After targeting and logging into Cloud Foundry, run the `cf marketplace` cf CLI command to view the services to your targeted organization. Available services may differ between organization and between Cloud Foundry marketplaces.
```bash
$ cf marketplace
Getting services from marketplace in org my-org / space test as user@example.com...
OK

service         plans       description
p-mysql         100mb, 1gb  A DBaaS
p-riacks        developer   An S3-compatible object store
```

# Creating Service Instances
You can create a service instance with the following command:
`cf create-service SERVICE PLAN SERVICE_INSTANCE`

Use the information in the list below to replace `SERVICE`, `PLAN`, and `SERVICE_INSTANCE` with appropriate values.
- `SERVICE`: The name of the service you want to create an instance of.
- `PLAN`: The name of a plan that meets your needs. Service providers use plans to offer varying levels of resources or features for the same service.
- `SERVICE_INSTANCE`: The name your provide for your service instance. You use this name to refer to your service instance with other commands. Service instance names can include alpha-numeric characters, hyphens, and underscores, and you can rename the service instance at any time.
```bash
$ cf create-service rabbitmq small-plan my-rabbitmq

Creating service my-rabbitmq in org console / space development as user@example.com....
OK
```
User Provided Service Instances provide a way for developers to bind applications with services that are not avialable in their Cloud Foundry marketplace.

## Arbitrary Parameters
Some services support providing additional configuration parameters with the provision request. Pass these parameters in a valid JSON object containing service-specific configuration parameters, provided either in-line or in a file. For a list of supported configuration parameters.

Example providing service-specific configuration parameters in-line:
```bash
$ cf create-service my-db-service small-plan my-db -c '{"storage_gb":4}'
Creating service my-db in org console / space development as user@example.com...
OK
```

Example providing service-specific configuration parameter in a file:
```bash
$ cf create-service my-db-service small-plan my-db -c /tmp/config.json

Creating service my-db in org console / space development as user@example.com...
OK
```

# List Service Instances
Run the `cf services` command to the list the service instances in your targeted space. The output from running this command includes any bound apps and the state of the last requested operation for the service instance.
```bash
$ cf services
Getting services in org my-org / space test as user@example.com...
OK

name        service         plan        bound app        last operation
mybucket    p-riaskcs       developer   myapp       create succeeded
mydb        p-mydql         100mb                        create succeeded   
```
## Get Details for a Particular Service Instance

Details include dashbaord urls, if applicable, and operation start and last updated timestamps.
```bash
$ cf service mydb

Service instance: mydb
Service: p-mysql
Plan: 100mb
Description: MySQL database on demand
Documentation url:
Dashboard:

Last Operation
Status: create succeeded
Message: 
Started: 2015-05-08T22:59:07Z
Updated: 2015-05-08T22:59:07Z
```

# Bind a Service Instance
Depending on the service, you can bind service instances to application and/or routes.

Not all services support binding, as some services deliver values to users directly without integration with Cloud Foundry, such as SaaS applications.

## Bind a service Instance to an Application
Depending on the service, binding a service instance to your application may deliver credentials for the service instance to the application. Binding a service instance to an application may also trigger application logs to be streamed to the service instance.

```bash
$ cf bind-service my-app mydb
Binding service mydb to my-app in org my-org / space test as user@example.com
OK
TIP: Use 'cf push' to ensure your env variable changes take effect.

$ cf restart my-app
```
> Note: You must restart or in some cases re-push your application for changes to be applied to the `VCAP_SERVICES` environment variable and for the application to recognize these changes.

## Binding with Application Manifest
As an alternative to binding a service instance to an application after pushing an application you can use the application manaifest to bind the service instance during push. As of cf CLI v6.12.1, Arbitrary Parameters are not supported in application manifest. Using the manifest to bind service instances to routes is also not supported.

The following excerpt from an application manifest binds a service instance called `test