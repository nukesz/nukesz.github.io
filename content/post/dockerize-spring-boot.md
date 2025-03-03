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

In this post, I'd like to present a few options to ship a spring boot application in a docker container. There are many ways to *dockerize a spring boot* (probably a nice google hit search), but I don't see too much discussion around the pros and cons. So let's jump into it.

## Create new project

Go to https://start.spring.io/ and create a new project. I'll be using:

- Gradle - Groovy
- Spring Boot 3.4.2
- Java 21
- Dependencies: Spring Web

For demonstration, I'm going to add a "/ping" endpoint and it's going to return "pong". Just simply create `PingController.java`.

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
$ curl http://localhost:8080/ping
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
docker run -it -p 8080:8080 --rm dockerize-spring-boot
```

Verify that we can reach our REST API within the container as expected:

```sh
$ curl http://localhost:8080/ping
> Pong!
```

Are we done? - Not at all.

### What's the problem?

Creating `Dockerfile` manually has its pros and cons. It's the most flexible solution where you control everything. No dependency needed.

But I think the biggest drawback with this approach is that **it seems** everything is working, but in fact it is hiding the underlining work that is missing. The problem comes when you need more than a `Hello World` example.

#### Repetitive

When you have more than 1 java app to dockerize, the number of dockerfiles starts to grow and you have to maintain and update each file independently.

#### Efficiency

In this simple example, we defined our base image and started our *fat jar*. But is that the most optimal way to build and run a spring boot (or any other java) application?
For example, let's change a single file in our application and build the image again:

```sh
# Let's measure the re-build
$ time  ( ./gradlew build -x test; docker build -t dockerize-spring-boot . )
> ..
> => [3/3] COPY build/libs/dockerize-spring-boot-*.jar /opt/app/myapp.jar
> ..
> real 0m7,640s
```

So even a single change could cause the jar to be re-built and copied again. We are obviously not using the benefits of docker layers.

Can't we leverage other people's work rather than trying to come up with most optimal `Dockerfile` ourself?

## Use Buildpacks

Many of the problems that I mentioned previously comes from that the fact that we wrote our `Dockerfile` manually. So let's look another approach which does not need any Dockerfile at all.

[Buildpacks](https://buildpacks.io/) transform the source code into containers without Dockerfiles. There are multiple projects that implements the Cloud Native Buildpacks (CNBs) [spec](https://github.com/buildpacks/spec) and probably best two options for java application today is:
[Paketo](https://paketo.io/) and [Jib](https://github.com/GoogleContainerTools/jib).

### Paketo

We can use paketo by executing the following gradle (or maven) task:

```gradle
./gradlew bootBuildImage
```

The task will look and analyze your files, configurations and create a docker image based on it. The task's output is really user friendly and show all the parameters that it used for building the image.

```bash
> Task :bootBuildImage
Building image 'docker.io/library/dockerize-spring-boot:0.0.1-SNAPSHOT'

 > Pulling builder image 'docker.io/paketobuildpacks/builder-jammy-java-tiny:latest'
 > Pulled builder image 'paketobuildpacks/builder-jammy-java-tiny@sha256:c5c53'
 > Pulling run image 'docker.io/paketobuildpacks/run-jammy-tiny:latest' for platform 'linux/amd64'
 > Pulled run image 'paketobuildpacks/run-jammy-tiny@sha256:0c5ac'
 > Executing lifecycle version v0.20.6
 > Using build cache volume 'pack-cache-7b0feb92a365.build'

 > Running creator
    [creator]     ===> ANALYZING
    [creator]     Image with name "docker.io/library/dockerize-spring-boot:0.0.1-SNAPSHOT" not found
    [creator]     ===> DETECTING
    [creator]       ...
    [creator]     ===> RESTORING
    [creator]     ===> BUILDING
    [creator]       ...
    [creator]     Paketo Buildpack for Spring Boot 5.32.1
    [creator]       https://github.com/paketo-buildpacks/spring-boot
    [creator]       Build Configuration:
    [creator]         $BPL_JVM_CDS_ENABLED                 false  whether to enable CDS optimizations at runtime
    [creator]         $BPL_SPRING_AOT_ENABLED              false  whether to enable Spring AOT at runtime
    [creator]         $BP_JVM_CDS_ENABLED                  false  whether to enable CDS & perform JVM training run
    [creator]         $BP_SPRING_AOT_ENABLED               false  whether to enable Spring AOT
    [creator]         $BP_SPRING_CLOUD_BINDINGS_DISABLED   false  whether to contribute Spring Boot cloud bindings support
    [creator]         $BP_SPRING_CLOUD_BINDINGS_VERSION    1      default version of Spring Cloud Bindings library to contribute
    [creator]       Launch Configuration:
    [creator]         $BPL_SPRING_CLOUD_BINDINGS_DISABLED  false  whether to auto-configure Spring Boot environment properties from bindings
    [creator]         $BPL_SPRING_CLOUD_BINDINGS_ENABLED   true   Deprecated - whether to auto-configure Spring Boot environment properties from bindings
    [creator]       Creating slices from layers index
    [creator]         dependencies (19.5 MB)
    [creator]         spring-boot-loader (458.8 KB)
    [creator]         snapshot-dependencies (0.0 B)
    [creator]         application (35.9 KB)
    [creator]       Spring Cloud Bindings 2.0.4: Contributing to layer
    [creator]         Downloading from https://repo1.maven.org/maven2/org/springframework/cloud/spring-cloud-bindings/2.0.4/spring-cloud-bindings-2.0.4.jar
    [creator]         Verifying checksum
    [creator]         Copying to /layers/paketo-buildpacks_spring-boot/spring-cloud-bindings
    [creator]       Web Application Type: Contributing to layer
    [creator]         Servlet web application detected
    [creator]         Writing env.launch/BPL_JVM_THREAD_COUNT.default
    [creator]       Launch Helper: Contributing to layer
    [creator]         Creating /layers/paketo-buildpacks_spring-boot/helper/exec.d/spring-cloud-bindings
    [creator]       4 application slices
    [creator]       Image labels:
    [creator]         org.opencontainers.image.title
    [creator]         org.opencontainers.image.version
    [creator]         org.springframework.boot.version
    [creator]     ===> EXPORTING
    [creator]     Adding layer 'paketo-buildpacks/...
    [creator]         ...

Successfully built image 'docker.io/library/dockerize-spring-boot:0.0.1-SNAPSHOT'

BUILD SUCCESSFUL in 1m 57s
```

Let's run and test our the newly created images as before:

```sh
$ docker run -it -p 8080:8080 --rm dockerize-spring-boot:0.0.1-SNAPSHOT
$ curl http://localhost:8080/ping
> Pong!
```

This already gives us a solid ground to build upon for production and uses all the best practices almost for free. For example we already could see two benefits:

- The size of the created docker image from the "manual" `Dockerfile` is **468MB** while the image created by paketo with default settings is **265MB**.
- The docker layers are cached. When only the java source code is changed, we can see the following message: *Reused 4/5 app layer(s)*. So we (Paketo) can optimize our build and rebuild only what's actually necesseary.

For more information please have a look on the Spring doc [Packaging OCI Images](https://docs.spring.io/spring-boot/gradle-plugin/packaging-oci-image.html).

### Jib

*Coming soon...*