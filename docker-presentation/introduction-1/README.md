![Docker logo](https://raw.githubusercontent.com/madeinchina/docker-presentation/gh-pages/img/docker_logo.png)

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
- [Linked](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/) containers
- Using [docker-compose](https://docs.docker.com/compose/)
- Scale containers with docker-compose
- Share an image (share this presentation)
- Package an app with its environment
- Screen and sound within containers (x-forward)

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

### Example: Export/Save/Load etc

```
docker pull nimmis/alpine-apache
docker run -d --name apache_example \
           nimmis/alpine-apache

// Create a file inside the container.
// See https://github.com/nimmis/docker-alpine-apache for details.
docker exec -ti apache_example \
            /bin/sh -c 'mkdir /test && echo "This is it." >> /test/test.txt'

// Test it. You should see message: "This is it."
docker exec apache_example cat /test/test.txt

// Commit the change.
docker commit apache_export_example myapache:latest

// Create a new container with the new image.
docker run -d --name myapache_example myapache

// You should see the new folder/file inside the myapache_example container.
docker exec myapache_example cat /test/test.txt

// Export the container as image
cd ~/Docker-presentation
docker export myapache_example > myapache_example.tar

// Import a new image from the exported files
cd ~/Docker-presentation
docker import myapache_example.tar myapache:new

// Save a new image as tar
docker save -o ~/Docker-presentation/myapache_image.tar myapache:new

// Load an image from tar file
docker load < myapache_image.tar

```

---

### Example: GUI with Docker

See examples at [hub.docker.com/u/jess](https://hub.docker.com/u/jess/)

```
// Before staring we should grant access to everyone on the X Server (locally)
// Otherwise the containers below will never start and they will not be able to use x11
xhost +

// Libreoffice
docker run  -d \
            -v /etc/localtime:/etc/localtime:ro \
            -v /tmp/.X11-unix:/tmp/.X11-unix \
            -e DISPLAY=unix$DISPLAY \
            -e GDK_SCALE \
            -e GDK_DPI_SCALE \
            --name libreoffice \
            jess/libreoffice

// SublimeText 3
docker run -it \
           -v $HOME/.config/sublime-text-3/:/root/.config/sublime-text-3 \
           -v /tmp/.X11-unix:/tmp/.X11-unix \
           -e DISPLAY=$DISPLAY \
           --name sublime_text \
           jess/sublime-text-3

// Audacity (sound in docker container)
docker run  -d \
            -v /etc/localtime:/etc/localtime:ro \
            -v /tmp/.X11-unix:/tmp/.X11-unix \
            -e DISPLAY=unix$DISPLAY \
            -e QT_DEVICE_PIXEL_RATIO \
            --device /dev/snd \
            --group-add audio \
            --name audacity \
            jess/audacity

// Disable access to x11
xhost -

```

---

### Docker tips

There are known best practices (see a list at [examples/tips](https://github.com/madeinchina/docker-presentation/tree/gh-pages/examples/tips))

- Optimize containers (check [fromlatest.io](https://www.fromlatest.io/) and [imagelayers.io](https://imagelayers.io))
- Create your own tiny base
- Containers are not Virtual Machines
- Full stack Images VS 1 process per Container
- Create your private registry
- Create shortcut commands
- Use docker-compose.yml templates (see why at [lorry.io](https://lorry.io/))
- Be aware of the hub.docker.com docker agent version

---

### The Docker war

| Type | Software |
|:----:|----------|
| Clustering/orchestration | [Swarm](https://docs.docker.com/swarm/), [Kubernetes](http://kubernetes.io/), [Marathon](https://mesosphere.github.io/marathon/), [MaestroNG](https://github.com/signalfx/maestro-ng), [decking](http://decking.io/), [shipyard](http://shipyard-project.com/) |
| Docker registries | [Portus](http://port.us.org/), [Docker Distribution](https://github.com/docker/distribution), [hub.docker.com](http://hub.docker.com), [quay.io](https://quay.io), [Google container registry](https://cloud.google.com/tools/container-registry/), [Artifactory](https://www.jfrog.com/artifactory/), [projectatomic.io](http://www.projectatomic.io/) |
| PaaS with Docker | [Rancher](http://rancher.com/), [Tsuru](https://tsuru.io/), [dokku](https://github.com/dokku/dokku), [flynn](https://flynn.io/),  [Octohost](http://octohost.io/), [DEIS](http://deis.io/) |
| OS made of Containers | [RancherOS](http://rancher.com/rancher-os/) |

---

### Docker Alternatives

- [Rocket, rkt](https://github.com/coreos/rkt)
- [Linux Containers, LXC](https://linuxcontainers.org/)
- [Linux container hypervisor, LXD](http://www.ubuntu.com/cloud/lxd)
- [BSD Jails](https://www.freebsd.org/doc/handbook/jails.html)
- [Solaris Zones](http://oracle.com/solaris)
- [drawbridge](http://research.microsoft.com/en-us/projects/drawbridge/)

---

### Instead of Resources

 - [Awesome Docker](https://github.com/veggiemonk/awesome-docker) (list of Docker resources & projects)
 - [Docker cheat sheet](https://github.com/wsargent/docker-cheat-sheet)
 - [Docker in Practice](https://www.manning.com/books/docker-in-practice), [The Docker Book](http://www.dockerbook.com/) (books)
 - [Docker aliases/shortcuts](https://github.com/madeinchina/docker-presentation/tree/gh-pages/examples/shortcuts/docker-aliases.sh)
 - Docker [case studies](https://www.docker.com/customers)

---

### Questions?

![Pythons over Docker!](https://raw.githubusercontent.com/madeinchina/docker-presentation/gh-pages/img/docker_logo.png)

[Review this presentation](https://goo.gl/lkau9t)

> Next: Docker in production, Scaling, Private registries, PaaS.

###### In this presentation I have used [oh my zsh](http://ohmyz.sh/), [docker 1.11.1](https://github.com/docker/docker/releases/tag/v1.11.1), [wharfee](https://github.com/j-bennet/wharfee) and [dry](https://github.com/moncho/dry).

---

### Bonus!

> Get the [SKGTech.io docker image](https://github.com/skgtech/skgtech.io-docker)
