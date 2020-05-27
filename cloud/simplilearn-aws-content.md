# AWS Cloud

## What is AWS ?
Reliable and secure cloud computing platform.

## Cloud Computing
On demand provisioning of IT resources and application through the Internet.

Traditional Storage data centers have Direct Attached storage, Network attached storage, and Storage Area Networks which differ in their performance, durability and cost.

## Amazon Storage
S3 (Simple Storage Service)
S3 Standard : For general purpose active data
S3 Standard Rare Access: For general purpose and enduring, but not much active data.
CloudFront : is an economical , but dynamic content delivery network or CDN. Web browser & Mobile devices.
Elastic File System: The system is said to be elastic, as it automatically main storage whenever you create or delete file.
Glacier: An economical storage solution to store data that would remain forever, but rarely accessed.


Amazon CloudFront - key advantage
Ability to scale according to requirement
No need to manage high-priced web servers for you web traffic needs
Start small, and grow as the traffic to your website increases
Automatically manages traffic load without any intervention.
Flexible cost model
No minimum monthly commitment or a fixed-term contract
Pay only for content your deliver using Amazon Cloud front

Elastic File System(EFS) - Benefits
Maintain multiple development and test environments
Manage users accessing data and share datasets from a remote location
Seek solutions to manage content repositories
Scale performance of Big Data applications.

Amazon Storage Gateway
AWS Storage Gateway is a scalable and an economical amalgamation of your office IT and AWS storage infrastructure.

This amalgamation offers:
Storage protocols work in harmony with your current applications
Minimizes latency, or the gap between request and response time.

Amazon Import/Export
The AWS Import/Export service allows easy transfer of considerable volumes of data from and to AWS without using the internet, but physical storage devices.

AWS import/export Snowball
Snowball is a cost effective data transfer solution. A single snowball is capable of transferring up to 50 terabytes of data. Only importing S3 data
Create Job in AWS Management Console.
Connect it to the local network and install the Snowball Client to establish the connection
The client runs an internal program to encrypt the data using 256-bit encryption, and the transfers the selected files to the appliance.
Once the file are transferred, ship the Snowball appliance to AWS.
Uses:
Expensive network infrastructure upgrades are beyond your budget or capacity
Huge backlog of data
High-speed internet connection not available.
Amazon Import/Export Disk
Disk is first service for data transfer using UPS/Mail.

Elastic Block storage of AWS
Amazon EC2 instance is an ephemeral storage offering instance data storing

Ephemeral storage:
Storage for temporary use
Content is lost when the system is rebooted.

EBS is used mainly in stateless web hosting, transcoding, caching and High Performance Computing or HPC.

Two types of EBS constructions
Magnetic Storage : A slower, older Elastic Block storage. Provides up to 150 input-output operation per seconds (IOPS)
SSD Construction
General Purpose IOPS :
Possesses the capability to provide three IOPS per GB of provisional storage.
Is used for small websites. It is also used for small and medium databases.
Provisioned IOPS
Enables the user to specify the IOPS as per their requirement
Is used for applications and databases where there is a significant amount of traffic.

Compute Service and Networking

Amazon Web service or AWS provides several computing and networking services to meet your application.
Setting up virtual servers, internet access and firewall
Distributing and routing IP address
Scaling infrastructure to fulfill rising demands

AWS Computing servisse facilitate:
Automatic scaling of an assortment of computing instances
Dynamic distribution of network traffic
AWS networking service facilitate:
Setting up an isolated logical network

Eight Key Compute and Networking services by AWS
Amazon Elastic Compute Cloud: Offers virtual servers or compute instances in the cloud
Auto Scaling: Automatically scales the group of virtual servers, and according to the in-demand changes
Elastic Load Balancing: Disseminates the network traffic across the group of virtual servers
Amazon Virtual Private Cloud: Offers an isolated virtual networks for virtual servers
Amazon Route 53: Directs the network traffic to resources, such as websites, virtual server or a load balancer
Amazon EC2 Container Service: Offers Docker container on a cluster of virtual servers from Amazon EC2
AWS Lambda: Executes the code from Amazon EC2 instances on virtual servers, as a response to a triggered event
AWS Direct Connect: Establishes a dedicated connection from corporate premises, such as datacenter or office , to AWS

