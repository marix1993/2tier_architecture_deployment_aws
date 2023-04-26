Creating a load balancer and autoscaling group.
-

1. On EC2, on the left click on Launch Template then create launch template:

![launch_template.png](files%2Flaunch_template.png)

2. On the following page give your template a 
name using a similar naming convention `mateusz-tech221-ASG-app`, 
and check the Auto scaling guidance.

![auto_scaling.png](files%2Fauto_scaling.png)

3. Then Find your AMI, in our case we used Ubuntu 18.04 (18.04 LTS, amd64 bionic image build on 2022-09-01).
4. Make instance type t2.micro, and key pair name as your key pair, in our case this was tech221.
5. On `Security Groups`, we use an existing security group with our HTTP, SSH 
and 3000 ports - HTTP and 3000 on `anywhere` adn SSH to `my ip`.
6. Under Advanced Details, we scroll to User data and input the following code.

```#!/bin/bash 
sudo apt update -y 
sudo apt upgrade -y 
sudo apt install nginx -y
```

![usd.png](files%2Fusd.png)

7. Click `Create launch template`, and navigate on the left to `Auto scaling groups`.
8. Launch ASG, and now we use similar naming convention to before for the group name
`mateusz-tech221-app-asg`, and then we select our `Launch template` we created before, click next.
9. We use `default vpc`, and for our `Availability Zones` (AZ) we select 3 AZs, click next

![az.png](files%2Faz.png)

10. Check `Attach to a new load balancer`, then we check `Application load 
balancer` as we are dealing with HTTP and HTTPS, and make 
sure we check `Internet facing` , as we are working over the internet.

![conf.png](files%2Fconf.png)

11. Scroll down to the ports section and click into `default routing` click on it
and select `Create a target group`.
12. On health checks, we check to `turn on the health checks`, 
when unhealthy, auto scaling can create new 
EC2 instances when necessary, click next.

![health.png](files%2Fhealth.png)

13. Set the group size, desired capacity at 2, min capacity 
at 2, and maximum capacity at 3, so there is always 2 instances, 
and if needed, the maximum it can make is 3 instances. For scaling 
policies click target tracking scaling policy, change it if 
necessary and we keep it the same. Click next.

![size.png](files%2Fsize.png)

14. On the tags, we add `Name` to the `Key`, and we use a naming 
convention like `mateusz-tech221-asg-alb-app`, 
click `create Auto Scaling group`.
15. Navigate to Instances, and there should be 2 instances 
available. To further check the auto scaling and load balancing, 
terminating one instance, will result in another instance being 
creating automatically
