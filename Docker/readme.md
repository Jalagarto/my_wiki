# Docker Info & Resources page

## [Docker Cheat Sheet - Docker](https://www.docker.com/sites/default/files/d8/2019-09/docker-cheat-sheet.pdf)
    - Build, share, run, services, etc.
    
## [Docker Cheat Sheet - Github](https://github.com/wsargent/docker-cheat-sheet#containers)
    - ```docker version``` -  shows which version of docker you have running
    
#### Containers: Your basic isolated Docker process. Containers are to Virtual Machines as threads are to processes. Or you can think of them as chroots on steroids.

- Lifecycle:
        - ```docker create``` -  creates a container but does not start it.
        - ```docker rename``` -  allows the container to be renamed.
        - ```docker run``` -  creates and starts a container in one operation.
        - ```docker rm``` -  deletes a container.
        - ```docker update``` -  updates a container's resource limits.
        
        Normally if you run a container without options it will start and stop immediately, if you want keep it running you can use the command, docker run -td container_id this will use the option -t that will allocate a pseudo-TTY session and -d that will detach automatically the container (run container in background and print container ID).
        
#### Starting and Stopping:
        - ```docker start``` -  starts a container so it is running.
        - ```docker stop``` -  stops a running container.
        - ```docker restart``` -  stops and starts a container.
        - ```docker pause``` -  pauses a running container, "freezing" it in place.
        - ```docker unpause``` -  will unpause a running container.
        - ```docker wait```  -  blocks until running container stops.
        - ```docker kill```  -  sends a SIGKILL to a running container.
        - ```docker attach```  -  will connect to a running container.
        
      If you want to detach from a running container, use Ctrl + p, Ctrl + q. If you want to integrate a container with a host process manager, start the daemon with -r=false then use docker start -a.
      
#### Info:
        - ```docker ps``` - shows running containers.
        - ```docker logs``` - gets logs from container. (You can use a custom log driver, but logs is only available for json-file and journald in 1.10).
        - ```docker inspect``` - looks at all the info on a container (including IP address).
        - ```docker events``` - gets events from container.
        - ```docker port``` - shows public facing port of container.
        - ```docker top``` - shows running processes in container.
        - ```docker stats``` - shows containers' resource usage statistics.
        - ```docker diff``` - shows changed files in the container's FS.

    ```docker ps -a``` - shows running and stopped containers.

    ```docker stats --all``` - shows a list of all containers, default shows just running.
Import / Export

    ```docker cp``` - copies files or folders between a container and the local filesystem.
    
    ```docker export``` - turns container filesystem into tarball archive stream to STDOUT.

    - Executing Commands:

        - ```docker exec``` - to execute a command in container.

    To enter a running container, attach a new shell process to a running container called foo, use: docker exec -it foo /bin/bash.

#### Images

    Images are just templates for docker containers.
    
- Lifecycle

    - ```docker images``` - shows all images.
    - ```docker import``` - creates an image from a tarball.
    - ```docker build``` - creates image from Dockerfile.
    - ```docker commit``` - creates image from a container, pausing it temporarily if it is running.
    - ```docker rmi``` - removes an image.
    - ```docker load``` - loads an image from a tar archive as STDIN, including images and tags (as of 0.7).
    - ```docker save``` - saves an image to a tar archive stream to STDOUT with all parent layers, tags & versions (as of 0.7).

- Info

    - ```docker history``` - shows history of image.
    - ```docker tag``` - tags an image to a name (local or registry).

#### Cleaning up

While you can use the docker rmi command to remove specific images, there's a tool called docker-gc that will safely clean up images that are no longer used by any containers. As of docker 1.13, docker image prune is also available for removing unused images. See Prune.

#### Load/Save image

Load an image from file:
    ```docker load < my_image.tar.gz```

Save an existing image:
    ```docker save my_image:my_tag | gzip > my_image.tar.gz```
    
- Difference between loading a saved image and importing an exported container as an image

Loading an image using the load command creates a new image including its history.
Importing a container as an image using the import command creates a new image excluding the history which results in a smaller image size compared to loading an image.    

#### Dockerfile

The configuration file. Sets up a Docker container when you run docker build on it. Vastly preferable to docker commit

