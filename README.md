# Docker Overview

## Common Docker Commands

| Command
| -----------
| docker build [image name]
| docker rmi [imageId]
| docker ps
| docker run [image name]
| docker stop [containerId]
| docker start [containerId]
| docker rm [containerId]|

## Pulling a Docker image from Docker Hub

This will pull the nginx image with the alpine version.

```
docker pull nginx:alpine
```

## View existing local images

This will show all of the existing Docker images on your local machine.

```
docker images
```
Example:

```
PSCore C:\src> docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
nginx-angular          latest              057bcdd47b08        4 days ago          21.3MB
angular-node-service   latest              52c3a6b37bda        4 days ago          120MB
node                   alpine              3bf5a7d41d77        9 days ago          117MB
nginx                  alpine              7d0cdcc60a96        9 days ago          21.3MB
```

## Removing a Docker image

To remove a Docker image, just enter the **docker rmi** command followed by the first couple unique characters of the **Image ID**.

```
docker rmi [image id]
```
Example:

```
PSCore C:\src> docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
nginx-angular          latest              057bcdd47b08        5 days ago          21.3MB
angular-node-service   latest              52c3a6b37bda        5 days ago          120MB
node                   alpine              3bf5a7d41d77        9 days ago          117MB
nginx                  alpine              7d0cdcc60a96        9 days ago          21.3MB
```
```
PSCore C:\src> docker rmi 057
```
```
PSCore C:\src> docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
angular-node-service   latest              52c3a6b37bda        5 days ago          120MB
node                   alpine              3bf5a7d41d77        9 days ago          117MB
nginx                  alpine              7d0cdcc60a96        9 days ago          21.3MB
```

## Removing all Docker images

To remove all Docker images, you'll need to put the **docker images -q** command into a variable to run it again the **docker rmi** command.

```
docker rmi $(docker images -q)
```
Example:

```
PSCore C:\src> docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
angular-node-service   latest              52c3a6b37bda        5 days ago          120MB
node                   alpine              3bf5a7d41d77        9 days ago          117MB
nginx                  alpine              7d0cdcc60a96        9 days ago          21.3MB
```
```
PSCore C:\src> docker rmi $(docker images -q)
```
```
PSCore C:\src> docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

