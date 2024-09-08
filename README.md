# docker-networking
Docker Networking Stuff

Taken from NetworkChuck - Docker networking is CRAZY!!
https://youtu.be/bKFMS5C4CG0?si=Df6J9ZRZk4pHK1Te

## Commands

#### Docker network
```
docker network ls

NETWORK ID     NAME        DRIVER    SCOPE
4871c887007d   bridge      bridge    local
bb5c4a16fe34   host        host      local
2aff5e84d0f7   none        null      local
```

#### Docker run
```
docker run -itd --rm --name thor busybox
docker run -itd --rm --name mjolnir busybox
docker run -itd --rm --name stormbreaker nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
Status: Downloaded newer image for nginx:latest
```

#### Docker ps
```
docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS     NAMES
2dbd39703a70   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   80/tcp    stormbreaker
199ac648b515   busybox   "sh"                     2 minutes ago        Up 2 minutes                  mjolnir
03c77f5b00cb   busybox   "sh"                     3 minutes ago        Up 3 minutes                  thor
```

