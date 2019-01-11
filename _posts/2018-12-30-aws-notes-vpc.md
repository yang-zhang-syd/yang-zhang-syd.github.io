---
layout: post
title:  "AWS Notes - VPC"
date:   2018-12-30 00:00:00 +1000
categories: tech blogs
---

* IP ranges:
> 10.0.0.0 ~ 10.255.255.255 (10/8 prefix)  
> 172.16. 0.0 ~ 172.31.255.255 (172.16/12 prefix)  
> 192.168.0.0 ~ 192.168.255.255 (192.168/16 prefix)  
* Default 5 VPCs in a region
<!--more-->
* VPC can be peered with other VPCs from another account.
* 1 Subnet = 1 Availability Zone
* Secure Groups are stateful (open port 80 all in once), Network Control Lists are stateless (open port 80 for both inbound and outbound separately)
* There is no transitive peering support.
* 5 IP addresses will be reserved by AWS in a subnet ending in: 0, 1, 2, 3, 255.
* Only one IGW can be attached to a VPC
* Security Groups will not span multiple VPCs. They are only available in its own VPC.
* Open ICMP port in an EC2 instance to enable ping the instance from public network.
* You can only associate a subnet to a single ACL. One ACL can be associated with multiple sub nets.
* Default ACL allows all inbound and outbound traffic.
* When a private ACL is created all inbound and outbound traffics are denied.
* ACL rules are evaluated in the order of numbers.
* Block IP Addresses using ACL not Security Groups.
* Application load balancer requires at least two public availability zones.
* You can use Security Group to create a jump box to connect instances in a private subnet.
* When creating a NAT instance, remember to disable source/destination check.
* NAT must be in a public subnet
* NAT instance is behind a security group
* Security Groups operate at the instance level, they support "allow" rules only, and they evaluate all rules before deciding whether to allow traffic.
* VPC Flow Logs can be created at the VPC, subnet, and network interface levels.
* Each subnet must reside entirely within one Availability Zone and cannot span zones.
* When you create a custom VPC, a default Security Group, Access control List, and Route Table are created automaticaly. You must create your own subnets, Internet Gateway, and NAT Gateway (if you need one.)
* When a VPC is created, an ACL, a Security Group and a Route Table is created.
* ACL can be assigned to multiple subnets. 
* Security Groups are assigned to instances.