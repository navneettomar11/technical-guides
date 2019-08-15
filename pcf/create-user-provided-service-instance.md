# User-Provided Service Instances

User-provided service instances enable developers to use service that are not available in the marketplace with their apps running on Cloud Foundry.

User-provided service instances can be used to deliver service credentials to an app, and/or to trigger streaming of app logs to a syslog compatible consumer. These two functions can be used alone or at the same time.

Once created, user-provided service instance behave like service instances created through the marketplace.

## Create a User-Provided Service Instance
The alias for cf create-user-provided-service is `cf cups`

### Deliver Service Credentials to an App
Suppose a developer obtains a URL, port, username, ans password for communicating with an Oracle database manage outside of Cloud Foundry. The developer could manually create custom environment variables to configure their app with these credentials.

User-provided service instances enables developers to configure their apps with these using the familiar App Binding operation and the same app runtime environment variables used by Cloud Foundry to automatically deliver credentials for marketplace services(VCAP_SERVICES)

```bash
cf cups SERVICE_INSTANCE -p '{"username":"admin", "password":"pa55woRD"}'
```

To create my-user-provided-route-service `-p` option with a comma-separated list of parameter names. The Cloud Foundry Command Line Interface (cf CLI) prompts you for each parameter value.

```bash
$ cf cups my-user-provided-route-service -p "host, port"
host > rbd.local
port > 5432

Creating user provided service my-user-provided-route-sercie in org my-org / space my-space as user@example.com
OK
```
Once the user-provided service instance is created, to deliver the credentials to one or more apps.




