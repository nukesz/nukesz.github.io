---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Dockerize a Spring Boot application"
subtitle: ""
summary: ""
authors: []
tags: ["spring-boot", "docker", "java"]
categories: []
date: 2025-02-10T20:53:53+01:00
lastmod: 2025-02-10T20:53:53+01:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## Introduction

In this post, I'd like to present a few options to ship a spring boot application in a docker container. There are many easy ways to *dockerize a spring boot* (probably a nice google hit search), but I don't see too much discussion around the pros and cons. So let's jump into it

## Create new project

Just go to https://start.spring.io/ and create a new project. I'll be using:

- Gradle - Groovy
- Spring Boot 3.4.2
- Java 21
- Dependencies: Spring Web

For demonstration, I'm going to add the "/ping" endpoint and it's going to return "pong". Just simply create `PingController.java`.


```java
package com.nukesz.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class PingController {

    @GetMapping("/ping")
    public String ping() {
        return "Pong";
    }
}
```

Build and run the application as a normal **jar**:

```sh
# Build it
./gradlew build
# Run it
java -jar build/libs/dockerize-spring-boot-0.0.1-SNAPSHOT.jar
```

> The jar is actually created with the `bootJar` task. You can learn more about it in the [spring doc](https://docs.spring.io/spring-boot/docs/2.5.1/gradle-plugin/reference/htmlsingle/#packaging-executable.and-plain-archives)

Verify our REST API is working as expected:

```sh
curl http://localhost:8080/ping
> Pong!
```

## Create `Dockerfile` manually

Our application is ready, so let's create a docker image for it. First let's

```sh
FROM eclipse-temurin:21
LABEL org.opencontainers.image.authors="Norbert Benczur"
RUN mkdir /opt/app
COPY build/libs/dockerize-spring-boot-*.jar /opt/app/myapp.jar
CMD ["java", "-jar", "/opt/app/myapp.jar"]
```

You can build and run the Docker image:

```sh
docker build -t dockerize-spring-boot .
docker run -it -p 8080:8080 --rm $ dockerize-spring-boot
```

Verify that we can reach our REST API within the container as expected:

```sh
$ curl http://localhost:8080/ping
> Pong!
```

Are we done? - Not at all.

### What's the problem?

Creating `Dockerfile` manually has its pros and cons. It's the most flexible solution where you control everything. No dependency needed.

The problem comes when you need more than a `Hello World` example.

#### Repetitive

When you have more than 1 java app to dockerize, the number of dockerfiles starts to grow and you have to maintain and update each file independently.

#### Efficiency

In this simple example, we defined our base image and started our *fat jar*. But is that the most optimal way to build and run a spring boot (or any other java) application?
For example let's change a single file in our application and build the image again:

```sh
# Let's measure the re-build
$ time  ( ./gradlew build -x test; docker build -t dockerize-spring-boot .)
> ..
> => [3/3] COPY build/libs/dockerize-spring-boot-*.jar /opt/app/myapp.jar
> ..
> real 0m7,640s
```

So even a single change could cause the jar to be re-built and copied again. We are obviously not using the benefits of docker layers.

Can't we leverage other people's work rather than trying to come up with most optimal `Dockerfile` ourself?

## Use Buildpack

TBD