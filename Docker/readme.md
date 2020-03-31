### Docker Info & Resources page

- [Docker Cheat Sheet - Docker](https://www.docker.com/sites/default/files/d8/2019-09/docker-cheat-sheet.pdf)
    - Build, share, run, services, etc.
    
- [Docker Cheat Sheet - Github](https://github.com/wsargent/docker-cheat-sheet#containers)
    - ```docker version``` -  shows which version of docker you have running
    
    - Containers: Your basic isolated Docker process. Containers are to Virtual Machines as threads are to processes. Or you can think of them as chroots on steroids.
Lifecycle:
        - ```docker create``` -  creates a container but does not start it.
        - ```docker rename``` -  allows the container to be renamed.
        - ```docker run``` -  creates and starts a container in one operation.
        - ```docker rm``` -  deletes a container.
        - ```docker update``` -  updates a container's resource limits.
        
        Normally if you run a container without options it will start and stop immediately, if you want keep it running you can use the command, docker run -td container_id this will use the option -t that will allocate a pseudo-TTY session and -d that will detach automatically the container (run container in background and print container ID).
        
    - Starting and Stopping:
        - ```docker start``` -  starts a container so it is running.
        - ```docker stop``` -  stops a running container.
        - ```docker restart``` -  stops and starts a container.
        - ```docker pause``` -  pauses a running container, "freezing" it in place.
        - ```docker unpause``` -  will unpause a running container.
        - ```docker wait```  -  blocks until running container stops.
        - ```docker kill```  -  sends a SIGKILL to a running container.
        - ```docker attach```  -  will connect to a running container.
        
      If you want to detach from a running container, use Ctrl + p, Ctrl + q. If you want to integrate a container with a host process manager, start the daemon with -r=false then use docker start -a.


- [Quickstart](https://docs.docker.com/get-started/)


