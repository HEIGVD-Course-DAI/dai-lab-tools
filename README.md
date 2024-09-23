DAI Lab 1: Tools
================

Goals
------------------------------------------------------------------------------

The goal of this lab is to learn the tools we will use during the course:
- Docker
  - Run an image with Docker
  - Map ports between the container and the host
  - Use the basic Docker commands such as `docker ps`, `docker exec`, `docker build`, `docker images`, `docker logs`, etc.
- Docker Compose
  - Create a `docker-compose.yml` file
  - Start and stop a Docker Compose infrastructure
  - Inspect a running Docker Compose infrastructure using commands such as `docker compose ps`, `docker compose top`, `docker compose logs`, etc.
- Dockerfile
  - Containerize an application using a Dockerfile
- Java and Maven
  - Modify a Maven project using its `pom.xml` file
  - Compile a Java project with Maven
  - Run a Java application from the command line


Preparation
------------------------------------------------------------------------------

You will need Git installed on your computer. If it is not yet installed, go to https://git-scm.com/downloads and follow the instructions for your operating system.

Then clone this repository on your computer with the command:

```bash
git clone <urL of the repository>
```


Docker
------------------------------------------------------------------------------

Docker and Docker Compose are two of the main tools that we will be using for many labs. Docker allows you to package software into containers that can be easily run without compatibility problems or missing libraries. We will use Docker Compose to create complete infrastructures with multiple servers.

### Installation

Docker is available for Windows, Mac and Linux. You can find the installation instructions for your operating system here: https://docs.docker.com/install/. This will install Docker Desktop. Docker Desktop is free for education and personal use, so you can use it for this course.

Docker Desktop includes *Docker Compose*, so you don't need to install it separately.

### Using Docker

