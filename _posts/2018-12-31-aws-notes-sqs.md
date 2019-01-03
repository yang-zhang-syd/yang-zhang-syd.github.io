---
layout: post
title:  "AWS Notes - SQS"
date:   2018-12-31 00:00:00 +1000
categories: tech blogs
---

* Messages can contain up to _256kb_ of text in any format.
* Standard Queue more than one copy of a message might be delivered _out of order_.
* FIFO queue ensure ::ordering:: and a message is delivered ::once:: only. 
* SQS is ::pull based::.
* Messages can be kept from **1 min to 14 days**, default is **4 days**. 
* SQS guarantees messages will be processed at least once. 
* SQS visibility timeout: default is 30 sec, up to 12 hours.
* Short polling vs Long polling