Six key concept of compute and networking Services
Instance and Amazon Machine Image
VPC and subnets
Security Groups
Amazon Route 53 hosted zones
Auto Scaling groups
Load Balancer

Instances and Amazon Machine Image
AMI is software template for software configuration details such as Application Server, OS and applications.
An instance refer to a copy of AMI. This copy operates as a virtual server on a host computer in the AWS data centers

VPC and subnets
A virtual private cloud or VPC refers to a virtual network for your AWS account

Security Groups
You can use security groups to guard the AWS resources in all subnets

Amazon Route 53 and Hosted Zones
Amazon Route 53 is scalable, highly available and a cost-effective medium to direct visitors to a website virtual server, or a load balancer.
Hosted zone: A hosted zone is similar to a DNS zone file, and contains its own configuration and metadata information. While created hosted zone, you get four name servers to ensure high availability.

Auto Scaling
Auto scaling assists in retaining the application availability and automatically scaling the Amazon EC2 capacity based on specific condition.
The collection of virtual servers or Amazon EC2 instances are known as Auto Scaling Groups, which shrinks or grow based on the demand.

Load Balancer/Elastic Load Balancing
A Load balancer is responsible for distributing the network traffic across several Amazon EC2 instances.
As you launch or terminate the instances, the Load Balancer automatically directs traffic to the running instances.
Load balancer automatically direct traffic to the running instances
Elastic load balancer redirect traffic from unhealthy instances to healthy instances.


Depending on the load, when the Auto Scaling group terminates or launches the Amazon EC2 instances, the Load balancer automatically makes the necessary adjustments.

Amazon Virtual Private Cloud
Amazon VPC allows launching AWS resources into the virtual network, which is a logically isolated area containing cloud resources.
Amazon VPC service is similar to a traditional network but allows the use of the AWS infrastructure, and enjoy complete control over your virtual network.
Benefits of Amazon VPC
Offers several connectivity options, for example you can connect the Amazon VPC to other VPCs, your datacenter, and internet
Easy to create, leaving you time to focus on creating the applications.
Offers advanced security features which are available both at the subnet and instance levels
Provides you the scalability and reliability provided by AWS.

Benefits of Launching instance into a VPC
Run instances on the hardware used by a single entity.
Split the range of private IP addresses of VPC
Allocate multiple IP address to instances
Allocate static private IP address to instances
Defines network interfaces
Control inbound traffic to instances
Add an extra layer of Access Control to instances
Change the membership of Security Group of instances

VPC With a Single Public Subnet
This allow running a single-tier Web application to be made publicly available, such as a simple website or a blog.

VPC with Public and Private Subnets
This allow running a public Web application and ensuring the private backend server continue to run in another subnet.
The application servers and database can be launched in the private subnets, and the web servers can be launched in the public subnets
The applications servers and database asses the internet to download and install patches, when you set up a Network Address Translation.
The NAT Gateway lets instances in a private subnet to connect to other AWS services, but does not allow the AWS services to connect with instances in the private subnet.

Amazon EC2 instances
Amazon EC2 is a Web service that offers scalable computing capacity server in the AWS data centers.
Eliminates the upfront payment for the hardware.
Facilitates faster development and deployment of software applications
Introduces thousands of servers instances in minutes
Manage storage and configures networking and security parameters
Scale capacity and track requirement changes
Ensures easy and scalable cloud computing.

