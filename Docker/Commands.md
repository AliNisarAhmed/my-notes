- Create a container 
    `docker create <image-name>`
    
    
    
- Start a container 
     `docker start <container-id>`
     
     - watch for output from the just started container and print it 

        `docker start -a <container-id>`



- Create and Run a Container
    `docker run hello-world`



- Override the default command 

`docker run <image-name> <command>`

`docker run -it <image-name> sh` - start a container and go into shell of the new container



- Listing running containers 

    `docker ps`
    
     - Listing all containers
     
         `docker ps -a`



- Removing containers 

    `docker system prune`
    
    
- Get logs from a container 
    
    `docker logs <container-id>`
    
    
- Stopping containers 
    `docker stop <container-id>`  - issues a SIGTERM sys call (meaning, terminate on your on terms) - if container does not stop in 10 seconds, docker will issue a SIGKILL anyways
    
    `docker kill <container-id>` - issues a SIGKILL sys call (meaning, terminate now)
    
    
- Execute additional commands inside a container
    
    `docker exec -it <container-id> <command>` - without the -it flag , the new command will not be attached to the STDIN