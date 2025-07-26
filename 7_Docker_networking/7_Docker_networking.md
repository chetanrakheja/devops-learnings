# Networking

mac address in pysical address and is static for a device


Router does the NAT (netork address translation)
 
APR (Address Reolutio Protocal)
Wifi and physical wire may have different mac address

switch check host and ip to know address to be searched locally or over the interner, uses subnet mask for the same


----
34.00
# Docker Networking

`ip route` - to know network details in linux

`sudo apt intall docker.io` - unofficial command to install docker

`ip addr show` - to see all the network interface , after docker installation new interface is installed named `docker0` (172.17.0.1) 
its a network bridge between computer and network


`sudo docker network ls` 
- this will give network list, bridge, host, none

bridge - it is a interface between system and docker

`sudo docker run -d --rm  --name nginx1 nginx`
`sudo docker run -d --rm  --name <Image custom Name> <image name>`
    d for detached
    rm - to remove when closed 
    name for giving custom name
    ip assigned - 172.17.0.2

`sudo docker ps` - command to get running docker containers


`ip addr show` (executed in local machine) - now a new interface is visisble which 

`sudo docker inspect <custom image name>` - this will give lot of details about that container / image, includig ip details

`sudo docker run -d --rm  --name nginx2 nginx` - run nginx container with nginx2 name
ip assigned - 172.17.0.3
`ip addr show` - check new virtual interface is created

---

if we need to access container 2 from container 1

`sudo docker exec -it nginx1 /bin/bash` - connect nginx1 container in interactive mode and use bash
    `apt update`
    `apt install iputils-ping -y` - install ping in container
    `ping 172.17.0.3`
    `ping nginx2` - you can aslo use container name to get the ip

we need switch to allow ping but as docker create bridge interface it act as a virtual switch 

docker also create dns so that we can ping based on image name as well

---

in prod we dont use default bridge network which is not good, secure

for the same  we use custom bridge type network instead of using default bridge network

`sudo docker stop nginx1 nginx2` - to stop both the conainers

`sudo docker ps` - to get runing conatienrs

`ip addr show` - to check ip interfaces
when we stop the containers virtual interface will also get stopped

---


```bash

sudo docker network create \
--driver bridge \
cg-network
```
- when we need to pass longer command we can use `\` to go to next line

`ip addr show` - now a new bridge is created
id is `br-<network-id>`
ip - 172.18.0.1
it will connect yo direct interface will not use default docker network interface

`sudo docker network ls` - we can see all networks with there id m name , driver , scope

---

`sudo docker run -d -rm --name my-nginx --network cg0network nginx` -  create nginx conatiner named my-nginx with network as cg-network


`sudo docker instpect my-nginx` - check ip details 

`sudo docker run -d --rm --name my-redis --network cg-network redis` - run redis conatiner with name my-redis, in cg-network

`sudo docker exec -it my-nginx /bin/bash` - connect to my-nginx image using bash
    `apt update` - update everything
    `apt install iputils-ping` - install ping in container
    `ping 172.18.0.3` - redis ip
    `ping my-redis`- redis name
    `exit`


`sudo docker inspect cg-network` - know details about network , it will include details of connected conatiner as well

---

now create image on default network and verify containers on default network is not able to connect to this network containers

`sudo docker run -d --rm --name outer-nginx nginx` - create a conatiner on default bridge

`sudo docker exec -it outer-nginx /bin/bash` - connect to my-nginx image using bash
    `apt update` - update everything
    `apt install iputils-ping` - install ping in container
    `ping 172.18.0.2` - pinging nginx of different network which is not running
    `exit`

---

pinging docker conatiner from host machine
`ping 172.18.0.1` - will work 
but if we open 172.18.0.1:80 site is not accessable

`sudo docker run -d --rm --name my-nginx --network -p 80:80 nginx`

    `-p 80:80` - `-p host:docker-image`

if we open 192.168.1.72:80 - now we can access site on host machine ip 

---

### host network
after closing all running conatiner

`sudo docker run -d --rm --name kula-nginx --network host nginx`

when we use host it dont create any new network but run on host network interface
    `ip addr show` - from container, you will get same details as local system

---

### overlay

When we run docker on mutiple machines as a cluster we use overlay so that docker containers think they are on same network but they can be on different machines

will be used when using docker swan

---

### mac vlan
when we use this our roter will think a new devices is connected to it 
















