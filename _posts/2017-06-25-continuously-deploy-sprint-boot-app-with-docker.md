---
layout: post
title:  "Continuously Deploy Spring Boot Applications With Docker"
date:   2017-06-25 17:00:00 +1000
categories: tech blogs
---
>* You can download the [**source code**](https://github.com/yang-zhang-syd/spring-boot-docker-ci-example) on Github.

Spring Boot is a lightweight and fully fledged framework written in Java. It can be used to build RESTful APIs as well as console applications. It provides functionalities such as Dependency Injection, Database Connection Pool Management out of the box so that developers can focus on the business logics instead of struggling with nuts and bolts.
<!--more-->
There are a few benefits to do CI for Spring Boot applications in docker containers.

1. Make the whole CI processes documented in configuration files.
2. Build and run your application in the exactly same context as your development environment.
3. Made your application platform independent so that you can deploy your application to either AWS or Azure or an on premise server.
4. Easier to scale up or scale down your services.
5. Don't have to install build tools and libraries on build agent. All dependant tools and frameworks will be installed inside docker container.

We will build three docker images for application development, build and release respectively.

The Dockerfile for develop image is as below:

```
FROM maven:3.5-jdk-8-alpine

WORKDIR /usr/src/spring-boot-docker-ci-example
COPY pom.xml .
RUN mvn -B -f pom.xml -s /usr/share/maven/ref/settings-docker.xml dependency:resolve

COPY . .

RUN mvn -B -s /usr/share/maven/ref/settings-docker.xml package -DskipTests
ENTRYPOINT ["java", "-jar", "/usr/src/spring-boot-docker-ci-example/target/spring-boot-docker-ci-example-0.0.1-SNAPSHOT.jar"]
```

The build tool of choice is maven and we choose to use alpine as the underlying linux os. In the development dockerfile, we first specify the work directory. Then, we copy the pom.xml and resolve java dependencies. Following step is to copy the source code to work directory and build and package the application. The final step is to tell docker the entry point of our application.

As you may have noticed, the development image has the build tool, all dependent packages as well as the java source code. Also, in this image we have the java jdk installed which is a lot larger than a java jre. With all these overhead, we can use this image as an environment to test and debug our application. IntelliJ has built in support to debug remotely in docker containers. The instructions to set it up can be easily found by googling.

To build the application for release purpose, we will make a build docker image. The dockerfile is as below:

```
FROM maven:3.5-jdk-8-alpine

WORKDIR /usr/src/spring-boot-docker-ci-example

COPY pom.xml .

RUN mvn -B -f pom.xml -s /usr/share/maven/ref/settings-docker.xml dependency:resolve

COPY . .
```

In this build dockerfile, we only have the first 4 steps of the development image dockerfile. We intentionally leave out the step to build and package the application, because we will need to copy the built package to local directory instead of leaving the built files inside the docker container. With the help of docker compose, we can easily achieve this.

The idea is to create a container of the build image and map the built target path to local directory. This way the built results will be made available to us for release purpose. The docker compose file is as below:

```
version: '2'

services:

springboot:

build:

context: .

dockerfile: Dockerfile.build

image: demo/springboot-build

volumes:

# map the output folder to local folder

- ./target:/usr/src/spring-boot-docker-ci-example/target

# The command to package the jar. This will start when the container boots up.

command: /bin/bash -c "mvn -B -s /usr/share/maven/ref/settings-docker.xml package -DskipTests"
```

In above docker compose file, we specified the build context, build dockerfile and image name. The important settings are volume mappings and the command to run after the docker container starts up which are commented above.

Having the release artifacts, the final step is to build the release docker image. The dockerfile to do this is as below:

```
FROM openjdk:8-jre-alpine

WORKDIR /usr/app

COPY ./target/spring-boot-docker-ci-example-0.0.1-SNAPSHOT.jar .

ENTRYPOINT ["java", "-jar", "/usr/app/spring-boot-docker-ci-example-0.0.1-SNAPSHOT.jar"]
```

In this docker image, we are going to use alpine with java jre. This will give us a much smaller docker container in size compared to the development one. We simply copy the release artifacts and specify the entry point to our java application. With the release docker image, we are ready to deploy it to the production server.