# Cloud Lab 2 Submission — Load Balancer and Auto Scaling

Copy this template to `cc-lab-YOUR_ID/lab2/README.md` in your own repository. Follow [the instructions](lab2-instruction.md). Put your screenshots in `lab2/images/` with the file names below.

- **Student name:*Sreng Sopheakanha*
- **Student ID:*e20220477*
- **Learner Lab user name:*SRENG_SOPHEAKANHA*
- **Date completed:*30/10/2026*

---

## Security groups

![LoadBalancerSecurityGroup](images/01-sg-load-balancer.png)

![LaunchTemplateSecurityGroup](images/02-sg-launch-template.png)

## Target group

![LoadBalancerTargetGroup](images/03-target-group.png)

![Health checks](images/04-health-checks.png)

## Application Load Balancer

![Load balancer list](images/05-load-balancer-list.png)

![Load balancer details](images/06-load-balancer-details.png)

## Launch template

![AutoScalingLaunchTemplate](images/07-launch-template.png)

## Auto Scaling group

![Auto Scaling groups](images/08-asg-list.png)

![Auto Scaling group details](images/09-asg-details.png)

## Load balancing test

![First request](images/10-browser-1.png)

![Second request, different host](images/11-browser-2.png)

## Simulated application failure

![Stopping httpd](images/12-stop-httpd.png)

![Target unhealthy or draining](images/13-target-unhealthy.png)

![New target healthy](images/14-target-replaced.png)

About 5 (plus internet issue) minutes passed between stopping `httpd` and the new instance becoming healthy?

---

## Questions

1. Why does the page show a different host name when you refresh it?

Different host name: the load balancer spreads requests across the instances, and each instance’s index.html has its own host name.

2. After you stopped `httpd`, the instance was still *running* in EC2, but the target group marked it *unhealthy*. What did the health check test, and why did the instance fail it?

Health check: it requests / on port 80 every 30 seconds and expects a 200. After httpd stopped, nothing answered, and two failures in a row made the target unhealthy. EC2 still showed the instance running because it only checks the VM, not the application.

3. Who replaced the failed instance: the load balancer, the target group, or the Auto Scaling group? Explain the role of each one.

Who replaced it: the Auto Scaling group. The load balancer routes traffic, the target group runs the health checks and reports status, and the Auto Scaling group terminates the failed instance and launches a new one.

4. With desired 2, minimum 2 and maximum 3, how many instances run normally? In what situation could there be 3?

Capacity: 2 normally. It could reach 3 only if the desired capacity is raised to 3, manually or by a scaling policy, and this lab has no scaling policy.

5. The instances' security group allows HTTP from anywhere. How could you change it so that the instances accept HTTP **only** from the load balancer?

Locking down HTTP: edit the HTTP rule in LaunchTemplateSecurityGroup and change the source from 0.0.0.0/0 to LoadBalancerSecurityGroup.


## Problems and how I solved them (optional)

## AI note (optional)

One suggestion an AI tool gave you and how you checked it.

## Clean up

- [X] `MyAutoScalingGroup` deleted and its instances terminated.
- [X] `AutoScalingLaunchTemplate` deleted.
- [X] `MyLoadBalancer` and `LoadBalancerTargetGroup` deleted.
- [X] Both security groups deleted.