> Scalability of an application is equally important as its features and user interface. It becomes even more important if your app is going to serve more than a million user in the future.

Suppose you've built a web application and started getting few customers. After some feedback and suggestions, you are ready with a full-fledged product. Now, your marketing team shares your app on product hunt to accquire new customers. Suddenly, thousands of visitors are using your app and at one point they are unable to use your app.

You've test your app and it is working fine. So what happened?

"This is not a bug but a problem of scalability. Your cloud architecture is not designed to scale with increasing load."

I've seen many companies that usually focus more on features and less on scalability. Creating applications that are both resilient and scalable is an essential part of any application architecture. In this blog, you will learn how to build scalable web application architecture that can scale with increasing load.

# What is a Scalable Application ?
Scalability refers to the ability of a system to give a reasonable performance under growing demands(This can be larger data-sets, higher request rates, the combination of size and velocity, etc). It should work well with 1 user to 1 million users and handles spikes in traffic automatically. By adding and removing the resources only when needed scalable apps consume only the resources necessary to meet demand.

When talking about scalability in cloud computing, you will often hear about two main ways of scaling - horizontal or vertical.

# Vertical Scaling (Scaling Up)
Scaling up or vertical scaling refers to resource maximization of a single unit to expand its ability to handle the increase load. In hardware terms, this includes adding processing power and memory to the physical machine running the server. In software terms, scaling up may include optimizing algorithms and application code.