The [Docker documentation](https://docs.docker.com/get-started/hands-on-overview/) is very good, so you should read it. Here we will give you a quick introduction to the most important commands. For a complete overview of Docker, you will find more information [here](https://docs.docker.com/get-started/).

#### Run an image

We've provided a custom docker image `heigvddai/chucknorris` that you can use for your tests. It is publicly available on [Docker hub](https://hub.docker.com/). 

**Go to Docker hub and search for this image.**

Using this image, **you must read** the section "Working with containers" of the document "The Top 10 Docker commands" on Cyberlearn. You should:
- run the image `heigvddai/chucknorris`,
- run the image as background process,
- run the image, which uses TCP port 80 and map it to another port, for example 8080 (you can then open a browser and connect to `localhost:8080` to see a Chuck Norris joke and refresh the page several times),
- open a shell in the running container,
- list the running containers with `docker ps`,
- stop the running container,
- remove the downloaded image.

**Write down the commands you used for each of these steps.**

#### Port mapping

Of the examples above, the port mapping merits some more information.

A docker image may contain a server which uses a specific port (e.g., port 80). However, when you run the image, you may want the container to use a different port, to avoid conflicts. This is where the syntax `-p <host port>:<container port>` comes in. If you want to run the chucknorris image on port 3000, use the option `-p 3000:80`. 

Try different ports and connect to the container using your browser with the URL `http://localhost:<port>`.


Docker Compose
------------------------------------------------------------------------------

While running a single container is nice, Docker shows it's real power when you use Docker Compose to deploy complete infrastructures such as a Web server with a database and a cache.

It should already be installed with Docker Desktop. Just type `docker compose version` in a terminal to check.

### Using Docker Compose

Create a `docker-compose.yml` file with the following content:

```yaml
services:

  chucknorris:
    image: heigvddai/chucknorris
    ports:
      - "8080:80"

  web:
    image: nginx
    ports:
      - "80:80"
```

In the same directory, run the command:

```bash
docker compose up -d
```

to start the infrastructure. The option `-d` tells Docker Compose to run the containers in the background.

Check that the containers are running with the command `docker compose ps`. You should see two containers, one for the chucknorris image and one for the nginx image.

Then go to your browser and open http://localhost to access the nginx server. Open http://localhost:8080 to access the chucknorris server. It should show a Chuck Norris joke on each refresh.

Shut down the infrastructure with the command 

```bash
docker compose down
```

Java
------------------------------------------------------------------------------

### Java SDK installation

Throughout the course, we will use **Java 21**, which came out on September 19, 2023.

The easiest way to install Java on your laptop (Windows or Mac) is to use [SDKMAN](https://sdkman.io/). Follow the instructions on the website to install sdkman. Then use it install **Java 21**.

For Windows, you will need a Unix shell such as WSL2 or Git Bash.

After the installation, check if it works in a *new* terminal:

```bash
java --version
> java 21.0...
```

#### IDE

As IDE, you can use:
* IntelliJ IDEA Community Edition, which is free. Download it from https://www.jetbrains.com/idea/download/ and install it.
* VS Code, which is also free. Download it from https://code.visualstudio.com/download and install it. Then install the Java Extension Pack from https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack.

#### The demo app

This repository contains a simple Java project under the directory `./java/demo-app`. Open it in your IDE and try to run it. You should see "Hello world!" in the console output.


Maven
------------------------------------------------------------------------------

Maven is a build tool for Java. It is used to compile, test and package Java projects. It also manages dependencies between libraries.

You will need Maven since the code you will develop in the following labs will be containerized with Docker. You therefore need to provide a `.jar` file that contain the compiled code of your application and that can be run in a Docker container.

To install Maven, follow the instructions on https://maven.apache.org/install.html. You can also install it with your package manager (e.g., `sudo apt install maven` on Ubuntu).

Check if it works in a terminal:

```bash
mvn -v
> Apache Maven 3.6.3...
```

### Maven configuration

To understand the basics of Maven, read the [Maven in 5 minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) tutorial before moving on.

Maven uses a configuration file called `pom.xml` to manage the project. It contains the project name, version, dependencies, etc. The demo-app already contains an almost minimal `pom.xml` file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>ch.heig.dai.lab.tools</groupId>
    <artifactId>demo-app</artifactId>
    <version>1.0</version>

    <properties>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <build>
    </build>

    <dependencies>
    </dependencies>

</project>
```

Besides the standard template, there are only three important elements:
- `<groupId>`: the groupId gives the package name. This is usually the name of the organization that created the project. The source code must follow this structure. In our case, the source code is in the directory `.../src/main/java/ch/heig/dai/lab/tools/`, which corresponds to the groupId `ch.heig.dai.lab.tools`. In the `Main.java` file you will see the package declaration `package ch.heig.dai.lab.tools;`.
- `<artifactId>` and `<version`: together they define the name of the `.jar` file that will be generated. In our case, the `.jar` file will be named `demo-app-1.0.jar`.

Other elements to check are `maven.compiler.source` and `maven.compiler.target`. Here we use Java 21 for the compiler and the generated `.jar` file.

### Compiling with Maven

As described in the [Maven in 5 minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) tutorial, you can compile the project with the command:

```bash
mvn clean package
```

in the directory with the `pom.xml` file. This will create a `target` directory with the compiled `.class` files and the `.jar` file.


### Running a JAR file on the command line

An executable `.jar` file can be run with the command:

```bash
java -jar <jar file>
```

Try it with the `.jar` file generated by Maven. 

Unfortunately, java will complain with an error message: `no main manifest attribute, in demo-app-1.0.jar`. This is because the `.jar` file does not contain the information about the main class to run. To fix this, we need to add the `maven-jar-plugin` to the `pom.xml` file, inside the empty `<build>` element:

```xml
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.3.0</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>ch.heig.dai.lab.tools.Main</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

After this change, compile and run the jar file again with the commands:

```bash
mvn clean package
java -jar target/demo-app-1.0.jar
```

It should work now.


Creating a dockerized Java application
------------------------------------------------------------------------------

Now that we can compile the Java application, we would like to containerize it, that is run it in a Docker container.

### Modify the Java application

We need an application that runs continuously. Otherwise the container will stop once the application stops.

Modify the `Main.java` file of the demo app to print "Hello world!" in an infinite loop.

You can use the following code to sleep for 1 second between the iterations:

```java
...
try {
    Thread.sleep(1000);
} catch (InterruptedException e) { }
...
```

### Package the application as Docker image

Docker uses a file called `Dockerfile` to build the image. Create a `Dockerfile` in the directory `./demo-app` with the following content:

```dockerfile
FROM alpine:latest
RUN apk add --no-cache openjdk21
WORKDIR /app
COPY target/*.jar /app/app.jar
CMD ["java", "-jar", "app.jar"]
```

**Read the [Dockerfile documentation](https://docs.docker.com/build/building/packaging/) to understand each of the lines.**

To build the image, run the command:

```bash
 docker build -t dai/demo-app .
 ```
in the directory `./demo-app`. Be careful to not forget the dot at the end of the command. It is important. It tells Docker to use the current directory (`./`) as the build context. That means the command will be run in this directory.

The option `-t` gives a name to the Docker image. If the build was successful, you can check that the image is present with the command `docker images`.

Now try to run the image with the normal `docker run` command that you already know. It should work and print "Hello world!" in an infinite loop. Quit with Ctrl-C.

### Using Docker Compose

In a final step, we will use Docker Compose to run the application. Create a `compose.yml` file with the image as the only service. Run it with `docker compose` to check that it works.
