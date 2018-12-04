---
layout: post
title:  "Use docker compose with environment variables"
date:   2018-12-04 00:00:00 +1000
categories: tech blogs
---
Environment variables can be defined inside docker compose files. Most of the time, some environment variables are used repeatedly, for example api gateway urls, DNS servers and identity server urls. 

We can declare reusable variables inside a docker compose file and pull the values out of it into a ::.env:: file under the same folder.
<!--more-->
Below is from a section of an example `docker-compose.yml` file:

```
basket.api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - ConnectionString=${ESHOP_AZURE_REDIS_BASKET_DB:-basket.data}
      - identityUrl=http://identity.api              #Local: You need to open your local dev-machine firewall at range 5100-5110.
      - IdentityUrlExternal=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5105
      - EventBusConnection=${ESHOP_AZURE_SERVICE_BUS:-rabbitmq}
      - EventBusUserName=${ESHOP_SERVICE_BUS_USERNAME}
      - EventBusPassword=${ESHOP_SERVICE_BUS_PASSWORD}      
      - AzureServiceBusEnabled=False
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
      - UseLoadTest=${USE_LOADTEST:-False}

    ports:
      - "5103:80"
```

Note that the environment variables are referenced by using a syntax like ::${Variable_Name}::. These variables are defined in `.env` file under the same folder as below:

```
ESHOP_EXTERNAL_DNS_NAME_OR_IP=localhost
ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP=10.121.122.162
ESHOP_AZURE_CATALOG_DB=Server=192.168.1.100,1433;Database=Microsoft.eShopOnContainers.Services.CatalogDb;User Id=sa;Password=Pass@word
ESHOP_AZURE_IDENTITY_DB=Server=192.168.1.100,1433;Database=Microsoft.eShopOnContainers.Service.IdentityDb;User Id=sa;Password=Pass@word
ESHOP_AZURE_ORDERING_DB=Server=192.168.1.100,1433;Database=Microsoft.eShopOnContainers.Services.OrderingDb;User Id=sa;Password=Pass@word
ESHOP_AZURE_MARKETING_DB=Server=192.168.1.100,1433;Database=Microsoft.eShopOnContainers.Services.MarketingDb;User Id=sa;Password=Pass@word
```