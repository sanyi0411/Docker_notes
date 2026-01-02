# Container lifecycle

```
                                docker run
----------------------------------------------------------------------------+
                                                                            |
                                                                            ↓         
                           +-------------+                           +-------------+     docker pause     +------------+
    docker create          |             |       docker start        |             |  ------------------> |            |
-------------------------> |   CREATED   | ------------------------> |   RUNNING   |  <------------------ |   PAUSED   |
                           |             |                           |             |     docker unpause   |            |
                           +-------------+                           +-------------+                      +------------+
                                      |                                    | ↑         
                                      |                                    | | docker start
                                      |                       docker stop  | |
                                      |                                    ↓ |
                                      |                              +-------------+
                            docker rm |                              |             |
                                      |                              |   STOPPED   |
                                      |                              |             |
                                      |                              +-------------+
                                      |                  docker rm  /
                                      |              ---------------
                                      ↓             /                          
                                   +------------------+        
                                   |                  |
                                   |     DELETED      |
                                   |                  |
                                   +------------------+

```

## Pulling an image
```bash
$ docker pull nginx
```
- The `nginx` image is now stored locally on your system. This means that if you run `docker run nginx` again, Docker won't need to download the image from Docker Hub. It will use the local copy, making the process faster.

## Creating a container
- If you have the image pulled, you can create a container from it, without starting the container
```bash
$ docker pull nginx
$ docker create nginx
1a2b3c4d
```
- The return value of the `docker create` command is the `CONTAINER ID` (hash) of the created container (if successful)
- By default, Docker creates a name for your container
    - You can specify the name of your container:
```bash
$ docker create --name myawesomecontainer nginx
1a2b3c4d
```
- The `docker create` command only creates the container but does not start it
    - Check the difference between the two following listing:
```bash
$ docker ps
$ docker ps -a
```

## Starting a container
- You have to have an existing container to start
- You can start a container that has been created but never run
- Or you can start a container that previously run but has been stopped
```bash
$ docker start nginx
```

## Running a container
- Running a container includes pulling the image if needed, creating a container from the image and then starting the container
    - It is a shorthand for all of the above
```bash
$ docker run hello-world
```

## Modes of containers
- You can run a container in `detached` or `interactive` mode
- Use the `-d` option to run a container in `detached` mode
```bash
$ docker run -d nginx
```
- Use the `-it` option ti run a container in `interactive` mode
```bash
$ docker run -it ubuntu /bin/bash
```

## List containers
- To list locally running containers:
```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
81fc63386db6   nginx     "/docker-entrypoint.…"   4 seconds ago   Up 3 seconds   80/tcp    clever_kare
```
- To list all local containers, running or stopped:
    - Check the different `STATUS`es
```bash
$ docker ps -a
CONTAINER ID   IMAGE                    COMMAND                  CREATED              STATUS                          PORTS     NAMES
81fc63386db6   nginx                    "/docker-entrypoint.…"   33 seconds ago       Up 32 seconds                   80/tcp    clever_kare
2a0367683210   hello-world              "/hello"                 2 minutes ago        Exited (0) About a minute ago             optimistic_knuth
a5c8b09781a5   nginx                    "/docker-entrypoint.…"   8 minutes ago        Created                                   ecstatic_wu
```

## Stopping a container
- You can stop a running container:
```bash
$ docker stop nginx
```

## Removing a container
- You cannot remove a remove a running container, first you have to stop it
```bash
$ docker rm nginx
```

## Removing an image
- To remove an image from your system:
    - `rmi`: remove image
```bash
$ docker rmi python:3.7
```
- If you get the following error, then there is a container (running or stopped) created from the image you are trying to delete
```
Error response from daemon: conflict: unable to remove repository reference "python:3.7" (must force) - container <container_id> is using its referenced image <image_id>
```
- look for the image using `docker ps -a`
- and delete the container using `docker rm <container_id>`