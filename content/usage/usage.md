---
title: Use with Docker(Recommended)
type: docs
bookToc: false
weight: 20
---

# Usage with Docker(Recommended)

We've build docker images on [hub.docker.com](https://hub.docker.com/r/webpsh/webp-server-go) and [ghcr.io](https://github.com/webp-sh/webp_server_go/pkgs/container/webp_server_go). If you want to run `webp-server` insider docker container without using `docker-compose.yml` and without limiting the resources it will use, you can run the command below:

DockerHub
```shell
docker run -d -p 3333:3333 -v /path/to/pics:/opt/pics --name webp-server webpsh/webp-server-go
```
ghcr.io
```shell
docker run -d -p 3333:3333 -v /path/to/pics:/opt/pics --name webp-server ghcr.io/webp-sh/webp_server_go
```

The path `path/to/pics` is your images serving in local. The path `/opt/pics` is currently unable modify because it's defined in the `config.json` while building the docker image.

The cache folder `EXHAUST_PATH` 's default is defined in `/opt/exhaust`, you can also mount the local directory for the cache folder by using `-v /path/to/exhaust:/opt/exhaust` option.

You can find more about configuration in [Configuration](CONFIGURATION.md) page.

`docker-compose.yml` example file:

{{< tabs "dockercompose" >}}
{{< tab "AMD64" >}}

```yml
version: '3'

services:
  webp:
    image: webpsh/webp-server-go
    # image: ghcr.io/webp-sh/webp_server_go
    restart: always
    environment:
      - MALLOC_ARENA_MAX=1
      # - LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2
      # - LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libtcmalloc_minimal.so.4.5.6
    volumes:
      - /path/to/pics:/opt/pics
      - ./exhaust:/opt/exhaust
      - ./metadata:/opt/metadata
    ports:
      -  127.0.0.1:3333:3333
```

{{< /tab >}}
{{< tab "ARM64" >}} 

```yml
version: '3'

services:
  webp:
    image: webpsh/webp-server-go
    # image: ghcr.io/webp-sh/webp_server_go
    restart: always
    environment:
      - MALLOC_ARENA_MAX=1
      # - LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2
      # - LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libtcmalloc_minimal.so.4.5.6
    volumes:
      - /path/to/pics:/opt/pics
      - ./exhaust:/opt/exhaust
      - ./metadata:/opt/metadata
    ports:
      -  127.0.0.1:3333:3333
```

{{< /tab >}}
{{< /tabs >}}

{{< hint "info" >}}
Using `LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2` will use `jemalloc` for malloc, can reduce RAM usage, related discussion: https://github.com/webp-sh/webp_server_go/issues/198

Using `- LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libtcmalloc_minimal.so.4.5.6` will use `tcmalloc` for malloc.
{{< /hint >}}

Start the container using:

```
docker-compose up -d
```

Now the server should be running on `127.0.0.1:3333`, visiting `http://127.0.0.1:3333/some-images/tsuki.jpg` will see the optimized version of `/path/to/pics/some-images/tsuki.jpg`, you can now add reverse proxy to make it public, for example, let Nginx to `proxy_pass http://127.0.0.1:3333/;`, and your WebP Server is on-the-fly!

## Custom config

If you'd like to use a customized `config.json`, you can follow [Configuration](CONFIGURATION.md) page to genereate one, and mount it into the container's `/etc/config.json`, example `docker-compose.yml` as follows:

```yml
version: '3'

services:
  webp:
    image: webpsh/webp-server-go
    # image: ghcr.io/webp-sh/webp_server_go
    restart: always
    environment:
      - MALLOC_ARENA_MAX=1
      # - LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2
      # - LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libtcmalloc_minimal.so.4.5.6
    volumes:
      - /path/to/pics:/opt/pics
      - ./exhaust:/opt/exhaust
      - ./metadata:/opt/metadata
      - ./config.json:/etc/config.json
    ports:
      -  127.0.0.1:3333:3333
```