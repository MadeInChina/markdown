![Docker logo](https://raw.githubusercontent.com/MadeInChina/markdown/master/docker-presentation/introduction-1/img/docker_logo.png)

## Docker for Developers - introduction

#### [@hanrenwei](http://twitter.com/hanrenwei)
________________________

###### Get them: [online presentation](https://github.com/MadeInChina/markdown/tree/master/docker-presentation/introduction-1) / [source code](https://github.com/MadeInChina/markdown/tree/master/docker-presentation/introduction-1) / [docker image](https://hub.docker.com/)

###### Under [Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0/) license.

---

### What is Docker (17.03.0-ce)

> Docker is an open platform for developing, shipping, and running applications.

> Docker allows you to package an application with all of its dependencies into a standardized unit for software development.

---

### Docker Benefits

 - Fast (deployment, migration, restarts)
 - Secure
 - Lightweight (save disk & CPU)
 - Open Source
 - Portable software
 - Microservices and integrations (APIs)
 - Simplify DevOps
 - Version control capabilities

---

###### See the [survey results for 2016](https://www.docker.com/survey-2016)
 - Docker provides the software supply chain with agility, control and portability for app development.

![Docker_Supply-chain-V1.5-01.png](https://raw.githubusercontent.com/MadeInChina/markdown/master/docker-presentation/introduction-1/img/Docker_Supply-chain-V1.5-01.png)

---

 - Docker is delivering quantifiable improvements to application delivery through changing DevOps practices.
 
![Docker_Survey_DevOps_Alt2-V1.8-01-01.png](https://raw.githubusercontent.com/MadeInChina/markdown/master/docker-presentation/introduction-1/img/Docker_Survey_DevOps_Alt2-V1.8-01-01.png)

---

### Docker client

It is the primary user interface to Docker. It accepts commands from the user
and communicates back and forth with a Docker daemon.

---

### Docker compose

![Docker compose logo](https://raw.githubusercontent.com/madeinchina/markdown/master/docker-presentation/introduction-1/img/docker_compose.png)

A tool for defining and running complex applications with Docker
(eg a multi-container application) with a single file.

---

### Steps of a Docker workflow

```
docker run -i -t -d -p 9080:8080 docker.finnplay.net/tomcat:8.5-jre8-slim
```

 - Pulls the tomcat:8.5-jre8-slim [image](https://docs.docker.com/engine/userguide/containers/dockerimages/ "A read-only layer that is the base of your container. It can have a parent image to abstract away the more basic filesystem snapshot.") from the [registry](https://docs.docker.com/registry/ "The central place where all publicly published images live. You can search it, upload your images there and when you pull a docker image, it comes the repository/hub.")
 - Creates a new [container](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/ "A runnable instance of the image, basically it is a process isolated by docker that runs on top of the filesystem that an image provides.")
 - Allocates a filesystem and mounts a read-write [layer](https://docs.docker.com/engine/reference/glossary/#filesystem "A set of read-only files to provision the system. Think of a layer as a read only snapshot of the filesystem.")
 - Allocates a [network/bridge interface](https://www.wikiwand.com/en/Bridging_%28networking%29 "")
 - Sets up an [IP address](https://www.wikiwand.com/en/IP_address "An Internet Protocol address (IP address) is a numerical label assigned to each device (e.g., computer, printer) participating in a computer network that uses the Internet Protocol for communication.")
 - Executes a process that you specify

---

### The Dockerfile

> A Dockerfile is a text document that contains all the commands a user could call on the command line to create an image.

 - [Dockerfile](https://github.com/madeinchina/markdown/master/docker-presentation/introduction-1/examples/dockerfile/Dockerfile)
 - [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) on docker docs

---

### Common Docker Commands

```
// General info
docker help // docker help run
docker info
docker version
docker network ls

// Images
docker images // docker [IMAGE_NAME]
docker pull [IMAGE] // docker push [IMAGE]

// Containers
docker run
docker ps // docker ps -a, docker ps -l
docker stop/start/restart [CONTAINER]
docker stats [CONTAINER]
docker top [CONTAINER]
docker port [CONTAINER]
docker inspect [CONTAINER]
docker inspect -f "{{ .State.StartedAt }}" [CONTAINER]
docker rm [CONTAINER]

```

---

### Docker examples
- SSH into a container
- Build an image
- Docker [Volume](https://docs.docker.com/engine/userguide/containers/dockervolumes/)
- Using [docker-compose](https://docs.docker.com/compose/)

---

### Example: SSH into a container

```
docker pull docker.finnplay.net/tomcat:8.5-jre8-slim
docker run -i -t -d -p 9080:8080 docker.finnplay.net/tomcat:8.5-jre8-slim
docker ps -a
docker exec -it [container_id] /bin/bash
```

---

### Example: Build an Image

Let's build a [tomcat image](https://github.com/MadeInChina/markdown/blob/master/docker-presentation/introduction-1/examples/dockerfile/Dockerfile)

```
cd ~/docker-presentation
git clone https://github.com/MadeInChina/markdown.git
cd markdown/docker-presentation/introduction-1/examples/dockerfile
docker build -t tomcat:8.5-jre8-slim .

// Test it
docker run -d -p 8099:8080 --name tomcat tomcat:8.5-jre8-slim
// Open http://localhost:8099
```

---

### Example: Docker volume

Let's use [tomcat image](https://github.com/MadeInChina/markdown/blob/master/docker-presentation/introduction-1/examples/dockerfile/Dockerfile)

```
cd ~/docker-presentation
mkdir tomcat-example
cd tomcat-example

docker run --name tomcat \
           -p 8099:8080 \
           -v $(pwd):/usr/local/tomcat/webapps/demo \
           -d tomcat:8.5-jre8-slim

// Locally create an index.html file
echo "It works using mount." >> index.html

// Open http://localhost:8099/demo/index.html to view the html file
```

---

### Example: Using Docker Compose

Let's create a tomcat and mysql app with [docker-compose.yml](https://github.com/MadeInChina/markdown/blob/master/docker-presentation/introduction-1/examples/docker-compose/docker-compose.yml)

```
cd ~/docker-presentation
git clone https://github.com/MadeInChina/markdown.git
cd markdown/docker-presentation/introduction-1/examples/docker-compose

// Run docker-compose using the docker-compose.yml
cat docker-compose.yml
docker-compose up -d
```

---

### Docker tips

There are known best practices (see a list at [examples/tips](https://github.com/MadeInChina/markdown/blob/master/docker-presentation/introduction-1/examples/tips))

- Optimize containers (check [fromlatest.io](https://www.fromlatest.io/) and [imagelayers.io](https://imagelayers.io))
- Create your own tiny base
- Containers are not Virtual Machines
- Full stack Images VS 1 process per Container
- Create your private registry
- Create shortcut commands
- Use docker-compose.yml templates
- Be aware of the hub.docker.com docker agent version
---
### Continuous Delivery: Jenkins pipeline
![CI/CD](https://raw.githubusercontent.com/MadeInChina/markdown/master/docker-presentation/introduction-1/img/ci-cd.png)
---
### Demo
![CD](https://raw.githubusercontent.com/MadeInChina/markdown/master/docker-presentation/introduction-1/img/cd-process.png)


---

### Questions?


###### In this presentation I have used [oh my zsh](http://ohmyz.sh/), [docker 17.03.0-ce](https://github.com/docker/docker/releases/tag/v17.03.0-ce), [wharfee](https://github.com/j-bennet/wharfee) and [dry](https://github.com/moncho/dry).
---
