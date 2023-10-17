---
title: Use with Docker
type: docs
bookToc: false
weight: 20
---

# Usage with Docker(recommended)

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

The cache folder `EXHAUST_PATH` 's default is defined in `/opt/exhaust` , you can also mount the local directory for the cache folder by using `-v /path/to/exhaust:/opt/exhaust` option.

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
      # - READ_BUFFER_SIZE=4096
      # - CONCURRENCY=262144
      # - DISABLE_KEEPALIVE=false
    volumes:
      - ./path/to/pics:/opt/pics
      - ./path/to/exhaust:/opt/exhaust
      - ./path/to/metadata:/opt/metadata
    ports:
      -  127.0.0.1:3333:3333
    deploy:
      resources:
        limits:
          memory: 400M
    memswap_limit: 400M
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
      # - READ_BUFFER_SIZE=4096
      # - CONCURRENCY=262144
      # - DISABLE_KEEPALIVE=false
    volumes:
      - ./path/to/pics:/opt/pics
      - ./path/to/exhaust:/opt/exhaust
      - ./path/to/metadata:/opt/metadata
    ports:
      -  127.0.0.1:3333:3333
    deploy:
      resources:
        limits:
          memory: 400M
    memswap_limit: 400M
```

{{< /tab >}}
{{< /tabs >}}



{{< hint "info" >}}
`memory` and `memswap_limit` will limit the RAM used by container will not exceed 400M and will use no swap, more info can be seen on https://docs.docker.com/config/containers/resource_constraints/#--memory-swap-details.
{{< /hint >}}

{{< hint "info" >}}
Using `LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2` will use `jemalloc` for malloc, can reduce RAM usage, related discussion: https://github.com/webp-sh/webp_server_go/issues/198

Using `- LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libtcmalloc_minimal.so.4.5.6` will use `tcmalloc` for malloc.
{{< /hint >}}

{{< hint "info" >}}
After version 0.9.12, we support some environment variables to control the behavior of WebP Server Go.

* `READ_BUFFER_SIZE`
  * per-connection buffer size for requests' reading. This also limits the maximum header size. Increase this buffer if your clients send multi-KB RequestURIs and/or multi-KB headers (for example, BIG cookies).
  * Especially useful when you have large cookies, related issue: https://github.com/webp-sh/webp_server_go/issues/280
  * Default: 4096
* `CONCURRENCY`
  * Maximum number of concurrent connections
  * Default: 262144
* `DISABLE_KEEPALIVE`
  * Disable keep-alive connections, the server will close incoming connections after sending the first response to the client
  * Default: false
{{< /hint >}}


Suppose your website and image has the following pattern.

| Image Path                            | Website Path                         |
| ------------------------------------- | ------------------------------------ |
| `/var/www/img.webp.sh/path/tsuki.jpg` | `https://img.webp.sh/path/tsuki.jpg` |

Then

* `./path/to/pics` should be changed to `/var/www/img.webp.sh`
* `./exhaust` is cache folder for output images, by default it will be in `exhaust` directory alongside with `docker-compose.yml` file, if you'd like to keep cached images in another folder as , you can change  `./exhaust` to `/some/other/path/to/exhaust`

Start the container using:

```
docker-compose up -d
```

Now the server should be running on `127.0.0.1:3333`, visiting `http://127.0.0.1:3333/path/tsuki.jpg` will see the optimized version of `/var/www/img.webp.sh/path/tsuki.jpg`, you can now add reverse proxy to make it public, for example, let Nginx to `proxy_pass http://127.0.0.1:3333/;`, and your WebP Server is on-the-fly!

## Custom config

If you'd like to use a customized `config.json`, you can follow the steps in [Usage with Binary](/usage/usage-with-binary/) to genereate one, and mount it into the container's `/etc/config.json`, example `docker-compose.yml` as follows:

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
      # - READ_BUFFER_SIZE=4096
      # - CONCURRENCY=262144
      # - DISABLE_KEEPALIVE=false
    volumes:
      - ./path/to/pics:/opt/pics
      - ./path/to/exhaust:/opt/exhaust
      - ./path/to/metadata:/opt/metadata
      - ./config.json:/etc/config.json
    ports:
      -  127.0.0.1:3333:3333
    deploy:
      resources:
        limits:
          memory: 400M
    memswap_limit: 400M
```