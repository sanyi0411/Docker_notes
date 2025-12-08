# Docker notes
This repository contains the notes I took during learning about Docker and containerization

# Documentation

Official Docker documentation: https://docs.docker.com/

# History

- Inventor: Solomon Hykes
- 2013 March: First public announcement at PyCon
- 2013 September: RedHat collaboration
- 2014: Microsoft announces Windows integration

- All Docker versions are backwards compatible

# Glossary

**Container**: A lightweight, standalone, and executable package that includes everything needed to run a piece of software.

**Image**: Template or blueprint for containers. It contains all the instructions needed to create a container. Images are used to create containers.

**Dockerfile**: The script used to build the image.

**Docker Hub**: A cloud-based registry, serving as a central repository, service where you can store, find and share container images.

**Docker Engine**: The core technology that runs and manages containers on your machine. It can pull images from Docker Hub and push images to Docker Hub

**Daemon**: an application in Linux that runs in the background. If configured it will start with the OS if the computer is restarted

# VMs vs Containers

```
VM                                  Container
```
```
+-----------------------+
|  App  |  App  |  App  |
|   1   |   2   |   3   |
+-----------------------+          +-----------------------+
| Bin/  | Bin/  | Bin/  |          |  App  |  App  |  App  |
|  Lib  |  Lib  |  Lib  |          |   1   |   2   |   3   |
+-----------------------+          +-----------------------+
|       |       |       |          | Bin/  | Bin/  | Bin/  |
| Guest | Guest | Guest |          |  Lib  |  Lib  |  Lib  |
|  OS   |  OS   |  OS   |          +-----------------------+
|       |       |       |          |  Container engine     |
+-----------------------+          +-----------------------+
|                       |          |                       |
|      Hypervisor       |          |   Operating System    |
|                       |          |                       |
+-----------------------+          +-----------------------+
|                       |          |                       |
|      Hardware         |          |       Hardware        |
|                       |          |                       |
+-----------------------+          +-----------------------+
```
VMs are pets:
- they have names
- we take care of them
- they have long uptimes
- we work on them manually
- SW update:
    - create a maintenance window
    - shut down the VM
    - install the new SW
    - start the VM
    - test
    - hard to rollback if something goes wrong

Containers are cattle:
- they don't have special names, only random generated IDs
- a container with an issue is shut down and new one is started
- most of the work on them are automated
- SW update:
    - build new image
    - start containers with new image
    - stop containers with old image
    - no need for maintenance window
    - change takes seconds
    - easy rollback if something goes wrong, just start containers with old version

# Containers

Containers achieve isolation using the Linux kernel's existing features:
- `namespaces` and 
- `cgroups`

Containers have their own process trees, network, users, file system

# Basic Docker architecture

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


**Docker client** (`docker`): the part you interact with. The client sends your commands to `dockerd`. The client can communicate with more than one daemon.

**Docker daemon** (`dockerd`): the background service that manages Docker objects (e.g.: images, containers, networks, volumes) on your machine

# How to install docker

### Terminal

On Linux you can use the CLI based verison of Docker: https://docs.docker.com/engine/

### Docker Desktop

Follow the official installation: 
- Linux: https://docs.docker.com/desktop/setup/install/linux/ubuntu/
- Windows: https://docs.docker.com/desktop/setup/install/windows-install/
- Mac: https://docs.docker.com/desktop/setup/install/mac-install/

# Running your first container

```
> docker run hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete
Digest: sha256:...
Status Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```
When you run this command:
- The Docker client contacted the Docker daemon
- Docker daemon checks if the hello-world image is available locally.
- If not, it automatically downloads (aka "pulls") the image from Docker Hub.
- Docker daemon creates a new container based on this image.
- The container runs, the container's output was sent back to your terminal, and then the container exits.

# Docker images

**Docker image**: an image is like a blueprint or a template for a container

To see the images available on your local system:
```
> docker images

REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
hello-world         latest    feb5d9fea6a5   2 weeks ago     13.3kB
```

The `hello-world` image is now stored locally on your system. This means that if you run `docker run hello-world` again, Docker won't need to download the image from Docker Hub. It will use the local copy, making the process faster.

**REPOSITORY**: the name of the image

**TAG**: The version of the image. "latest" is the default tag

**IMAGE ID**: A unique identifier for the image (useful when you need to refer to a specific image)

**CREATED**: The time when the image was created (helps you know if you have the most recent version)

**SIZE**: The size of the image on disk

# Docker hub

https://hub.docker.com/

**Docker Hub**: A cloud-based registry, serving as a central repository, service where you can store, find and share container images.

**Official Images**: Curated by Docker and are typically well-maintained and documented.

**Pull Command**: Command to manually download the image without running a container.
```
docker pull hello-world
```
Q: Why use `docker pull` and not just `docker run`?

A: `docker runs` includes `docker pull` so you don't need to separately run `docker pull`. But if you know what the image you will be using, you can download it beforehand with `docker pull`. Later, when you want to start your container, it will start up much faster because the image is already downloaded and available locally.

# Docker containers
```
> docker ps

CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS          PORTS     NAMES
f3f3b3b3b3b3   docker/getting-started   "/docker-entrypoint.…"   1 minute ago   Up 1 minute   80/tcp   festive_mendel
```