Benefits of Amazon EC2
Elastic or Scalable Web-scale computing
Full Control over server instances
Flexibility in using services for hosting in the cloud
Quick availability of instances
Secure for your computing resources
Low rates for the used computing capacity
Ease of use
Compatibility of EC2 with other AWS services.

Features of Amazon EC2
Existence of instances of virtual computing environments
AMIs containing details for creating or launching instances
Instance types specifying the hardware of computer hosting the instance
Key pairs in the form of public and private key securing your login information
Instances storage volume for dealing with temporary data.
EBS volumes as persistent volumes of storage for data using EBS
Region and Availability zones as physical locations
Firewall for specifying ports, protocols, and source IP ranges that can access the instances
Tags as metadata assigned to Amazon EC2 resources
Elastic IP addresses that are fixed and are used for dynamic cloud computing

Overview of instances Types
Amazon EC2 instance refers to a virtual server in the cloud.

Available Instance Types
General Purpose
T2 instances type instances ensure CPU performance at the predetermined baseline, but capable of bursting above the baseline, for tackling the workload (Low Traffic, Small database). t2.micro, t2.small, t2.medium, t2, large
M3 instance types ensures a balance of compute network, and memory resources. m3.medium, m3.xlarge, m3.2xlarge
M4 instance types ensure a balance of memory, and network resources. It can be used for mid-size databases. Processing tasks that need more memory, and for cluster computing. m4.large, m4.4xlarge, m4.10xlarge

Compute Optimized
C4 instance type provides highest performing processor at the lowest price in Amazon EC2. c4.large, c4.8large
C3 instance types offer high frequency processors for enhanced networking clustering and instance storage. c3.large, c8.large (high performing scientific application, games etc)

Memory Optimized
The memory optimized family contains only R3 instances
These instances are:
Optimized for running memory-intensive programs
Lowest cost per Gibibyte of RAM
Example: HighPerformance Database, Genome Analysis, Microsoft Sharepoint, Distributed Caches, In-Memory Analytics

Storage Optimized
I2 instances types c contains high Input/Output instances. These are high storage instances providing rapid instance storage backed by Solid-State Drive. i2.xlarge & i2.8large. (NoSQL database, Data warehousing, Scalable transaction application)
D2 instance types contains dense storage instances. d2.2large & d2.8large. (Data processing application, distributed file system, network )

GPU instances

The previous generation types also have one more family called Micro instance types, which are more expensive than a few general purpose instance types.

Additional Instance Types
On-demand instances: When you launch these instances, you pay by the hour.
Reserved instances: You can purchase these at a considerable discount, and employ them for one month to three years.
Scheduled instances: These are always available for one year, and as per the specific recurring schedule.
Spot instances: These are unused instances on which you bid. You can launch them as long as they are available. Spot instances are affordable if you are flexible about the timings to run your application

Describing Instances
Amazon EC2 supports two platforms, namely EC2 classic and EC2 VPC.

Amazon EBS Volume & Snapshots
An Amazon EBS volume refers to a durable storage device attached to an Amazon EC2 instance in the same Availability zone.
It acts as primary data storage that needs regular updates

Benefits of EBS volume
Data Persistence
Data security
Data backups
Data availability

Types of EBS Volumes
Amazon EBS offers different volume type based on price and performance. You can choose and customized as per your application requirements.

The General Purpose SSD Volumes offer affordable storage perfect for virtual desktops, development and testing, system boot volumes, and small to medium databases.
Size ranges from 1 Gibibyte to 16 Tebibytes
Ensure low latency in milliseconds
Burst their ability up to 3,000 IOPS
Provide baseline performance of 10000 IOPS
Features a throughput of up to 128 Mebibytes/sec for volumes size =< 170 Gibibyte
Provisioned IOPS SSD Volumes are best for I/O-intensive workloads, mainly those of large databases, for example, Oracle and MySQL, and other critical application demanding constant I-OPS performance.
Enables you to define the I-OPS rate and it delivers within 10% of the IOPS performance rate 99.9 percent of the time in a year
You can specify up to 20,000 IOPS/volume, with volume size ranging from 4 Gibibyte - 16 Tebibytes, while the throughput range is up to 320 Mebibytes/sec
The Magnetic Volumes are prefect where the emphasis is on least possible storage cost, and workloads where data is accessed rarely.
Deliver almost 100 I-OPS and are capable of bursting up to hundreds of IOPS
Size range from 1 Gibibyte to 1 Tebibyte
The maximum throughput of magnetic volumes is around 40 to 90 Mebibytes per second.

