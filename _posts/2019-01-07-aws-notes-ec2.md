---
layout: post
title:  "AWS Notes - EC2"
date:   2019-01-07 00:00:00 +1000
categories: tech blogs
---

* EC2 supports `On Demand`, `Reserved`, `Spot`, `Dedicated Hosts` access.
*  SSD: General Purpose SSD, Provisioned IOPS SSD.
* Magnetic: Throughput Optimized HDD, Cold HDD, Magnetic
* One EC2 instance can have multiple security group.
* All inbound traffic is blocked by default and all outbound traffic is allowed by default.
* Inbound rules are automatically applied to outbound rules.
* EFS can be mounted to multiple EC2 instances
* EBS is replicated in multiple physical devices in the same AZ.
* There is a soft limit of 20 instances per region.
* You cannot create an unencrypted volume from an encrypted snapshot or encrypt an existing volume
* ELB can help to support statefull service
* Once a VPC is set to Dedicated hosting, it is not possible to change the VPC or the instances to Default hosting. You must re-create the VPC. Further information
* There is no limit to the number of EC2 instances you can have in the Auto Scaling group. However, there might an EC2 limitation in your account that can be increased by logging a support ticket.
* ELB can span multiple AZs within a region. It cannot span multiple regions.
* The customer is responsible for the security of anything running on the hypervisor, and therefore the operating system and the security of data are the customer’s responsibility.
* With proper scripting and scaling policies, the On-demand instances behind the Spot instances will deliver the most cost-effective solution because the on-demand will only spin up if the spot instances are not available.
* EBS, S3 and EFS all allow the user to configure encryption at rest using either the AWS Key Management Service (KMS) or, in some cases, using customer provided keys. The exception on the list is Elasticache for Memcached which does not offer a native encryption service, although Elasticache for Redis does.
* The Public IP address is not managed on the instance: It is, instead, an alias applied as a network address translation of the Private IP address.
* AWS does not copy launch permissions, user-defined tags, or Amazon S3 bucket permissions from the source AMI to the new AMI. 
* You can not encrypt an exiting EBS volume.
* AWS reserves both the first four and the last IP address in each subnet's CIDR block.