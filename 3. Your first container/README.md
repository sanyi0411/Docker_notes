# First steps

## How to install docker

### Terminal

On Linux you can use the CLI based verison of Docker: https://docs.docker.com/engine/

### Docker Desktop

Follow the official installation: 
- Linux: https://docs.docker.com/desktop/setup/install/linux/ubuntu/
- Windows: https://docs.docker.com/desktop/setup/install/windows-install/
- Mac: https://docs.docker.com/desktop/setup/install/mac-install/

## Running your first container

```bash
$ docker run hello-world

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
- Docker daemon checks if the `hello-world` image is available locally.
- If not, it automatically downloads (aka "pulls") the image from Docker Hub.
- Docker daemon creates a new container based on this image.
- The container runs, the container's output was sent back to your terminal, and then the container exits.

