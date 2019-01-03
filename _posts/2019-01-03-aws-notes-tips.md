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