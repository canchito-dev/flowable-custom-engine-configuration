# custom-engine-configuration

In this tutorial, we will go into detail on how to customize Flowable's engine.

## License
The MIT License (MIT)  

Copyright (c) 2020, canchito-dev  

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:  

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.  

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Contribute Code
If you would like to become an active contributor to this project please follow these simple steps:

1.  Fork it
2.  Create your feature branch
3.  Commit your changes
4.  Push to the branch
5.  Create new Pull Request

## What you’ll need
*   About 30 minutes
*   A favorite IDE. In this post, we use [Intellij Community](https://www.jetbrains.com/idea/download/index.html)
*   [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or later. It can be made to work with JDK6, but it will need configuration tweaks. Please check the Spring Boot documentation

## Starting with Spring Initializr

For all Spring applications, it is always a good idea to start with the [Spring Initializr](https://start.spring.io/). The Initializr is an excellent option for pulling in all the dependencies you need for an application and does a lot of the setup for you. This example needs only the Spring Web, and H2 Database dependency. The following image shows the Initializr set up for this sample project:

![Spring Initializr](images/initializr.png)

The following listing shows the `pom.xml` file that is created when you choose Maven:

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>  
	<parent> <groupId>org.springframework.boot</groupId>  
		<artifactId>spring-boot-starter-parent</artifactId>  
		<version>2.2.7.RELEASE</version>  
		<relativePath/> <!-- lookup parent from repository -->  
	</parent>  
	<groupId>com.canchitodev.example</groupId>  
	<artifactId>spring-flowable-integration</artifactId>  
	<version>0.0.1-SNAPSHOT</version>  
	<packaging>war</packaging>  
  
	<name>spring-flowable-integration</name>  
	<description>Demo project showing how to customize Flowable's engine</description>  
  
	<organization>
        <name>Canchito Development</name>  
		<url>http://www.canchito-dev.com</url>  
	</organization>  
	<issueManagement>
    	<system>Canchito Development</system>
    	<url>https://github.com/canchito-dev/custom-engine-configuration/issues</url>
    </issueManagement>
    
    <url>http://www.canchito-dev.com/public/blog/2020/05/14/customizing-flowable-engine/</url>
	
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>  
		<java.version>1.8</java.version>  
		<flowable.version>6.5.0</flowable.version>
		<junit.version>4.13</junit.version>
		<junit.jupiter.version>5.3.2</junit.jupiter.version>
		<junit.vintage.version>5.3.2</junit.vintage.version>
	</properties>  
 
	<dependencies>
		<!-- Starter for building web, including RESTful, applications using Spring MVC. Uses Tomcat as the default embedded container -->  
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<!-- Starter for building web, including RESTful, applications using Spring MVC. Uses Tomcat as the default embedded container -->  
		
		<!-- H2 Database Engine -->
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
		</dependency>
		<!-- H2 Database Engine -->
		
		<!-- Starter for using Tomcat as the embedded servlet container. Default servlet container starter used by spring-boot-starter-web -->
		<dependency>  
			<groupId>org.springframework.boot</groupId>  
			<artifactId>spring-boot-starter-tomcat</artifactId>  
		</dependency>  
		<!-- Starter for using Tomcat as the embedded servlet container. Default servlet container starter used by spring-boot-starter-web -->  

		<!-- Starter for testing Spring Boot applications with libraries including JUnit, Hamcrest and Mockito -->  
		<dependency>  
			<groupId>org.springframework.boot</groupId>  
			<artifactId>spring-boot-starter-test</artifactId>  
			<scope>test</scope>  
			<exclusions> 
				<exclusion> 
					<groupId>org.junit.vintage</groupId>  
					<artifactId>junit-vintage-engine</artifactId>  
				</exclusion> 
			</exclusions> 
		</dependency>  
		<!-- Starter for testing Spring Boot applications with libraries including JUnit, Hamcrest and Mockito -->  

		<dependency>
			<groupId>org.junit.jupiter</groupId>
			<artifactId>junit-jupiter-api</artifactId>
			<version>${junit.jupiter.version}</version>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>${junit.version}</version>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.junit.jupiter</groupId>
			<artifactId>junit-jupiter-engine</artifactId>
			<version>${junit.jupiter.version}</version>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.junit.vintage</groupId>
			<artifactId>junit-vintage-engine</artifactId>
			<version>${junit.vintage.version}</version>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.awaitility</groupId>
			<artifactId>awaitility</artifactId>
		</dependency>

	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>

			<plugin>
				<artifactId>maven-surefire-plugin</artifactId>
				<version>2.22.2</version>
			</plugin>

			<plugin>
				<artifactId>maven-failsafe-plugin</artifactId>
				<version>2.22.2</version>
			</plugin>
		</plugins>
	</build>
</project>
```

## Adding Flowable's Dependencies

Now, simply add _flowable-spring-boot-starter_ dependency. This will add all the engines.

```xml
<!-- Flowable Spring Boot Starter Basic -->
<dependency>
	<groupId>org.flowable</groupId>
	<artifactId>flowable-spring-boot-starter-basic</artifactId>
	<version>${flowable.version}</version>
</dependency>
<!-- Flowable Spring Boot Starter Basic -->
```