EBS Snapshots
You can create EBS snapshots or backups of any Amazon EBS volume, and copy the volume’s data in Amazon S3.
Even though Snapshots are updated incrementally, they are deleted in a way such that only the most recent snapshot stays back for restoring the volume.
Snapshots are always restricted to the region when they are created.
For a subsequent snapshot, you are charged only for additional data exceeding the original size of the volume.
You can copy snapshots across regions with the status as ‘Completed;, and share the snapshots with required AWS accounts
At the time of issuing the Snapshot command:
Snapshots only include the data written to the attached volume.
Exclude data cached by the operating system or an application
Snapshot of an associated volume that is in use.
To create a snapshot for volumes acting as root devices, it is necessary to stop the instance prior to creating the snapshot.

Restoring Volumes for EBS Snapshots
Active snapshots have the desired information to restore your data to a new EBS volume.
In case, you access a data not yet loaded, the volume loads it immediately before loading the rest of the data block. Restored data blocks on volume needs initialization which can consume time, however it evens out over the lifespan of the volume.

Amazon Dynamo DB
Amazon DynamoDB is a service that offers NoSQL databases.

DynamoDB is a fully managed service and it relives you from the administrative load of running and scaling a distributed database. You can employ it to perform all critical tasks such as setting up a database, providing the required hardware, replicating the tables and scaling the database capacity.

Use of Amazon DynamoDB
Easily create table in a NoSQL database
Service operate fast, delivers the expected performance, and scales flawlessly
Table data is stored on Solid state disks or SSDs and is copied over several availability zones in the region.
Spread a particular table’s data and traffic over an adequate number of servers
Easily scale throughput capacity for reads and writes of tables.
Monitor performance and resource utilization of databases.

Nine DynamoDB features
Document Data Model Support: DynamoB allows storing, updating and querying documents written in the JSON format. These documents are stored directly into tables. Because you can add, retrieve, update and delete items of a table throughout a graphical interface, there is no need to write new code to perform these tasks for JSON documents stored in that table.
Key-value Data Model Support: Each item or row in a table has a key-value pair. Here, only the Primary Key column is mandatory for each row as its value identifies each row uniquely in the table.
Schema-less: DynamoDB has no schema, which means that each row in a table need not have the same attribute or even the same number of attributes. The items can attribute value of different data types, such as strings, binary values, numbers.
Seamless Scaling: DynamoDB ensures flawless storage and throughput scaling through both the AWS Management Console and Application Program Interface(API). There is no maximum limit for you to demand the storage size or read or write throughput.
Local Development but Global Scaling: DynamoDB allows you to build and test applications easily on an EC2 instance in the cloud. Once these applications are ready, DynamoDB allow you scale them quickly in the cloud.
Strong Consistency, Atomic Counter: As compared with several other non-relational databases, DynamoDB ensures easier development of your database environment. It does so by employing strong consistency on reads so that you can read only the latest value. Atomic counters allow you increase or decrease numerical attributes in the atomic manager, through an API call.
Integrated Monitoring: Apart from making major operational metrics available through the AWS Management console, DynamoDB integrates with CloudWatch to comprehensively monitor your databases.
Security: DynamoDB also integrates with AWS identity and Access Management service to control access permissions for you organizational users.
Integration with AWS Data pipeline: This integration allows you to automatically move and transform data into and out of DynamoDB. You can schedule and run recurring jobs without any kind of programming

