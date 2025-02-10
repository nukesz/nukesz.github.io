---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Dockerize Spring Boot"
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

# Why?

In this post, I'd like to present a few options to ship a spring boot application in a docker container. There are many easy ways to *dockerize a spring boot* (probably a nice google hit search), but I don't see too much discussion around the pros and cons. So let's jump into it

## Create new project

Just hit https://start.spring.io/ and create a new project. I'll be using:

- Gradle - Groovy
- Spring Boot 3.4.2
- Java 21
- Dependencies: Spring Web

Let's add a new `@RestController` and create an endpoint to test:

```java
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MessageController {
    @GetMapping("/ping")
    public String getMessage() {
        return "Pong!";
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

## Create `dockerfile` manually


