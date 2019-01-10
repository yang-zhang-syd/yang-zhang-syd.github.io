---
layout: post
title:  "AWS Notes - Route53"
date:   2019-01-10 00:00:00 +1000
categories: tech blogs
---

* A CNAME record assigns an Alias name to a Canonical Name. This is a normal DNS capability as defined in the RFCs. By design CNAMEs are not intended to point at IP addresses.
* Route53 has a security feature that prevents internal DNS from being read by external sources. The work around is to create a EC2 hosted DNS instance that does zone transfers from the internal DNS, and allows itself to be queried by external servers. 
* Route 53 supports alias resource record sets, which enables routing of queries to a CloudFront distribution, Elastic Beanstalk, ELB, an S3 bucket configured as a static website, or another Route 53 resource record set
* Alias records are not standard for DNS RFC and are an Route 53 extension to DNS functionality
* Alias records help map the apex zone (root domain without the www) records to the load balancer DNS name as the DNS specification requires "zone apex" to point to an ‘A’ record (ip address) and not to an CNAME
* Route 53 automatically recognizes changes in the resource record sets that the alias resource record set refers to for e.g. for a site pointing to an load balancer, if the ip of the load balancer changes, Route 53 will reflect those changes automatically in the DNS answers without any changes to the hosted zone that contains resource record sets
* If an alias resource record set points to a CloudFront distribution, a load balancer, or an S3 bucket, the time to live (TTL) can’t be set; Route 53 uses the CloudFront, load balancer, or Amazon S3 TTLs.