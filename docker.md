---
title: Docker
---

## Basic usage

Testing:

`docker run hello-world`

## Docker Machine (Docker Toolbox)

Check IP for docker machine.

> When using Docker Toolbox, container ports are not accessible on localhost but on the docker machine IP address.

`docker-machine ip`

## Errors

Fix for error `Error response from daemon: Get https://registry-1.docker.io/v2/: dial tcp: lookup registry-1.docker.io on 10.0.2.3:53: read udp 10.0.2.15:44243->10.0.2.3:53: i/o timeout`

```
docker-machine restart default
eval $(docker-machine env default)
```

---

## Outros

Listar imagens disponíveis localmente:

    docker images

~~~
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              113a43faa138        2 weeks ago         81.2MB
nginx               latest              cd5239a0906a        2 weeks ago         109MB
hello-world         latest              e38bc07ac18e        2 months ago        1.85kB
~~~

Listar imagens disponíveis para baixar:

    docker search ubuntu

Baixar imagem para repositório local:

    docker pull ubuntu

Executar imagem:

    docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Exemplos:

Executa uma imagem simples de teste, para verificar se está tudo ok:
      
    docker run hello-world

Executa uma imagem "ubuntu" e cai diretamente no prompt do shell (bash):

    docker run -it ubuntu bash

    -i, --interactive                    Keep STDIN open even if not attached
    -t, --tty                            Allocate a pseudo-TTY

Executa um servidor web (nginx):
      
    docker run --detach --publish 80:80 --name webserver nginx

    -d, --detach                         Run container in background and print container ID
    -p, --publish list                   Publish a container's port(s) to the host
        --name string                    Assign a name to the container


Lista containers em execução:

    docker ps
    docker container ls

Testing, basic usage:

    docker run docker/whalesay cowsay "Hey Team! 👋"

Running a specific postgres DB version with password (db user: "postgres"):

    docker run -e POSTGRES_PASSWORD=foobarbaz -p 5432:5432 postgres:15.1-alpine

Interactive mode:

    docker run --interactive --tty --rm ubuntu:20.04
    docker run -it --rm ubuntu:20.04

Named container:

    docker run -it --name my-ubuntu-container ubuntu:20.04

    docker start my-ubuntu-container
    docker attach my-ubuntu-container

Build an image (at prompt, no dockerfile)

    docker build --tag my-ubuntu-image -<<EOF
    FROM ubuntu:20.04
    RUN apt update && apt install iputils-ping --yes
    EOF

    docker run -it --rm my-ubuntu-image

Containers (list, inspect, remove):

    docker container ls -a
    docker container inspect my-ubuntu-container | jq
    docker container rm my-ubuntu-container

Images (list, inspect, remove, etc):

    docker image ls
    docker image history my-ubuntu-image
    docker image rm my-ubuntu-image
    docker image prune


Volume mount (more efficient):

    docker volume create my-volume
    docker run -it --rm -v my-volume:/my-data ubuntu:20.04

Bind mount (local filesystem, less efficient, more convenient for development):

    docker run -it --rm -v d:\users\felip\my-data:/my-data ubuntu:20.04

## Windows / WSL

Location for images:

    \\wsl$\docker-desktop-data\data\docker\image\overlay2

Location for volumes:

    \\wsl$\docker-desktop-data\data\docker\volumes

Actual VM filesystem:

    C:\Users\YOUR_USER_NAME\AppData\Local\Docker\wsl\data\ext4.vhdx

## Postgres

Using image with a given volume name:

    docker run -d --rm \
      -v pgdata:/var/lib/postgresql/data \
      -e POSTGRES_PASSWORD=foobarbaz \
      -p 5432:5432 \
      postgres:15.1-alpine  
  