Five New DynamoDB Features
Streams:
Is a timed sequence of changes at item level in the table
Helps track the latest changes or updates in the last 24 hours
Help take backups, analyze trends, and create innovative application for replication.
Cross-region Replication:
Replicates its table over several regions without any intervention
Helps develop glHandling crirtiobally distributes applications with quicker data migration, better traffic management, lower latency access, and effortless disaster recovery.
Triggers:
Integrates with Lambda offers triggers
Helps execute a user defined task or function when change occurs at an item level in the desired table.
Free-text Search:
Offers q quick and easy-to-use search facility
Enables you to search its content such as keywords, tags and locations
Integrated with Titan Graph database:
Allow efficient storage and navigation through the graphs
Enables you to optimize graphs for quick navigation

Amazon Relational Database Service
Amazon RDS help in:
Running relational database
Scaling a relational database
Setting up relational database

Six Popular Database Engines
Oracle
MySQL
Microsoft SQL Server
PostgresSQL
Maria DB
Amazon Aurora

Eight Reasons for Using Amazon RDS
Scalability: Make it easy to scale database resources
Handling critical tasks: Handles critical tasks its own, such as software patching, backups, failure detection, and recovery.
Fast Service: Provides different sizing options for a database server which are up tp 244 Gigabytes and 32v CPUs
Database engine: A familiar database engine can be chosen and used
High availability & reliability: Service copies data synchronously to a reserved instance residing in another. Availability Zone and allows taking automatic backups or snapshots whenever you want.
Tracking performance of relational database instances: Allow tracking by offering the CloudWatch metrics for free
Low rates: You pay low rates and only for the resources you use.
Security and full control: Ensured over who can access what from your relational databases.

DB instances
Regarded as the building block of Amazon RDS, a DB instances refers to a secluded database environment containing several created database with following features
Can store from five Gigabytes to six Terabytes of data
Each such instance has a DB engine and set of parameters to control the database behavior
At the time of creating the instance, the DB engine is loaded automatically with its own set of features
You may get new features if you choose the latest version of the engine while creating the instance
You can make the necessary change to a DB instance whenever required

DB instance Class
The RDS service uses a DB instance class to identify the computing and memory capacity of an instance

DB Instance Storage Types
General Purpose SSD
Provisional IOPS SSD
Magnetic

Each of these types is different in terms of price and performance aspects so that you can customize your storage cost and performance according to your database requirements

DB Instances in regions and availability zones
Launch the DB instances in different Availability Zones, as it can prevent your applications from being affected by the failure to access a database stored at one location.

Security Groups
A security group aims to control access to a DB instance. The RDS service utilizes three different security groups:
DB security groups for DB instances not in a VPC
EC2 security group for DB instances
VPC security group for DB instances in a VPC
DB security group rules are only for inbound traffic as outbound traffic is currently prohibited for DB instances.
If you use an EC2 security group, you permit incoming traffic from any EC2 instance using security group.

DB Parameter Group
A DB parameter group allows you to specify the configuration of a DB engine
It contains engine configuration values that you can apply to one or more DB instances of the same type.
By default, a parameter group is applied to a DB instance if you do not specify your desired parameter group while creating the instance.

The default parameter group has default settings for a particular database engine and an instance class.

DB Options Groups
A few DB engines provide tools to easily manage the relational databases. Such tool are accessible via the DB option groups.

Currently, the option groups exist only for Microsoft SQL server, MySQL 5.6 and Oracle DB instance

Deployment and Management
AWS CloudFormation
Create and provisions the AWS resources
Enables the administrators and developers to design, manage and update a collection of associated AWS resources
Administrators and developers can spend more time to run other application in AWS, instead of handling the associated resources
You need to describe all the AWS resources such as Amazon EC2 and Amazon RDS instances
Automatically configures the resources, and identifies the dependent resources
You can take advantage of several AWS products, such as Amazon EC2, Auto Scaling, and Amazon ELB
Allows to develop highly scalable, reliable and lucrative applications without configuring the underlying cloud infrastructure
Free of cost, and you only pay for the resource you use for your applications

