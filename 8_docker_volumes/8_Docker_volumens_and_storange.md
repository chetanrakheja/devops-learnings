## Docker volumes

container with muliple ips
`sudo docker network connect <networkName> <containername>` - to add network to a container

---
containers are ephereral (temprary)


## Stateful services

Services which saves data 

--- 
when we stop container data remains there but when we remove the container data is also deleted


---
```bash
sudo docker run -d \
--name mydb \
-e MYSQL_ROOT_PASSWORD=root \
mysql
```
 - `-e` is used for enviroment variable
 - starting mysql image with root password as root
 - name of container in mydb

`sudo docker exec -it mybd bash` - login into mysql container
    `mysql -u root -p` - conect to db 
    `show databases` - view dbs in container
    `create database if not exisits blog;` - create new db blog
    `show databases` - view dbs in container
    `exit` - exit db connection not docker
    `cd /var/lib/mysql` - here all the db files are stored 
    `ls -la` see all the files in the folder
    `exit` - exit container

`sudo docker stop mydb` - stop container

--- 
check if database exists when we stop the db
`sudo docker start mydb` - start db 
`sudo docker exec -it mybd bash` - login into mysql container
    `mysql -u root -p` - conect to db 
    `show databases` - view dbs in container 
    **Data will exists**
    `exit` - exit db connection not docker
    `exit` - exit container

---

check if after deleting image data exists or not 

`sudo docker stop mydb` - stop container
`sudo docker rm mydb` - remove db image if --rm is not used while creating the image


`sudo docker run -d  --name mydb -e MYSQL_ROOT_PASSWORD=root mysql` - create db image again

`sudo docker exec -it mybd bash` - login into mysql container
    `mysql -u root -p` - conect to db 
    `show databases` - view dbs in container 
    **Blog database will not be present**
    `exit` - exit db connection not docker
    `exit` - exit container

`sudo docker stop mydb` - stop container
`sudo docker rm mydb` - remove db image if --rm is not used while creating the image


----

## Docker Volumes

docker volumes are like data buckets which are managed by docker and stored on host machine
- i.e when docker is running data is stored in your local machine 
- we can also share same data to multiple buckets 


`sudo docker volume ls` - to c heck volume details

```bash
sudo docker run -d \
--rm \
--name mydb \
-v mydb-vol:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
mysql
```
create mydb docker container with volume and env defined
    - `-v` to define volume `<volumeName>:<path in conatiner>`


`sudo docker exec -it mybd bash` - login into mysql container
    `mysql -u root -p` - conect to db 
    `show databases;` - view dbs in container
    `create database if not exisits blog;` - create new db blog
    `show databases` - view dbs in container
    `exit` - exit db connection not docker
    `exit` - exit container

`sudo docker stop mydb` - stop container - this will also delete container as we used `--rm`


`sudo docker run -d --rm --name mydb -v mydb-vol:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql` 
    - create mydb docker container with volume and env defined


`sudo docker exec -it mybd bash` - login into mysql container
    `mysql -u root -p` - conect to db 
    `show databases;` - view dbs in container
    **database will be present**


`/var/lib/docker/volumes` - at this location docker volumnes will be presnet (in case of unix docker installation check for mac/windows)

**THIS IS RECOMENDED WAY TO USE**

- this can be also backed up


---
`sudo docker run -d --name mydb -v mydb-vol:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql` 
    - create mydb docker container with volume and env defined
    - `/var/liv/docker/overlay2` - when docker container is stopped the data is stored in this folder (when volume is not defined)

`sudo docker ps -a` - to see stopped container as well


---

## Bind mounts

here we bind a file/folder with file/folder inside docker

like we need to provide config files we can keep that config files out of container and that can be access using bind mounts 





---
crete dev env for blog which uses express, sql db 

`sudo docker network create --driver bridge blognet` - create network
`sudo docker run -d --name blog-my-db -v blog-db-vol:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root --network blog-net mysql` 
    - create mydb docker container with volume, env defined and network

`sudo docker build -t blog-my-app-image .`
    - command to run dockerfile from current directory
    -`-t` is for tagging and give custom name `-t <name>:<version>` or `-t name` - this will create latest version by default
    `sudo docker image ls` - list images with us you can see blog-my-app-image present
    - `CMD` is the command which will executed when docker image is executed


```bash
sudo docker run \
-d \
--rm \
--name blog-my-app \
-p 3000:3000 \
blog-my-app-image
```
- create image of blog application

`sudo docker connect blog-net blog-my-app` - add custom network to the container (this will not remove existing network)


`sudp docker inspect blog-my-app` - inspect the image to verify network connected or not 

`sudo docker logs blog-my-app` - view docker logs of the image

`sudo docker exec -it blog-my-db bash` - connect to sql container
    
`show databases;`
`create database blog;`
`show databases;`
`use blog;`
`show tables;`
```sql
    create table if not exists  blogs (
    id int auto_increment primary key,
    title varchar(255),
    description text
    );
```
`show tables;`
```sql
inset into blogs (title,description)
values
('First blog post','This is the desc of the first blog post'),
('second blog post','This is the desc of the second blog post');
```
`select * from blogs;`

open `localhost:3000`

if we need to update the file, we made some changes in file but changes are not displayed so we need to restart the blog application

`sudo docker stop blog-my-app`

`sudo docker build -t blog-my-app-image .`

`sudo docker run -d --rm --name blog-my-app -p 3000:3000 blog-my-app-image`
`sudo docker connect blog-net blog-my-app`

this is very long process


`sudo docker run -d --rm --name blog-my-app -p 3000:3000 --network blog-net -v $(pwd):/app blog-my-app-image`
- command with pwd as mount
  
`tmp-fs` mount is there for in memory mount





