Using existing data and using specific version  
  
    docker run -d --rm \
      -v //d/Users/felip/pgsql/data:/var/lib/postgresql/data \
      -v //d/Users/felip/pgsql/postgresql.conf:/var/lib/postgresql/data/postgresql.conf \
      -e POSTGRES_PASSWORD=postgres \
      -p 5432:5432 \
      postgres:14-alpine

Client:

    psql -U felip postgres


## Redis


    docker run -d --rm -v redisdata:/data redis:7.0.8-alpine
  
  
  
## MongoDB  

Using a volume and some version:
  
    docker run -d --rm \
      -v mongodata:/data/db \
      -e MONGO_INITDB_ROOT_USERNAME=root \
      -e MONGO_INITDB_ROOT_PASSWORD=foobarbaz \
      -p 27017:27017 \
      mongo:6.0.4

  
Using existing local data:  
  
> *NOT WORKING PROPERLY!!!*

    docker run -d --rm \
      -v //v/Portable/Programas/mongodb/log:/log \
      -v //v/Portable/Programas/mongodb/data:/data \
      -v //v/Portable/Programas/mongodb/mongodb.config.docker:/etc/mongod.conf \
      -e MONGO_INITDB_ROOT_USERNAME=root \
      -e MONGO_INITDB_ROOT_PASSWORD=foobarbaz \
      -p 27017:27017 \
      mongo:4.0.5 --config /etc/mongod.conf


## MySQL

Using a volume and some version with local files:

    docker run -d --rm \
      -v "V:\Portable\EasyPHP-Devserver-17\eds-binaries\dbserver\mysql5717x86x200905214542\data":/var/lib/mysql \
      -e MYSQL_ROOT_PASSWORD=foobarbaz \
      mysql:5.7.17
  

# Interactive Test Environments

## Operating systems

    # https://hub.docker.com/_/ubuntu
    docker run -it --rm ubuntu:20.04

    # https://hub.docker.com/_/debian
    docker run -it --rm debian:bullseye-slim

    # https://hub.docker.com/_/alpine
    docker run -it --rm alpine:3.17.1

    # https://hub.docker.com/_/busybox
    # small image with lots of useful utilities
    docker run -it --rm busybox:1.36.0

## Programming runtimes:

    # https://hub.docker.com/_/python
    docker run -it --rm python:3.11.1

    # https://hub.docker.com/_/node
    docker run -it --rm node:18.13.0

    # https://hub.docker.com/_/php
    docker run -it --rm php:8.1

    # https://hub.docker.com/_/ruby
    docker run -it --rm ruby:alpine3.17
    
## CLI Utilities

    #jq (json command line utility)
    #https://hub.docker.com/r/stedolan/jq
    docker run -i stedolan/jq <sample-data/test.json '.key_1 + .key_2'

    #yq (yaml command line utility)
    #https://hub.docker.com/r/mikefarah/yq
    docker run -i mikefarah/yq <sample-data/test.yaml '.key_1 + .key_2'

    # sed
    # GNU sed behaves differently from the default MacOS version for certain edge cases.
    docker run -i --rm busybox:1.36.0 sed 's/file./file!/g' <sample-data/test.txt

    # base64
    # GNU base64 behaves differently from the default MacOS version for certain edge cases.
    # Pipe input from previous command
    echo "This string is just long enough to trigger a line break in GNU base64." | docker run -i --rm busybox:1.36.0 base64

    # Read input from file
    docker run -i --rm busybox:1.36.0 base64 </sample-data/test.txt

    #Amazon Web Services CLI
    # https://hub.docker.com/r/amazon/aws-cli
    # Bind mount the credentials into the container
    docker run --rm -v ~/.aws:/root/.aws amazon/aws-cli:2.9.18 s3 ls
    
    # Google Cloud Platform CLI
    # Bind mount the credentials into the container
    docker run --rm -v ~/.config/gcloud:/root/.config/gcloud gcr.io/google.com/cloudsdktool/google-cloud-cli:415.0.0 gsutil ls
    # Why is the container image so big 😭?! 2.8GB
