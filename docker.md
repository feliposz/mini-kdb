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
