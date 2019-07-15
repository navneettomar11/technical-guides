# Getting Started with PWS
You can deploy, update and scale your application on Pivotal Web Services(PWS) which is powered by the CloudFoundry as a Service(PaaS).

## Sign up for PWS
Sign up for Pivotal Web Services at [http://run.pivotal.io](http://run.pivotal.io)

Each customer is eligible for one trial organization. A trial org has 2GB of memory and your given $87 of Trial credits good for one year. Your trail will end when you either use up all your Trial credits or a year has passed, whichever come first. Trail credits can be used towards app and task usage. Paid service plans are not available to trial orgs. You cand add your credit card to your trail org an anytime which will give you access to paid servicce plans and 25GB of memory. Your remaining trail credits will get rolled over.

Once you create your org, you will land on the Apps Manager page where you can manage your org. By default, PWS configures each new org with a space named `development`.
[!Screenshot of development space](assets/development-space.png)

# PWS Usage and Billing

## Usage Types
Pivotal Web Services has three major types of usage:
- App Usage
- Task Usage
- Service Subscriptions

### App Usage
App usage cost is based on how much application memory the container takes to deploy your app and how long an app is running. Note that this is application compute memory, not disk usage. All users are entitled to up to 2GB of ephemeral disk per application instance.

App usage is measured in GB-Hours (GB-Hr). A GB-Hr is calculated by multiplying the number of instances by memory size(in GB) by total duration(in hours). Pivotal Web Services charges $0.03/GB-Hr. The more instances you use, the more its costs proportionally.

The minimum charge for an app run is $0.01. Your are chaged a new app run every time you `cf push` or `cf scale`  an app (including the same app).

PWS does not charge for apps that deployed but stopped.

To better understand this, let's look at some examples:
[!PWS app usage price examples](assets/pws-appusage.png)

### Task Usage
Task usage cost is slightly different from app usage. Since task are short-lived, we sum up the total usage (in GB-Hr) of all tasks for an app and charge $0.03/GB-Hr.

For example: Suppose you run the follwing tasks as part of the same parent application.

|Task ID|App Memory(GB)|Duration(Hr)|Task Usage (GB-Hr)|
|-------|--------------|------------|------------|
|1|0.5|0.16|0.08|
|2|0.5|0.5|0.25|
|3|0.5|0.4|0.2|

Total Task Usage = 0.08 + 0.25 + 0.2 = 0.53 (GB-Hr)

Total Task Cost = 0.53 *0.03/GB-Hr = 0.02

### Service Subscriptions
Service subscription are charged on monthly basis and the smallest unit is one month of service. Each service plan has its own pricing per month. To view the price of the monthly subscription, find the service in the [MarketPlace](https://console.run.pivotal.io/marketplace) and you will see the price of each plan.

Upon creation of a service, you are charged for one month's service and billed accordingly. If you delete the service before the month is over, you will still pay the full price; usuage is not prorated. If a servicce is still active 30 days later, you will be charged for another month.
