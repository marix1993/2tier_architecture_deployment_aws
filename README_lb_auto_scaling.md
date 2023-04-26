Autoscaling and Load balancing.
-

- **Autoscaling** is a feature on AWS that automatically adjusts 
the amount of computing resources (such as servers or virtual machines) 
that your application has access to, based on changes in demand. So if your 
application suddenly gets a lot of traffic, Autoscaling can quickly spin up more 
computing resources to handle the increased load. And when the demand decreases, 
Autoscaling can scale down the number of resources being used, which can save you 
money.


- **Load balancing** is another feature on AWS that distributes 
incoming traffic across multiple computing resources, such as 
servers or virtual machines, to ensure that no single resource 
becomes overwhelmed. This can help prevent downtime and ensure that 
your application remains available to users. Load balancing can also
work in conjunction with Autoscaling, so that as more resources are added 
or removed, the load balancer can adjust which resources are handling the traffic.

In short, Autoscaling and Load balancing are both tools that help ensure your application can handle changes in demand and remain available to users, while also helping you optimize your use of computing resources.

When you use **Autoscaling** and **Load balancing** together, 
you create an Autoscaling group with multiple instances, and then 
attach a load balancer to the group. This load balancer distributes 
incoming traffic across the instances in the group, and as demand changes, 
Autoscaling can add or remove instances from the group as needed. This ensures 
that your application can handle changes in demand, while also ensuring that no 
single instance becomes overwhelmed with traffic.

![auto_sc_load_bal_dia.png](files%2Fauto_sc_load_bal_dia.png)

- Load balancing distributes incoming traffic across multiple EC2 instances, we can see this in the diagram above.
- Application load balancers (ALB) is provided by AWS : Routes incoming traffic to Targets such as EC2 instances.
- Autoscaling is a feature which can automatically adjust the number of EC2 instances based on the current demand of the application.
- You can scale up - increase resources allocated.
- Scale out - adding more EC2 instances to handle increased workload.
- Scaling down - reducing resources allocated depending on demand.
- You can use both Autoscaling and Load balancing together as shown in the diagram.
- When combined, lets you create a system to automatically add/remove EC2 instances in response to the load.
- The LB will route traffic across these instances, and will deem them to be healthy or not.
- If needed autoscaling will ad new instance to the auto scaling group.
- Auto scaling can remove an instance from the group.
- If an existing instance is deemed unhealthy, it will be taken out of the service.






