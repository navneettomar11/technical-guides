# Monolithic Architecture
You are developing a server-side enterprise application. It must support a variety of different clients including desktop browsers, mobile browsers and native mobile applications. The application might also expose an API for 3rd parties to consume. It might also integrate with other applications via either web services or a message broker. The application handles request (HTTP requests and messages) by executing business logic; accessing a database; exchanging messages with other systems; and returning a HTML/JSON/XML response. There are logical components corresponding to different functional areas of the application.

# Problem
What's the application's deployment archiecture?

# Forces
- There is a team of developers working on the application
- New team members must quickly become productive.
- The application must be easy to understand and modify.
- You want to pratice continous deployment of the application.
- You must run multiple instances of the application on multiple machines in order to satisfy scalability and availability requirements. 
- You want to take advantage of emerging technologies(framework, programming languages, etc).

# Solution
Build an application with a monloithic archiecture. For example - 
- a single Java WAR file.
- a single directory hierarchy of Rails or NodeJS code.

# Example
Let's imagine that you are building an e-comerce application that takes orders from customers, verifies inventory and available credit, and ship them. The application consists of several components including the StoreFrontUI, which implements the user interface, along with some backend services for checking credit, maintaining inventory and shipping orders.

The application is deployed as a single monolithic application. For example, a Java web application consists of a single WAR file that runs on a web container such as Tomcat. A Rails consists of a single directory hierarchy deployed using either, for example, Phusion Passenger on Apache/Ngix or JRuby on Tomcat. You can run multiple instance of the application behind a load balancer in order to scale and improve availability.

![Decomposing Application](assets/DecomposingApplications.011.jpg)


# Resulting Context
This solution has a number of benefits:
- **Simple to develop** - the goal of current development tools and IDEs is to support the development of monolithic applications.
- **Simple to deploy** - you simply need to deploy the WAR file(or directory hierarchy) on the appropriate runtime.
- **Simple to scale** - you can scale the application by running multiple copies of the application behind a load balancer.

However, once the application becomes large and the team grow in size, this approach has a number of drawbacks that become increasingly significant:
- The large monolithic code base intimidates developers, especially ones who are new to the team. The application can be diffcult to understand and modify. As a result, development typically slows down. Also, because there are not hard module boundaries, modularity breaks down over time. Moreover, because it can be diffcult to understand how to correctly implement a change the quality of the code declines over time. It's downward spiral.
- Overloaded IDE - the large the code base the slower the IDE and the less productive developers are.
- Overloaded web containers - the larger the application the longer it takes to start up. This had have a huge impact on developer productivity because of time wasted waiting for the container to start. It also impacted deployment too.
- Continous deployment is diffcult - a large monolithic application is also on obstacle to frequent deployments. In order to update one component you have to redeploy the entire application. This will interrupt background tasks (e.g. Quartz jobs in Java application), regardless of whether they are impacted by the change and possibly cause problems. There is also the chance that components that haven't been updated will fail to start correctly. As a result, the risk associated with redeployment increase, which discourages frequent updates. This especially a problem for user interface developers, since they usually need to iterative rapidly and redeploy frequently.
- Scaling the application can be diffcult - a monolithic architecture is that can only scale in one dimension. On the one hand, it can scale with an increasing transaction volumne by running more copies of the application. Some clouds can even adjust the number of instances dynamically based on load. But on the other hand, this architecture can't scale with an increasing data volume. Each copy of application instance will access all of the data, which makes caching less effective and increase memory consumption and I/O traffic. Also, different application components have different resource requirements - one might be CPU intensive while another might memory intensive. With a monolithic architecture we cannot scale each component independently.
- Obstacle to scaling development - A monolithic application is also an obstacle to scaling development. Once the application gets to a certain size its useful to divide up the engineering organization into teams that focus on specific functional areas. For example - we might want to have the UI team, accounting team, inventory team, etc. The trouble with a monolithic application is that it prevent the teams from working independently. The teams must coordinate their development efforts and redeployment. It is much more diffcult for a team to make a change and update production.
- Requires a long-term commitment to a technology stack - a monolithic architecture forces you to be married to the technology stack ( and in some cases , to a particular version of that technology) you chose at the start of development. With a monolithic application, can be diffcult to incrmenttly adopt a new technology. For example, let imagine that you choose the JVM. You have some languages choices since as well as Java you can use other JVM language that inter-operate nicely with Java such as Groov and Scala. But components written in no-JVM languages do not have a place within your monolithic archiecture. Also, if your application uses a platform framework that subsequently becomes obsolete then it can be challenging to incrmentally migrate the application to a newer and better framework. It's possible that in order to adopt a newer platform framework you have to rewrite the entire application, which is a risky undertaking.
