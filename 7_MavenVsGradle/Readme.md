## Lab : Gradle Lab

# Purpose of this lab
-  Develop SpringBootApplication with Gradle as build tool.
-  Analyse build.gradle
-  Understand Difference between pom.xml and build.gradle


### 1. Check Your Gradle version

~~~
gradle -version
~~~

You should see something like this.
~~~
------------------------------------------------------------
Gradle 5.1
------------------------------------------------------------

Build time:   2019-01-02 18:57:47 UTC
Revision:     d09c2e354576ac41078c322815cc6db2b66d976e

Kotlin DSL:   1.1.0
Kotlin:       1.3.11
Groovy:       2.5.4
Ant:          Apache Ant(TM) version 1.9.13 compiled on July 10 2018
JVM:          1.8.0_191 (Oracle Corporation 25.191-b12)
OS:           Windows 10 10.0 amd64
~~~

### 2. Create Spring Boot App using Gradle

#### Create a Spring Boot Web Application using the Spring Initializr
-  Open a browser and hit the url: http://start.spring.io
-  Fill out the Project metadata with:
-  Group: io.pivotal.workshop
-  Artifact: spring-boot-web-gradle
-  Type Web, REST in the Dependencies field and press Enter.
-  Select Gradle at the top instead of maven.
-  Click the Generate Project button.

-  You will get a spring-boot-web.zip file. Uncompress the file and take a look at the layout.

#### Important folders and files to notice
-  build.gradle
-  settings.gradle
-  src

### 3. Inspect the build.gradle file to see whats included and also compare with pom.xml you have from last lab.

### 4. Lets make some code changes for fun.

-  Modify the SpringBootWebGradleApplication.javaa class to create a Web application.
~~~
package io.pivotal.workshop;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@SpringBootApplication
public class SpringBootWebGradleApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringBootWebGradleApplication.class, args);
	}

	@RequestMapping("/")
	public String greetings(){
		return "Hello: Spring Boot with Gradle!";
	}
}
~~~
### 5. Save your changes

### 6. Package your app
~~~~
gradle build
~~~~

#### 7. Run your app locally
~~~
$ gradle bootrun
~~~

#### 8. Verify your app
-  Navigate to http://localhost:8080 and you will see "Hello: Spring Boot with Gradle!".

#### 9. Create a deployment manifest file
-  Go to Spring Boot App root directory.
-  In your preferred IDE:
  -  Create a new file  with following details.
~~~~
 applications:
- name: spring-boot-web-gradle
  instances: 1
  path: build/lib/spring-boot-web-gradle-0.0.1.war
  random-route: false
  routes:
  - route: spring-boot-webGradleTraining.cfapps.io
~~~~
-  Save file as
  -  manifest.yml
  -  make sure file saves at project root directory.

#### 10. Login into PCF
-  In Powershell/command Prompt
  -  Login to PCF Web Service
  ~~~
  cf login -a api.run.pivotal.io
  ~~~

-  Follow the prompt entering userid and password for PCF.

### 11. Push app into PCF
-  Make sure you are in project root directory and you have manifest file too in there.
-  Use powershell or command prompt.

~~~
cf push
~~~

#### 12. Validate your App running in PCF.
-  Navigate to https://spring-boot-webGradleTraining.cfapps.io/ and you will see "Hello: Spring Boot with Gradle!".
