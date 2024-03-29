## Docker CLI

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
    
    
    
### Clear Everything: 

- remove images
    `docker rmi $(docker images -q)`
- remove containers
    `docker stop $(docker ps -aq)`
      `docker rm $(docker ps -aq)`
- remove volumes
    `docker volume rm $(docker volume ls -q)`
    
   
### Network

`docker container inspect --format '{{ .NetworkSettings.IPAddress }}' <container_name>`
    - inspect the IP of the container
    
    
`docker container post <container>`
    - check port of the container
    
    
`docker network inspect <n/w name`

`docker network ls`

`docker network create`
- spawn a new virtual n/w to connect our containers to


## Docker Compose

`docker-compose up` provided a `docker-compose.yml` file is in the directory

`docker-compose up --build` to re-build and then run the containers

`docker-compose down` for stopping 

`docker-compose ps` (in the directory of the `docker-compose.yml` file) to  