- Instructions:

    .dockerignore
    FROM Sets the Base Image for subsequent instructions.
    MAINTAINER (deprecated - use LABEL instead) Set the Author field of the generated images.
    RUN execute any commands in a new layer on top of the current image and commit the results.
    CMD provide defaults for an executing container.
    EXPOSE informs Docker that the container listens on the specified network ports at runtime. NOTE: does not actually make ports accessible.
    ENV sets environment variable.
    ADD copies new files, directories or remote file to container. Invalidates caches. Avoid ADD and use COPY instead.
    COPY copies new files or directories to container. By default this copies as root regardless of the USER/WORKDIR settings. Use --chown=<user>:<group> to give ownership to another user/group. (Same for ADD.)
    ENTRYPOINT configures a container that will run as an executable.
    VOLUME creates a mount point for externally mounted volumes or other containers.
    USER sets the user name for following RUN / CMD / ENTRYPOINT commands.
    WORKDIR sets the working directory.
    ARG defines a build-time variable.
    ONBUILD adds a trigger instruction when the image is used as the base for another build.
    STOPSIGNAL sets the system call signal that will be sent to the container to exit.
    LABEL apply key/value metadata to your images, containers, or daemons.
    SHELL override default shell is used by docker to run commands.
    HEALTHCHECK tells docker how to test a container to check that it is still working.

- Tutorial:

    [Flux7's Dockerfile Tutorial](https://www.flux7.com/tutorial/docker-tutorial-series-part-3-automation-is-the-word-using-dockerfile/)


#### Layers

The versioned filesystem in Docker is based on layers. They're like git commits or changesets for filesystems.

#### Volumes

Docker volumes are free-floating filesystems. They don't have to be connected to a particular container. You can use volumes mounted from data-only containers for portability. As of Docker 1.9.0, Docker has named volumes which replace data-only containers. Consider using named volumes to implement it rather than data containers.

- Lifecycle

    - ```docker volume create```
    - ```docker volume rm```

- Info

    - ```docker volume ls```
    - ```docker volume inspect```

Volumes are useful in situations where you can't use links (which are TCP/IP only). For instance, if you need to have two docker instances communicate by leaving stuff on the filesystem.


Get Environment Settings

    - ```docker run --rm ubuntu env```

Kill running containers

    - ```docker kill $(docker ps -q)```

Delete all containers (force!! running or stopped containers)

    - ```docker rm -f $(docker ps -qa)```

Delete old containers

    - ```docker ps -a | grep 'weeks ago' | awk '{print $1}' | xargs docker rm```

Delete stopped containers

    - ```docker rm -v $(docker ps -a -q -f status=exited)```

Delete containers after stopping

    - ```docker stop $(docker ps -aq) && docker rm -v $(docker ps -aq)```

Delete dangling images

    - ```docker rmi $(docker images -q -f dangling=true)```

Delete all images

    - ```docker rmi $(docker images -q)```

Delete dangling volumes

As of Docker 1.9:

    - ```docker volume rm $(docker volume ls -q -f dangling=true)```

In 1.9.0, the filter dangling=false does not work - it is ignored and will list all volumes.
Show image dependencies

    - ```docker images -viz | dot -Tpng -o docker.png```

Monitor system resource utilization for running containers

To check the CPU, memory, and network I/O usage of a single container, you can use:

    - ```docker stats <container>```

For all containers listed by id:

    - ```docker stats $(docker ps -q)```

For all containers listed by name:

    - ```docker stats $(docker ps --format '{{.Names}}')```

For all containers listed by image:

    - ```docker ps -a -f ancestor=ubuntu```

Remove all untagged images:

    - ```docker rmi $(docker images | grep “^” | awk '{split($0,a," "); print a[3]}')```

Remove container by a regular expression:

    - ```docker ps -a | grep wildfly | awk '{print $1}' | xargs docker rm -f```

Remove all exited containers:

    - ```docker rm -f $(docker ps -a | grep Exit | awk '{ print $1 }')```


## [Quickstart](https://docs.docker.com/get-started/)

## [caylent - cheatsheet](https://caylent.com/docker-commands-cheat-sheet)

## [EDUCBA - COMMANDS - nice!](https://www.educba.com/docker-commands-cheat-sheet/)

## [Docker Cheat Sheet with examples](https://coderwall.com/p/2es5jw/docker-cheat-sheet-with-examples)

## [Commands! nice](https://phoenixnap.com/kb/list-of-docker-commands-cheat-sheet)

## [Dockerfile - config_file](https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index)


