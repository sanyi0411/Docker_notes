# Docker basics

## Table of contents
[Documentation](#documentation) </br>
[History](#history) </br>
[Glossary](#glossary) </br>
[Basic Docker architecture](#basic-docker-architecture) </br>
[Docker images](#docker-images) </br>
[Docker hub](#docker-hub) </br>
[Docker containers](#docker-containers) </br>

## Documentation

Official Docker documentation: https://docs.docker.com/

## History

- Inventor: Solomon Hykes
- 2013 March: First public announcement at PyCon
- 2013 September: RedHat collaboration
- 2014: Microsoft announces Windows integration

- All Docker versions are backwards compatible

## Glossary

**Dockerfile**: The script used to build the image. Docker builds images by reading the instructions from a Dockerfile. It contains instructions for building your source code. See specifications [here](https://docs.docker.com/reference/dockerfile/)

**Image**: Template or blueprint for containers. An image is a standardized package that includes all of the files, binaries, libraries dependencies and configurations to run a container. Images are used to create containers. 

**Container**: A lightweight, standalone, and executable package that includes everything needed to run a piece of software.

**Docker Hub**: A cloud-based registry, serving as a central repository, service where you can store, find and share container images.

**Docker Engine**: The core technology that runs and manages containers on your machine. It can pull images from Docker Hub and push images to Docker Hub

**Docker client** (`docker`): the part you interact with. The client sends your commands to `dockerd`. The client can communicate with more than one daemon.

**Daemon**: an application in Linux that runs in the background. If configured it will start with the OS if the computer is restarted

**Docker daemon** (`dockerd`): the background service that manages Docker objects (e.g.: images, containers, networks, volumes) on your machine

## Basic Docker architecture

```
+-----------+        +-----------------------------------+          +--------------+
|  CLIENT   |        |                HOST               |          |   REGISTRY   |   
|           |        |                                   |          |              |
|           |        |    +-------------------------+    |          |  Docker Hub  |
|  Terminal | ---->  |    |      Docker Daemon      |    | ------>  |              |
|           | <----  |    |                         |    |          |              |
|    or     |        |    +-------------------------+    |          |              |
|           |        |                                   |          |    Images    |
|  Docker   |        |                                   |          |  Extensions  |
|  Desktop  |        | +------------+       +----------+ |          |    Plugins   |
|           |        | | Containers | <---- |  Images  | |  <-----  |              |
|           |        | +------------+       +----------+ |          |              |
|           |        |                                   |          |              |
+-----------+        +-----------------------------------+          +--------------+
 
```

Docker is using the Linux kernel to run the containers

On Windows and Mac there is a Linux virtual machine created for you that runs the containers

## Docker images

- An image is like a blueprint or a template for creating a container. An image is a standardized package that includes all of the files, binaries, libraries dependencies and configurations to run a container.
- Images are composed of layers. Each layer represents a set of file system changes (add, remove, modify)
- Every command in the Dockerfile is a set of file system change, so each layer can be matched to a line in the Dockerfile
- Images are immutable, they cannot be modified. You can either create a new image or add a layer on top of an image

- To see the images available on your local system:
    - `docker images` is the same as `docker image ls`
```bash
$ docker image ls
$ docker images
REPOSITORY     TAG     IMAGE ID       CREATED       SIZE
hello-world    latest  feb5d9fea6a5   2 weeks ago   13.3kB
```

- **REPOSITORY**: the name of the image
- **TAG**: The version of the image. "latest" is the default tag
    - Docker allows you to run specific versions of an image by using tags.
    - For example you can download multiple python images:
    - To see all python images on your local system:
    ```bash
    $ docker images python
    REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
    python       latest    1edf53fea028   2 days ago    1.12GB
    python       3.7       16d93ae3411b   2 years ago   994MB
    ```
- **IMAGE ID**: A unique identifier for the image (useful when you need to refer to a specific image)
- **CREATED**: The time when the image was created (helps you know if you have the most recent version)
- **SIZE**: The size of the image on disk

## Docker hub

https://hub.docker.com/

- **Docker Hub**: A cloud-based registry, serving as a central repository, service where you can store, find and share container images. A public repository for Docker images. You can find pre-built images for many popular software applications. 
- **Official Images**: Curated by Docker and are typically well-maintained and documented.
- **Pull Command**: Command to manually download the image without running a container.
```bash
$ docker pull hello-world
```
- Q: Why use `docker pull` and not just `docker run`?
- A: `docker run` includes `docker pull` so you don't need to separately run `docker pull`. But if you know what the image you will be using, you can download it beforehand with `docker pull`. Later, when you want to start your container, it will start up much faster because the image is already downloaded and available locally.

## Docker containers
- A container is an isolated process, it has minimal influence on the host and other containers
- Containers are self-contained, they have everything they need to function, with no reliance on any pre-installed dependencies on the host machine
- Containers are independent. Deleting one container will not effect any other container
- Containers are easily portable, can run anywhere, and will work the same way on any machine 
```bash
$ docker ps
CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS          PORTS     NAMES
f3f3b3b3b3b3   docker/getting-started   "/docker-entrypoint.…"   1 minute ago   Up 1 minute   80/tcp   festive_mendel
```
