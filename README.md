# Docker Tips

## create image and skip cache
    
    docker build -f docker/DockerfileGW -t device-gw --no-cache .

## list images
    docker images

## list all containers
    docker ps -a

## attach to docker container
     - check container id of running container: docker ps
     - docker exec -it <container_id> /bin/bash

## remove all images exept img1, img2 and img3:
    docker rmi $(docker images | grep -v 'IMAGE\|img1\|img2\|img3' | awk {'print $3'})

## remove all containers exept c1, c2 and c3:
    docker rm -f $(docker ps -a | grep -v 'CONTAINER\|c1\|c2\|c3' | awk {'print $1'})

## create isolated network:
    docker network create --driver bridge isolated_nw

## run container:
    docker run -d \
       --network=isolated_nw \
       --name=demo-app \
       -p 8000:8000 \
       -e "MONGO_URL=mongodb://mongo/V5-TEST" \
       demo-app

    * --name - container name
    * -p - port mapping
    * -e - ENV
    * demo-app - image name

## copy file from container to host machine
    docker cp container-name:/path/to/file /local/path

## copy file from host machine to container
    docker cp /local/path/to/file container-name:/path/to/file

## cleanup space consumed by unused Docker images, containers, volumes
    docker rm $(docker ps -q -f 'status=exited')
    docker rmi $(docker images -q -f "dangling=true")
    docker volume rm $(docker volume ls -qf dangling=true)