Three Key benefits of Amazon CloudFormation
You get to work with a simplified infrastructure management
You can easily and quickly replicate your infrastructure across regions
You are ensured of easy control and tracking of the infrastructure

CloudFormation Components - Template & Stack
The key component of AWS CloudFormation are templates and stacks
Describe resources, their properties, and runtime parameters
Conforms to the Javascript Object Notation
Depicts AWS infrastructure
File extension .txt, .json or .template
A blueprint for creating and configuring your resource

A Stack is an array of resources manageable as a single unit
A stack can have all resources needed to run a Web Application, such as Database or Web Server.

AWS cloud formation create manages, and updates a collection of resources by creating, managing and updating stacks.

A Stack is handle as a single unit.
If Anyone resource fails to create itself then stack is rolled back or deleted
If All the resource are retained Then stack is not deleted

The AWS CloudFormation console allows creating and updating templates along with the repeated collection of resources or stacks

—>Amazon EC2
—>Instance types
Stack With Template —> Describes —> Key Pair name
—>EBS Volume
—> Additional Resources

Eight Section of Template Structure
Format Version: It mention the AWS template version. Syntax: “AWSTemplateFormatVersion”:”2010-09-09”
Description: This follows the FormatVersion section, and contains a text string that describes the template. Syntax: “Description”: “Here are some details…”
Metadata Section: It provide extra information about the template. Syntax: “Metadata”: { “Instances” : {}, …}
Parameter Section: It contains value to be passed at runtime to the template. These values are passed while creating or updating a stack. Syntax: “Parameters”: { “InstanceTypeParameter”: { “Type”: “string”, “Default” :”t1.micro”, ….}
Mapping Section: It includes mapping of keys. Their values are similar to a lookup table. Syntax: “Mappings”: { “Mapping01”: { “Key01”: {“Name”: “Value01”},…}}
Conditions Section: It specifies the condition for creating some specific resources only when they are fulfilled. Syntax: “Conditions” : { “Logical ID”: { Intrinsic Function}}
Resource Section: It contains the stack resource along with their properties. Resource is the only mandatory section. Syntax: “Resources”: { “Logical ID” : { “Types” : “Resource type”, “Properties” : {Set of properties}}}. Its mandatory section
Output Section: It contains the returned value while observing the stack’s properties. Syntax: “Outputs” : { “Logical ID” : { “Description”: “Information about the value”, “Value” : “Value to return”}}

Step to Create and Configure Resources
Create a template, or choose an existing template
Save the template with a suitable extension, Locally or in Amazon S3 bucket
Create a stack and mention the template’s location, which can be either your local computer or Amazon S3.

Amazon CloudWatch Metrics
Amazon CloudWatch is a service that allows real-time monitoring of cloud resources such as, Amazon EC2, Amazon RDS instances, and other applications.
Gather and track log files and metrics
Set alarms
View the latest statistics and graphs
Respond to change in the resources

Gaol of Amazon CloudWatch
The goal of Amazon CloudWatch is to give system-wide insights into the usage of resources, performance of applications, and status of conducted tasks.
View the trends
Troubleshoot systems
Setup an automates action

Benefits of Amazon CloudWatch
You can monitor the Amazon EC2 instance by viewing the basic metrics for disk and CPU usage, along with the data transfer rate at no extra cost.
You can keep an eye on other AWS resource at no extra cost or software
You can monitor custom metrics generated by our own application
You can efficiently monitor and store the logs for troubleshooting your application and systems at any point in time
You can receive notifications or take a quick automated action by setting an alarm on any metrics.
You can view the statistics and graph in the CloudWatch Dashboard. You can reuse few graphs of custom metrics to quickly monitor the operational status, and detect any issue.
You can easily detect and respond to resource changes due to CloudWatch Events.

Overview of Metrics
Metrics refer to indicates that signify the performance of employed AWS resources, and your application in the cloud.
Metric such as request count, latency and CPU utilization for these resources.

Custom Metrics
Amazon CloudWatch monitors custom metrics generated by our application which includes;
Memory usage
Page Load Times
Error rates

You can provide these metrics through a simple Application Program interface, or API request.

You can set your application to send a specific page load time through an API. All cloud watch functionalities, such as statistics and alarms, are accessible at up to one-minute frequency.

Alarms
Alarms are set in any metric to obtain notifications, or respond automatically if a particular metric goes beyond the mentioned threshold.
Example 1: If an Amazon EC2 metric extends beyond the alarm threshold, you can dynamically remove the instances using the Auto Scaling service
Example 2: Set an alarm to shut down the underutilized or unused Amazon EC2 instance.

Working of Alarms
An alarm keeps a watch on a single metric for a specified period, and perform single or multiple actions
It takes a specified action, when the gained metric value surpasses the specified threshold of the metric monitored.
A change in state makes the alarm invoke an action but only for the lasting changes
No action is taken until the changed state of a resource or application is maintained for the mentioned number of periods.
Taking an action involves passing a notification to the Auto scaling policy or the Amazon Simple Notification Service or SNS topic
It is your responsibility to ensure the actions are performed. Because, CloudWatch is not designed to monitor the actions or its resulting errors.

AWS Identity And Access Management

Need for AWS Identity and Access Management (IAM)
AWS CloudFormation can only take those actions you are permitted to perform.
To manage such permission, you use AWS identity and Access Management, or IAM.

Nine Key Features of IAM
Allow other people to use your resources without providing password.
Different permissions to different users, for different resources.
Allow applications running on Amazon EC2 to securely access AWS resources.
Enable two-factor authentication for individual users and your account.
Users can temporarily access your account using your account password.
Obtain logs that have information about the users who are a part of the IAM identities.
Supports storing, transmitting, and processing of credit card data.
Integrate IAM with almost all other AWS services.
Free to use and pay only for other AWS services.

Functionality of Identity and Access Management
IAM users and access : Manage users by giving individual credentials such as password and multi-factor authentication code, after creating them through IAM.
IAM role and permissions: Manage user roles by controlling permissions for the operations that they can perform, after creating them.
Federated users and permissions: Here use the features of identity federation for enabling several identities without creating an IAM user for each identity

Need for an IAM User
To prevent user from accessing your root account.
To assign different authorization permission or policies to some users accessing particular services and their resources.
To use the AWS Command Line Interface.
To use a role.
To allow federated users or external identities to access resource securely.

Policies and User Group
A policy refers to a document containing actions to be performed, and the resources on which those actions can be performed.
For example: If a policy does not mention accessing the books table through an Amazon DynamoDB action, the user cannot access the table through any Amazon DynamoDB action.

A group is set of IAM users with common permissions assigned through a group policy.
You can create a group called Warehouse Administrators, and provide suitable permissions to that group. Any group user automatically has the permissions you assign to the group.
To grant administrator privileges to a user, add the user to the Warehouse Administrator group.
If a user changes jobs in your company, move the user to new the group, instead of changing the permissions.

IAM Roles
IAM Roles are similar to users, but are AWS identities associated with permission policies that determine what the identity can perform. A role has no credentials associated with it. When you assign a role to a user, access key are generated automatically and sent to the user.

Scenarios for Using Roles
You want to give users of an AWS account access to resources in the other account.
You want a mobile app to use cloud resources, without embedding credentials in the application where users can extract them.
You want to give access to users having identities outside AWS, such as in your business directory.
You want to give the external parties access to your AWS account for auditing your resources.