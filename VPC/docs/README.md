# AWS Solution — VPC with public and private subnets
Architecture summary
- Created a VPC spanning 2 Availability Zones (AZs).
- 2 Public subnets (one per AZ) and 2 Private subnets (one per AZ).
- Public subnets host the Application Load Balancer (ALB) and NAT gateways (if used).
- Private subnets host Auto Scaling EC2 instances (application servers).
- ALB distributes traffic to Target Groups containing EC2 instances in private subnets.

- VPC (Virtual Private Cloud): A logically isolated virtual network in AWS. It contains subnets, route tables, internet gateways, NAT gateways, and network ACLs to control networking for resources.
- Public Subnet: A subnet with a route to an Internet Gateway (IGW). Resources in a public subnet can have public IPs and be reachable from the internet (e.g., ALB).
- Private Subnet: A subnet without a direct route to the Internet Gateway. Resources in private subnets do not receive public IPs and are isolated from direct inbound internet access (e.g., application EC2 instances). Outbound internet access may be provided via a NAT Gateway in a public subnet.
- Auto Scaling: Service that automatically adjusts the number of EC2 instances in an Auto Scaling Group (ASG) based on scaling policies, schedules, or health checks to maintain application availability and handle load changes.
- Application Load Balancer (ALB): A Layer 7 load balancer that distributes incoming HTTP/HTTPS traffic across targets (EC2 instances, IP addresses, Lambda functions) based on rules, host/path-based routing, and health checks.
- Target Group: A logical group of targets (for example, EC2 instances) registered with a load balancer. Health checks are configured per target group and the ALB routes traffic to healthy targets.
- EC2 (Elastic Compute Cloud): Virtual server instances used to run applications. EC2 instances can be launched into public or private subnets depending on required accessibility.

- Internet-facing ALB is placed in the public subnets and receives traffic from clients.
- ALB forwards requests to a target group consisting of EC2 instances in private subnets.
- Auto Scaling Group runs across the two private subnets (AZs) to provide high availability and scale-out/scale-in.
- NAT Gateway (optional) is placed in a public subnet to allow private subnet instances to initiate outbound internet access for updates or external calls while remaining unreachable from the internet.

- Route tables: one for public subnets (route 0.0.0.0/0 -> IGW) and one for private subnets (route 0.0.0.0/0 -> NAT Gateway).
- Security Groups: ALB security group allows inbound HTTP/HTTPS from the internet; EC2 security group allows inbound from the ALB security group only.
- Health checks: configure target group health checks (HTTP/HTTPS path) so ALB only routes to healthy instances.

This README documents the core components and their roles for a VPC with 2 public and 2 private subnets across 2 AZs, using Auto Scaling, an ALB and Target Groups to serve EC2 application servers.
```