![Vertical Scaling](https://www.simform.com/wp-content/uploads/2017/09/Single-Host-Vertical-Scalibility.png)

# Horizontal Scaling
Scaling out or horizontal scaling refers to resource increment by the addition of units to the app's cloud archiecture. This means adding more units of smaller capacity instead of adding a single unit of larger capacity. The request for resources are then spread across multiple units thus reducing the excess load on a single machine.
![Horizontal Scaling](https://www.simform.com/wp-content/uploads/2017/09/Single-Host-Horizontal-Scalibility.png)

# Characteristics of the Scalable Application
There are some areas where an app needs to excel to be considered scalable.
## Performance
First and foremost, the application must operate well under stress with low latency. The speed of a website affects usage and user satisfaction, as well as search engines rankings, a factor that directly correlates to revenue and retention. As a result, creating a scalable web application architecture that is optimized for fast response and low latency is key.

## Availability and Reliablity
These are closely related and equally necessary. Scalable apps rarely if ever go down under stress. They need to reliably produce data upon request and not lose stored information.

## Manageability
The manageability of the cloud architecture equates to the scalability of operations: maintainance and updates. Thing to consider for manageability are the ease of diagnosing and understanding problems when they occur, ease of making updates or modification and how simple the system is to operate (i.e does it routinely operate without failure or exceptions).

## Cost
Highly scalable applications don't have to be unreasonably expensive to build, maintain, or scale. Planning for scalability during development allows the app to expand as demand increases without causing undue expenses.

---

You've plenty of options to choose the cloud provider while building the high-performance web application architecture. The three leading cloud computing vendors, AWS, Microsoft Azure and Google Cloud, each have their own strength and weaknesses that make them ideal for different use cases.

I've chosen AWS to show you how to build web scalable application. AWS is a subsidiary of the reowned company. Amazon, it provides different services that are cloud-centered for various requirements. AWS holds the highest 33% market share of cloud computing. They provide excellent documentation of each of their services, helpful guides and white papers, reference architectures for common apps.

# Step to build a scalable application based on increasing users from 1 to 1 million

## Step 1 - Initial Setup of Cloud Archiecture (1 User)
You are the only one using the app on the localhost. The start can be as simple as deploying an application in a box. Here you need to use the following AWS services to get started.
- **Amazon Machine Images(AMI)** gives the information required to launch an instance, which is a virtual server in the cloud. You can specify an AMI during the launch an instance. An AMI includes a template for the root volume for the instance, launch permissions that control which AWS accounts can use the AMI to launch instances and a block device mapping that specifies the volume to attache to the instance when it's launched.
- **Amazon Elastic Compute Cloud(Amazon EC2)** provides the scalable computing capacity in the AWS cloud. This eliminates the hardware upfront so that you can develop and deplpoy application faster.
- **Amazon Virtual Private Cloud(Amazon VPC)** gives a provision to launch AWS resources in a virtual network. It gives complete control over the virtual networking environment a selection of IP address range, subnet creation, the configuration of route tables and network gateways.
- **Amazon Route 53** is a highly available and scalable cloud DNS web service. Amazon Route 53 effectively connect users request to infrastructure running in AWS - such as Amazon EC2 instances. Elastic Load Balancing load balancers or Amazon S3 buckets.

Here you need a bigger box. You can simply choose the larger instance type which is called vertical scaling. At the initial stage, vertical scaling is enough but we can't scale vertically indefinitely. Eventually, you'll hit the wall. Also, it doesn't address failover and redundancy.

## Step 2 - Creating multiple hosts and choose the database (User > 10)
First, you need to choose the database as users are increasing and generating data. It's advisable to start with SQL database first because of the following reasons:
- Established and well-worn technology.
- Community support and latest tools
- We aren't going to break SQL DBs in our first 10 millions users.

Note, you can choose the NoSQL database if your users are going to generate a large volume of data in various forms.

At this stage, you have everthing in a single bucket. This archiecture is harder to scale and complex to manage in the long run. It's time to introduce the multi-tier architecture to separate the database from the application.

## Step 3 - Store database on Amazon RDS to ease the operations (User > 100)
When users increase to 100, Database deployment is the first thing which needs to be done. There are two general directions to deploy a database on AWS. The foremost option is to use a managed database service such as Amazon Relational Database Service (Amazon RDS) or Amazon Dynamo DB and the second step is to host your own database softwate on Amazon EC2.
- **Amazon Relational Database Service (RDS)** make it easy to setup, operate and scale a relational database in the cloud. Amazon RDS provides six familiar database engines to choose from, inlcluding Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL and MariaDB.

## Step 4 - Create multiple availability zones to improve availability (User > 1000)
As per current architecture, you may face avaialability issues. If the host for your web app fails then it may go down. So you need another web instance in another Availability Zone where you will put the slaves databases to RDS.
- **Elastic Load Balancer (ELB)** distributes the incoming application traffic across EC2 instances. It is horizontally scaled, imposes no bandwidth limit, supports SSL termination, and performs health checks so that only healthy instances receive traffic.
    This configuration has 2 instances behind the ELB. We can have 1000s of instance behind ELB. This is horizontal scaling.
    At this stage, you've multiple EC2 instances to serve thousands of users which ultimately increases your cloud cost. To reduce the cost, you have to optimize instances usage based on varying load.

## Step 5 - Move static content to object-based storage for better performance (Users: 10,000 - 100, 00)
To improve performance and efficency, you'll need to add more read replicas to RDS. This will take load off the write master database. Furthermore, you can reduce the load from web servers by moving static content to Amazon S3 and Amazon CloudFront.
- **Amazon S3** is object-based storage. It is not attached to EC2 instance which make is best suitable to store static content, like javascript, CSS, images, videos. It is designed for 99.999999% of durability and can store multiple pertabytes of data.
- **Amazon CloudFront** is a Content Delivery Network(CDN). It retrieves dataa from Amazon S3 bucket and distributes it to multiple data center locations. It caches content at the edge locations to provide our users with the lowest latency access.

Furthermore, to reduce the load from database servers, you can use DynamoDB(managed NoSQL database) to store session state. For caching data from the database, you can use Amazon ElastiCache.
- **Amazon DynamoDB** is a fast and flexible NoSQL database service for applications that need consistent, single-digit millisecond latency. It is a completely managed cloud database and supports document and key-value store models.
- **Amazon ElastiCache** is a Caching-as-a-service. It removes the complexity associated with deploying and managing a distributed cache environment. It's a self-healing infrastructure if nodes fail new nodes are started automatically.

## Step 6 - Setting up Auto Scaling to meet the varying demand automatically
At this stage, your architecture is quite complex to be managed by the small team and without proper monitoring analyzing it's diffcult to move forward.

Now that the web tier is much more lightweight, it's time for Auto Scaling!

> Auto Scaling is nothing but an automatic resizing of compute clusters based on demand.

Auto Scaling enables "just-in-time provising", allowing users to scale infrastructure dynamically as load demands. It can launch or terminate EC2 instances automatically based on Spikes in Traffic. You pay only the for the resources which are enough to handle the load.
![AutoScaling](https://www.simform.com/wp-content/uploads/2017/09/Amazon-1.png)

For monitoring you can use the following AWS services:

- **Amazon CloudWatch** provides a rich set of tools to monitor the health and resource utilization of various AWS services. The metric collected by CloudWatch can used to setup alarms, send notification and trigger actions upon alarm firing Amazon EC2 send metrics to. CloudWatch that describe your Auto Scaling instances.

The autoscaling group can include multiple AZs, up to as many as are in the same region. Instances can pop up in multiple AZs not just for scalability, but for availability.

We need to add monitoring, metrics and logging to optimize Auto Scaling efficiently.

- **Host-level metrics**. Look at a single CPU instance within  an autoscaling group and figure out what's going wrong.
- **Aggregate level metrics**. Look at metrics on the Elastic Load Balancer to understand the performance of the entire set of instances.
- **Log analysis**. Look at what the application is telling you using **CloudWatch** Logs. **CloudTrail** helps you analyze and manage logs. If you have set up region-specific configurations in CloudWatch, it is not easy to combine metrics from different regions within as AWS monitoring tool. In that case you can use [Loggly](https://www.loggly.com/), a log management tool. You can send logs metrics from CloudWatch and CloudTrail to Loggly and unify these logs with other data for a better data for a better understanding of your infrastructure and applications.

Sequeeze as much performance as you can from your configuration. Auto Scaling can help with that. You don't want system that are at 20% CPU utilization.

The infrastructure is getting big, it can scale to 1000s of instance. We have read replicas, we have horizontal scaling, but we need some automation to help manage it all, we don't want to manage each individual instance. Here some automation tools.





Source: [https://www.simform.com/building-scalable-application-aws-platform/](https://www.simform.com/building-scalable-application-aws-platform/)
