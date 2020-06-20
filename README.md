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

This will show only the image ID.

```
docker images -q
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

## Cleanup System

To clean up your system from fragments, you'll run a pruning command.

```
docker system prune
```

Example:

```
PSCore C:\src\Docker> docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y
```

## Running a container in detached mode

This command will run a new container detached and will host a website on port 8080 of the host and port 80 inside the container.

```
docker run -d -p 8080:80 --name web1 nginx:alpine
```

## Get running containers

This command will allow you to view all running containers.

```
docker ps
```
Example:

```
PSCore C:\src> docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS
     NAMES
537bb2ac04d8        nginx:alpine        "/docker-entrypoint.…"   5 minutes ago       Up 5 minutes        0.0.0.0:8080->80/tcp   web1
```

## Get all containers

This command will allow you to view all containers.  Containers that are stopped and running.

```
docker ps -a
```

Example:

```
PSCore C:\src> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS                  NAMES
847bf6a810ce        nginx:alpine        "/docker-entrypoint.…"   22 seconds ago      Exited (0) 5 seconds ago
            web2
537bb2ac04d8        nginx:alpine        "/docker-entrypoint.…"   5 minutes ago       Up 5 minutes               0.0.0.0:8080->80/tcp   web1
```

## Stopping a container

```
docker stop web1
```

## Starting a container

```
docker start web1
```

## Removing a container

Before removing a container, it's best to stop it first.  You can specify the name of the container or the container ID.

```
docker rm web1
```

This command will allow you to stop and remove the container in one line.

```
docker stop web1 && docker rm web1
```

## Removing all containers

This command will remove all of your containers and will force them to stop if they are running.

```
docker rm $(docker ps -aq) -f
```

## Running a container interactively

```
docker -it [image] [command]
```

Example:

In the example, I gave the container a name, which is optional.

```
PSCore C:\src> docker run -it --name test01 ubuntu bash
root@da5f8ea05376:/#
```
## Viewing running processes inside the container

```
ps -elf
```

Example:

You can see bash is the only running process in this container.

```
root@da5f8ea05376:/# ps -elf
F S UID        PID  PPID  C PRI  NI ADDR SZ WCHAN  STIME TTY          TIME CMD
4 S root         1     0  0  80   0 -  1028 -      13:46 pts/0    00:00:00 bash
0 R root        10     1  0  80   0 -  1471 -      13:49 pts/0    00:00:00 ps -elf
```

## Exiting an interactive session but leave the container running

If you enter **exit** to leave the interactive session, the container will stop.  In order to exit the container without stopping it, you must press the following keyboard combination **CTRL Q P**

## Go back into a running container interactively

```
docker exec -it [name or ID] [command]
```
Example:

```
docker exec -it test01 bash
```

Another command that work similarly

```
docker attach test01
```

## Viewing the properties of a container

This will give you all sorts of information about the container including IP address, ports, volume information, and more.

```
docker inspect web1
```

# Volumes

## Create a volume from the Docker host

This will create a mount point in a container that points to externally mounted volumes on the host or other containers.

```
docker run -it -v [directory in container] --name test04 ubuntu bash
```
Example:
```
docker run -it -v /data --name test04 ubuntu bash
```

## View existing container on the host

```
docker volume ls
```

Example:

```
PSCore C:\src> docker volume ls
DRIVER              VOLUME NAME
local               3f9a536a7e270ed7f634bf9a7a5d8b93cbe6c3eeea5920af8964612e99fc935e
```
## Removing volumes from the host
This show two different ways to delete volumes from the host.

```
docker rm -v [full containerId]
```

```
docker volume prune
```

## Create a volume from your current local working directory

This will mount your currently working directory to whatever directory you choose inside the contatiner.  Use ${pwd} for Windows and $(pwd) for Bash.

```
docker run -d -p 8080:80 -v ${pwd}:[path in container] [image]
```

Example:
```
docker run -d -p 8080:80 -v ${pwd}:/user/share/nginx/html nginx:alpine
```

## Create a volume by specifying a local directory
This will mount a local directory that you specify to a directory in the container that you specifiy.

```
docker run -v [local path]:[path in contatiner] --name test02 -it ubuntu bash
```

Example:

```
docker run -v /c/src:/data --name test02 -it ubuntu bash
```

## View the mount point inside the container

```
df -H
```

Example:

You can see the mount point **Mounted on** **/data**
```
root@dd685edd80b9:/# df -H
Filesystem      Size  Used Avail Use% Mounted on
overlay          63G  1.3G   59G   3% /
tmpfs            68M     0   68M   0% /dev
tmpfs           1.1G     0  1.1G   0% /sys/fs/cgroup
shm              68M     0   68M   0% /dev/shm
grpcfuse        500G   76G  424G  16% /data
```

## View the mount point using the inspect command

You'll have to scroll up a little to find the **Mounts** section
```
docker inspect test02
```

```
   "Mounts": [
            {
                "Type": "bind",
                "Source": "/host_mnt/c/src",
                "Destination": "/data",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
```

## Mapping a volume from one container to a new container
Start a new container and use the -volumes-from flag to map the mapped volume from the source container to a new container being launched.

```
docker run --volumes-from test01 --name test02 -it ubuntu bash
```

# Networking

## Different types of networks in containers

### **Bridge Networking**
A bridge network provides a bridge between the host's network and the containers via a vNIC on the host.

### **Macvlan Networking**
A macvlan network uses the same network as the host.

### **Overlay Networking**
Overlay networks give you the ablilty to span multiple hosts into one logical network, which allow container to communicate across hosts.

## Listing current networks

```
docker network ls
```
Example:
```
PSCore C:\src> docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
c7f4e0611cac        bridge              bridge              local
575c4777b9e2        host                host                local
a60b3ffda7f5        none                null                local
```

## Creating a new bridge network

```
docker network create --driver bridge [network name]
```
Example:
```
PSCore C:\src> docker network create --driver bridge my-bridge-network
5c8a95c15a1e26f1e032e6deca830947d74dc46465261715ab4bceabaee03816
PSCore C:\src> docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
ebfb84be617e        bridge              bridge              local
575c4777b9e2        host                host                local
5c8a95c15a1e        my-bridge-network   bridge              local
a60b3ffda7f5        none                null                local
```

## View the properties of a network

```
docker inspect [network name]
```
Example:
```
PSCore C:\src> docker inspect my-bridge-network
[
    {
        "Name": "my-bridge-network",
        "Id": "5c8a95c15a1e26f1e032e6deca830947d74dc46465261715ab4bceabaee03816",
        "Created": "2020-06-20T13:06:00.064863376Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
```

## Running a container on a specific network

```
docker run -dit --name test03 --network [network name] [image] [command]
```

Example:
```
PSCore C:\src> docker run -dit --name test03 --network my-bridge-network ubuntu bash
094c4c298ee0c7466f5721f9f8ee64564aaf9e0ece0a3cad9d104720618879d0
PSCore C:\src> docker inspect my-bridge-network
        "Containers": {
            "094c4c298ee0c7466f5721f9f8ee64564aaf9e0ece0a3cad9d104720618879d0": {
                "Name": "test03",
                "EndpointID": "be258a2736d313ee624819be971010dfb5d9f9fc4af1c2099ec0292c75b49027",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
```
## Creating a macvlan network
```
docker network create macvlan --subnet=172.20.2.0/24 --gateway=172.20.2.1 -o parent=eth0 macnet32
```
Example:

In this example, I didn't specify the host's ethernet adapter.

```
PSCore C:\src> docker network create macvlan --subnet=172.20.2.0/24 --gateway=172.20.2.1
9c24d15dc543c104b170bfd427eeddfd558478bea2631f9eef476547f57963e8
PSCore C:\src> docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
ebfb84be617e        bridge              bridge              local
575c4777b9e2        host                host                local
9c24d15dc543        macvlan             bridge              local
5c8a95c15a1e        my-bridge-network   bridge              local
a60b3ffda7f5        none                null                local
PSCore C:\src> docker inspect macvlan
[
    {
        "Name": "macvlan",
        "Id": "9c24d15dc543c104b170bfd427eeddfd558478bea2631f9eef476547f57963e8",
        "Created": "2020-06-20T13:29:41.533694934Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.20.2.0/24",
                    "Gateway": "172.20.2.1"
```

## Removing a network

```
docker network rm [network name]
```

