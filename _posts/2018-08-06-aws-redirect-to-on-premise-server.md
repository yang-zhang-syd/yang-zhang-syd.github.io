---
layout: post
title:  "Integrate AWS Services With On-Premise Servers"
date:   2018-08-06 20:00:00 +1000
categories: tech blogs
---
### Summary
In this blog, I will introduce a solution which enables integration of AWS services with your on-premise servers. The idea is to utlise the felxibilities of the cloud while maintaining security of on-premise servers. 
<!--more-->

### Steps

#### 1. If you have already registered a domain, follow this [Blog Post](https://lobster1234.github.io/2017/05/10/migrating-a-domain-to-amazon-route53/) to migrate it to Amazon Route 53. 

#### 2. Follow [This Blog Post](https://hackernoon.com/using-a-vpn-server-to-connect-to-your-aws-vpc-for-just-the-cost-of-an-ec2-nano-instance-3c81269c71c2) to setup a VPN Server (EC2 Instance) on AWS

#### 3. Create a load balancer to redirect traffic to EC2 Instance.

#### 4. Install Nginx on EC2 Instance created in step 2.  
We will use Nginx as a proxy for three purposes. First, we use Nginx to redirect all http requests to https url. To do this we have the following configuration. 

Edit the default Nginx configuration file ```/etc/nginx/sites-enabled/default``` to make the following changes.

```
server {
        listen 8080 default_server;
        listen [::]:8080 default_server;

        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name _;
        rewrite ^(.*) https://$host$1 permanent;
        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
}
```
*Note: ```rewrite ^(.*) https://$host$1 permanent;``` will redirect all http requests to https with the same requested path.*

Second, forward the request for ```www.sydneybb.com``` to our on-premise server on port 80.
```
server {
    listen 80;
    listen [::]:80;

    server_name www.sydneybb.com;

    location / {
        proxy_pass http://10.8.0.6:80;
    }
}
```

Third, forward the request for ```apps.sydneybb.com``` to our on-premise server on port 81.
```
server {
    listen 80;
    listen [::]:80;

    server_name apps.sydneybb.com;

    location / {
        proxy_pass http://10.8.0.6:81;
    }
}
```
*Note: ```10.8.0.6``` is the on-premise server ip address on VPC*

#### The overall architecture is as below
Inline-style: 
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")
