# Why we need containers

- coder code the app and the push to git and then it is pulled from git and deployed
- Difficulty 
    - Coder work on windows, but deployed on Linux
    - Different version in local and server
    - inconsisant Dev setup
- Solution - **Docker containers**

---

Docker CLI ->Hit API -> Docker Engine
Docker Enginer 
    -if image exists-> Run else
    - if image does not exists -> check in Docker Registry (docker hub)
    - Container d is used to load and run the image 
    - containerd calls runc
    - runc - linux Kernal

Docker CLI -> Docker Engine

Docker Enginer -> Containerd -> runc -> Linux Kernel

linux kernal have namespaces and cgroups which can be used to natively run the conatainers

runc have low level api which can call low level apis of linux kernel 

---
Docker is native for linux

Docker desktop create a linux VM which is used by docker to hit linux kernal
 

---

| Virtual Machince | Docker |
|---|---|
|Total OS is installed| full OS is not created, uses linux kernal|
|Slower |V fast|

---
`docker --version` - to know docker version


`docker pull <image name>` - this will pull image from docker hub (`docker pull hello-world`)

`docker run <image name>` - run the image

---  
`docker run ubuntu` - to run unbuntu container

`docker run -it ubuntu bash` - it will run ubuntu image in interactive mode and connect a bash shell
 
`docker ps` - to get running process

`docker stop <Image Id>` - to stop the running container   

---
docker enginer is made on golang




















