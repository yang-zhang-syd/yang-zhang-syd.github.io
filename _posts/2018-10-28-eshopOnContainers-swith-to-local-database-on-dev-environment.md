---
layout: post
title:  "eshopOnContainers Switch to Local Database on Dev Environment"
date:   2018-10-28 17:00:00 +1000
categories: tech blogs
---
It is too heavy to run sql server in a docker container on a development machine. The goal of this article is to introduce a way to use sql server express installed locally instead. 

I have cloned a copy from the main repo and updated the configuration to use local database instead of run a separate sql server inside docker. 

It can be found from [here](https://github.com/yang-zhang-syd/eShopOnContainers-Tutorial)