# Reverse Proxy Docker Container

docker-compose.yml file to create an [NGNIX](https://www.nginx.com/) based reverse proxy server that is using
a companion container to create SSL certifcates to ensure all services are HTTPS with
valid certificates.

This is using [nginx-proxy](https://github.com/jwilder/nginx-proxy)
and [docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion) Docker containers.

A Docker network needs to be setup to allow further docker container to see the network that is in use under this
docker-compose.yml

In this Docker Compose I have create a network __jscnetwork__ using the docker command line

`docker network create jscnetwork`

Another container that is to site behind the reverse proxy needs to have the network lines in its compose file
to link to the same network.  It also requires the expose command to note the port that it will receive.

```
version: '2'

services:
  whoami:
    image: jwilder/whoami
    environment:
      - VIRTUAL_HOST=www.mydomain.com
      - LETSENCRYPT_HOST=www.mydomain.com
      - LETSENCRYPT_EMAIL=email@mydomain.com
    expose:
      - "8000"
    networks:
      - jscnetwork

networks:
  jscnetwork:
    external: true
```