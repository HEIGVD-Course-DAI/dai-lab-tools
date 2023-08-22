DAI Lab 1: Tools
================

Goals
-----

The goal of this lab is to learn the tools we will use during the course:
- Docker and Docker Compose
- Git
- Java
- Maven



•	Docker
    o	Utiliser docker run avec une image fournie
    o	Comprendre l’utilisation des ports
    o	Créer un docker-compose et l’utiliser
•	Java
    o	Compiler avec javac
    o	Exécuter un fichier jar avec java
•	Maven
    o	Créer un fichier .pom de base
    o	Ajouter une dépendance
    o	Compiler avec Maven
•	Git
    o	Maîtriser le workflow fork/pull request/pull




Preparation
-----------

You will need Git installed on your computer. If it is not yet installed, go to https://git-scm.com/downloads and follow the instructions for your operating system.

Then clone this repository on your computer with the command:

```bash
git clone <urL of the repository>
```




Docker
------

Docker and Docker Compose are two of the main tools that we will be using for many labs. Docker allows you to package software into containers that can be easily run without compatibility problems or missing libraries. We will use Docker Compose to create complete infrastructures with multiple servers.

### Installation

Docker is available for Windows, Mac and Linux. You can find the installation instructions for your operating system here: https://docs.docker.com/install/. This will install Docker Desktop. Docker Desktop is free for education and personal use, so you can use it for this course.

Docker Desktop includes *Docker Compose*, so you don't need to install it separately.

### Using Docker

The [Docker documentation](https://docs.docker.com/get-started/hands-on-overview/) is very good, so you should read it. Here we will give you a quick introduction to the most important commands. For a complete overview of Docker, you will find more information [here](https://docs.docker.com/get-started/).

#### Run an image

We've provided a custom docker image `heigvddai/chucknorris` that you can use for your tests. It is publicly available on [Docker hub](https://hub.docker.com/). 

**Go to Docker hub and search for this image.**

Using this image, work through the section "Working with containers" of the document "The Top 10 Docker commands" on Cyberlearn. You should:
- run the image `heigvddai/chucknorris`,
- run the image as background process,
- run the image, which uses TCP port 80 and map it to another port, for example 8080 (you can then open a browser and connect to `localhost:8080` to see as Chuck Norris joke and refresh the page several times),
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
--------------

While running a single container is nice, Docker shows it's real power when you use Docker Compose to deploy complete infrastructures such as a Web server with a database and a cache.

It should already be installed with Docker Desktop. Just type `docker compose version` in a terminal to check.

### Using Docker Compose

Create a `docker-compose.yml` file with the following content:

```yaml
version: '3.8'

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

