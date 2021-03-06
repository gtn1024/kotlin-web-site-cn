---
type: tutorial
layout: tutorial
title: "用 Spring Boot 创建 RESTful Web 服务"
description: "本教程将引导完成使用 Spring Boot 创建简单的应用程序的过程。"
---

You will create an application with the HTTP endpoint that returns a data objects list in the JSON format.

This tutorial consists of two parts:
* Create a RESTful Web Service with Spring Boot
* [Add a database to a Spring Boot RESTful web service](spring-boot-restful-db.html)

To get started, first download and install the latest version of [IntelliJ IDEA](http://www.jetbrains.com/idea/download/index.html).

## Bootstrap the project

To generate a new project, use the Spring Initializr:

> You can also create a new project using [IntelliJ IDEA with the Spring Boot plugin](https://www.jetbrains.com/help/idea/spring-boot.html).
{:.note}

1. Open the [Spring Initializr](https://start.spring.io/#!type=gradle-project&language=kotlin&platformVersion=2.4.2.RELEASE&packaging=jar&jvmVersion=11&groupId=com.example&artifactId=demo&name=demo&description=Demo%20project%20for%20Spring%20Boot&packageName=demo&dependencies=web,data-jdbc,h2). The link from the tutorial opens the window with the predefined settings of the new project. 
  This project uses **Gradle** as a build tool, **Kotlin** as a language of choice, and the following dependencies: **Spring Web**, **Spring Data JDBC**, and **H2 Database**:

   ![Create a new project with Spring Initializr]({{ url_for('tutorial_img', filename='spring-boot-restful/spring-boot-create-project-with-initializr.png') }})

2. Click **GENERATE** at the bottom of the screen. Spring Initializr will generate the project with the specified settings. The download starts automatically.

3. Unpack the **.zip** file and open in the IntelliJ IDEA.
  
   The project has the following structure: 
   ![The Spring Boot project structure]({{ url_for('tutorial_img', filename='spring-boot-restful/spring-boot-project-structure.png') }})
   
   There are packages and classes under the `main/kotlin` folder that belong to the application. The entry point to the application is the `main()` method of the `DemoApplication.kt` file.

## Explore the project build file

Open the `build.gradle.kts` file.

This is the Gradle Kotlin build script, which contains a list of the dependencies required for the application. 

The Gradle file is standard for Spring Boot, but also contains necessary Kotlin dependencies, including [kotlin-spring](../reference/compiler-plugins.html#spring-support) Gradle plugin.

## Explore the Spring Boot application

Open the `DemoApplication.kt` file:

<div class="sample" markdown="1" theme="idea" mode="kotlin" data-highlight-only>

```kotlin
package demo

import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication

@SpringBootApplication
class DemoApplication

fun main(args: Array<String>) {
   runApplication<DemoApplication>(*args)
}
```

</div>

Comparing to Java, the application file has the following differences:
* As Spring Boot looks for a public static `main()` method, the Kotlin uses a [top-level function](../reference/functions.html) defined outside the `DemoApplication` class.
* The `DemoApplication` class is not declared as `open`, since the [kotlin-spring](../reference/compiler-plugins.html#spring-support) Gradle plugin does that automatically.

## Create a data class and a controller

To create an endpoint, add a [data class](../reference/data-classes.html) and a controller:

1. In the `DemoApplication.kt` file, create a `Message` data class with two properties: `id` and `text`:

   <div class="sample" markdown="1" theme="idea" mode="kotlin" data-highlight-only>

   ```kotlin
   data class Message(val id: String?, val text: String)
   ```
   
   </div>

2. In the same file, create a `MessageResource` class which will serve the requests and return a JSON document representing a collection of `Message` objects:

   <div class="sample" markdown="1" theme="idea" mode="kotlin" data-highlight-only>
   
   ```kotlin
   @RestController
   class MessageResource {
       @GetMapping
       fun index(): List<Message> = listOf(
           Message("1", "Hello!"),
           Message("2", "Bonjour!"),
           Message("3", "Privet!"),
      )
   }
   ```

   </div>

Full code of the `DemoApplication.kt`:

<div class="sample" markdown="1" theme="idea" mode="kotlin" data-highlight-only>

```kotlin
package demo

import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication
import org.springframework.data.annotation.Id
import org.springframework.web.bind.annotation.GetMapping
import org.springframework.web.bind.annotation.RestController

@SpringBootApplication
class DemoApplication

fun main(args: Array<String>) {
  runApplication<DemoApplication>(*args)
}

@RestController
class MessageResource {
  @GetMapping
  fun index(): List<Message> = listOf(
      Message("1", "Hello!"),
      Message("2", "Bonjour!"),
      Message("3", "Privet!"),
  )
}

data class Message(val id: String?, val text: String)
```

</div>

## Run the application

Application is ready to run:

1. Click the green **Run** icon in the gutter to the `main()` method or hit the **Alt+Enter** shortcut to invoke the launch menu in IntelliJ IDEA:

    ![Run the application]({{ url_for('tutorial_img', filename='spring-boot-restful/spring-boot-run-the-application.png') }})
    
    > You can also run the `./gradlew bootRun` command in the terminal.
    {:.note}

2. Once the application starts, open the following URL: [http://localhost:8080](http://localhost:8080). 

    You will see a page with a collection of messages in JSON format:

    ![Application output]({{ url_for('tutorial_img', filename='spring-boot-restful/spring-boot-output.png') }})

## Proceed to the next tutorial

Once you’ve created this application, add a database for storing objects and two endpoints to write and retrieve them using the next part of the tutorial – [Add a database to a Spring Boot RESTful web service](spring-boot-restful-db.html).