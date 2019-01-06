---
layout: post
title:  "AWS Notes - Tips"
date:   2019-01-03 00:00:00 +1000
categories: tech blogs
---

* Kinesis for social media, news feeds, logs.
* Redshift for Business intelligence.
* Elastic Map Reduce for Big data processing

* Cloudtrail is per AWS account per region
* Can consolidate logs using S3 bucket cross multiple accounts

* AWS Organisation Service Control Policies will override individual account policies.

* To connect your data center with AWS, you will need a customer gateway on your side and a virtual private gateway on AWS. 
* An internet gateway is to connect a VPC to the internet and NAT gateway connect the servers running in private subnet to the internet.
* A VPC endpoint enables you to privately connect your VPC to supported AWS  services.  
* You cannot do vpc peering cross region. But can do it cross accounts.
* You cannot creat VPC peering when the two VPCs have matching or overlapping CIDR blocks.
* VPC peering does not support transitive peering relationships.
* SSL certificates will only be useful to encrypt data in transit, not data at rest.
* ELB can span multiple AZs within a region. It cannot span multiple regions.
* The customer is responsible for the security of anything running on the hypervisor, and therefore the operating system and the security of data are the customer’s responsibility.