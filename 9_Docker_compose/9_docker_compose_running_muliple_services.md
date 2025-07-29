Repo Ref URL - `https://github.com/codersgyan/blog-app-docker-compose`

---

docker compose
    - it is used to start multiple containers at once 
    - we uses `.YAML` / `.yml` files
        - we uses indentation 
     
---

# DEMO

default file name is 
`compose.yml` - preffered
`docker-compose.yml` - backword capatable


---

```yaml

# This is for dev Env not prod   

# version: (obsoete)
name: blog-app  #if not give your folder(where docker file is present) name will become the name of the docker execution - this name will be used as prefix in all the networks, volumes etc , try it to be unique

services:
  blog-db:
    image: 'mysql'
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} # we dont give passwords for prod , it is not good practice, we are using env variable in this case
      # we can define env in shell using `export <variablename>=<value>`
    volumes:
      - blog-db-vol:/var/lib/mysql
    ports:
      - '3306:3306'
    networks:
      - blog-app-net

  blog-server:
    # in this case we are using existing dockerfile to create image
    # image: '' 
    build:
      dockerfile: Dockerfile
      context: "./" # filepath of dockerfile (useful when multiple dockerfile exists)
    container_name: blog-app-container
    ports:
      - '3001:3000'
    env_file:
      - ./.env
    volumes:
      - ./:/app # binding currenty folder with /app in docker - it is not volume, it is a bind mount
    depends_on:
      - blog-db
    networks:
      - blog-app-net
    
volumes:
# it is not list but key value pair (read docs for advance knowledge)
  blog-db-vol:

networks:
  blog-app-net:

```

command used to start
`docker compose up -d`
- `-d` is used for detached mode

we can self host docker registry like docker hub
we can also self host npm registry

#### Points to note

As we started docker compose file using above file
    - it create custom network with the name defined in the file i.e `blog-app_default`
        - it is of type bridge
    - Volume as `blog-app_blog-db-vol`
    

we can use `docker compose ps` - to see running docker compose files/ containers

`docker compose down` - to stop the container
    - when we turn down the compose, it also remove the network as well with containers

`docker-compose export MYSQL_ROOT_PASSWORD=root` - command to export variable in shell session which can be used by docker compose

`docker compose up -d` - to start again

we have health checks as well using which we can start another container if condition is met



---


outside -> server- nginx(433) -> container

3001:3000
<lapy>:<docker-port>

domain -> reverse proxy -> nginx




