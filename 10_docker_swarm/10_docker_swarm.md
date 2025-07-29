
# Zero downtime deployment

we will use docker swarm

docker swarm uses multiple machines, and it will automatically see how much resoureses server have and will auto scale it

Downside
    - Autoscaling is not there but can be done useing workarounds custom scripts

---

### Docker Swarm

- It have 2 parts
    - Control Plane
        - it have multiple (optional) managers
        - all of managers are deployed on diff machines
        - each machine have its own docker engine
        - manager is also a node
        - it will also run container in manager machine as well
        - it will manage worker nodes

        
    - Data Plane
        - workers node
        - multiple machines
        - each machine have its own docker engine
        - 

---

Example
    Raft consensus group
    It have internal distributed state store
        - 3 managers
    Gossip Network    
        - 4 worker machines

---
### Raft consensus group

Raft is an algorithim , it is used to decide which server are managers, this decision is take by consensus (when all are agree)

- Not only used in docker, but others as well like in kafka as well
- this algo uses debounce whenever a heart beat is recived it resets the time

- It will help to decide which is the leader
- There will be only 1 leader and other will be followers
- Leader will decide which machine will have how many workers, and store the info for the same and data will be in sync with there followers

how election happen (how leader will be selected)
    - when majority is achived
        - (n/2) + 1
        - The above formual is also call `Quorum`
        - similar concept is also used in kafka

- only talking about manager nodes
- manager will start a timer random from range (timer range is 150 to 300 ms)
- leader will continuesly send heartbeat at fixed interval
- timmer will get reset everytie heartbeat is recived
- when heartbeat is not recived and if random timer runs out of time and get timesout that manager node will become candidate and will start election 
- when election starts it will increate the term count 
- it will cast vote to itslef and then ask other available nodes to ask for votes
- who ever asks for vote first other managers will give the vote to that node
- as managers were 3 , where 1 node was down and majority is 2
- as soon as majority is recived it manager node will get promoted to leader
- others will become followers now
- first node who gets timeout will wins the race and will become the leader

- when the old manager nodes get online , it will get the headbeat of the new leader node and it will resent its timer, and hearbeat also provides term count and will get to know that term has increased and will wait to get leader assigned by above mentioned process

- this algo is used to achive high availablity 

---
Q how many managers should be there
 - we try number of managers should be odd 
 - 1 is not good as it may fail and dont provide backup
 - 2 nodes will cause problem to achiving the majority
 - odd number help to get majority
 - if we want high failure tolerance we should have high numbers of managers

Failure tolerance - means how many manager nodes we can lose


| No of managers | (n- Quorumn) <br> Failure tolerance  | (n+2)+1 <br> Quorum | Remarks |
|---|---|---|---|
|1|0|1||
|2|0|2||
|3|1|2|recommended for small application|
|4|1|3||
|5|2|3|recommended for medium application|
|6|2|4||
|7|3|4||


---

Demo 

1 manager
    - asburn us (location)
2 workers
    - singapore (location)
    - Falkenstein (location)


---
Docker swarm is used for deployment not for development
so we dont need code , we just need the code
- swarm does not support `depends on`
    - so we will use wait using custom sh file 



- we will create and push docker image to docker hub

`docker build --no-cache --platform linuix/amd64 -t codersgyan/blog-app:v1`
- as we are building  image so we should specify the build cretaed for
- codersgyan is the username of docker (signed in using this user)
- `-no-cache` is used to make sure new image is created without usung cache data

- `docker login -u codersgyan` - login using codersgyan username

---


all 3 machines should have docker installed 


Manager
- `docker swarm init`
- this will give a output with token 
- they uses port so it should be open from firewall
- port `2377` TCP - used for docker
- port `3001` TCP- so that applocation can be seen
- port `4769` - UDP -  used by overlay network 
- port `7946` both TCP and UDP - used for network discover 

`docker info` - check info can see ismanager, how many nodes are there etc

`docker swarm join-token worker` - command to let worker join the manager

`docker swarm join-token manager` - command to get token to connect as manager


`docker network ls` - get list of network
    - you can see one bridge network created 
    - you can also find overlay network as well

`docker node ls` - to see list of nodes with details that who is the leader


worker 1
    - we got command from manager with token use that to run 
    

worker 2
    - we got command from manager with token use that to run 
    

Manager 
- create a blog folder
- create and copy docker compose file in the folder

`docker stack deploy -c compose.yml blogstack`
 - command to start stack

- `docker stack ls`
get list of stacks

- `docker service ls` - see running services

-`docker service ls` - to see list of services running and see where it is running

- `docker service ps blogstack_blog-server` - get details of running service based on its name
- give node details as well

- `docker service ps blogstack_blog-db` - get details of running service based on its name
- give node details as well


- will see if application up and running by visiting managers ip address on browser
- even we can try visiting worker ip 

- we can define where to start a particualr service in docker compose file 

-  `docker node update --label-add signapore` - to assign a lable

----
# overlay network

- `docker network ls`

- `docker network inspect ingres` 
    - overlay network creates a hidden container
    - ignress means incoming
    - this container act as proxy 
    - overlay create a tunnel using vx-lan , uses udp, 
    - this uses `4789` port
    - `vxlan` act as a virual switch


---
# scaling

Manager

- `docker service scale blogtsack-blog-server=10` - scale to 10 containers


- `docker service scale blogtsack-blog-server=3` - scale down to 3 containers

`docker service ps <service name>` - to see where are nodes deployed


---

### zero downtime deployment

let say there is a new image with latest changes

we have also pushed the latest image


Manager

- edit compose yaml
    - update the version to v2

`docker service update --force blogstack_blog_server`

`docker stack deploy -c compose.yml blogstack` - correct command

---

autocanon is the library using which we can load test

`npx autocanon -c 25 -d 120 http://<ip address>:<port>`

- `- c 25` is to define number of concurrent users
- `- d 120` is used for duration
- above command will hit 25 users for 120 seconds







