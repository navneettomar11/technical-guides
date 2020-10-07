# Load Balancer
Load balancer is another critical component of any distributed system. It helps to spread the traffic across a cluster of servers to improve responsiveness and availability of applications, websites or databases. LB also keep track of the status of all the resources while distributing requests. If a server is not available to take new request or is not responding or has elevated error rate, LB will stop spending traffic to such a server.

Typically a load balancer sit betweens the client and the server accepting incoming network and application traffic and distributing the traffic across multiple backend servers using various algorithms. By balancing application requests across multiple servers, a load balancer reduces individual server load and prevent any one application from becoming a single point of failure, thus improving overall application availability and responsiveness.

To utilize full scalability and redundancy, we can try to balance the load at each layer of the system. We can add LBs at three places.
- Between the user and the webserver
- Between the webserver and an internal platform layer, like application servers or cache servers.
- Between internal platform layer and databases.

## Benefits of Load Balancing
- User experience faster, uniterrupted service. Users won't have to wait for a single struggling server to finish its previous tasks. Instead, their requests are immediately passed on to more readily available resource.
- Service providers experience less downtime and higher throughput. Even a full server failure won't affect the end user experience as the load balancer will simply route around it to a healthy server.
- Load balancing makes it easier for system adminstrators to handle incoming requests while decreasing wait time for users.
-  Smart load balancers provide benefits like predictive analytics that determine traffic bottlenecks before they happen. As a result, the smart load balancer gives an organization actionable insights. These are key to automation and can help drive business decisions.
- System adminsistrators experience fewer failed or stressed components. Instead of a single device performaning a lot of work, load balancing has several devices perform a little bit of work.
