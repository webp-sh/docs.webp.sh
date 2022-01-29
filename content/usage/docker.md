# Docker

We've build docker images on [hub.docker.com](https://hub.docker.com/r/webpsh/webp-server-go). If you want to run `webp-server` insider docker container, you can run the command below,
```shell
docker run -d -p 3333:3333 -v /path/to/pics:/opt/pics --name webps webpsh/webps
```
The path `path/to/pics` is your images serving in local. The path `/opt/pics` is currently unable modify because it's defined in the `config.json` while building the docker image.

The cache folder `EXHAUST_PATH` 's default is defined in `/opt/exhaust` , you can also mount the local directory for the cache folder by using `-v /path/to/exhaust:/opt/exhaust` option.

`docker-compose.yml` example file:

```yml
version: '3'

services:
  webp:
    image: webpsh/webp-server-go
    restart: always
    volumes:
      - ./path/to/pics:/opt/pics
      - ./path/to/exhaust:/opt/exhaust
    ports:
      -  127.0.0.1:3333:3333
    deploy:
      resources:
        limits:
          memory: 200M
```

Use with `docker-compose --compatibility up -d`.