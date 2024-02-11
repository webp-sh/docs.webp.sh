---
title: Use with Binary(Advanced)
type: docs
bookToc: false
weight: 25
---

# Usage with Binary(Advanced)

{{< hint "info" >}}
Note: There is a potential memory leak problem with this server and remains unsolved, we recommend using Docker to mitigate this problem, refer to [Docker](./DOCKER.html).
Related discussion: https://github.com/webp-sh/webp_server_go/issues/75
{{< /hint >}}

## Download or build the binary
Download the `webp-server-go` from [release](https://github.com/webp-sh/webp_server_go/releases) page.

## Install dependencies

Install `libvips` on your machine, more info [here](https://github.com/davidbyttow/govips).

{{< tabs "after" >}}
{{< tab "macOS" >}} Run `brew install vips pkg-config` {{< /tab >}}
{{< tab "Ubuntu" >}} Run `apt install libvips-dev` {{< /tab >}}
{{< tab "Fedora" >}} Run `dnf install vips-devel` {{< /tab >}}
{{< /tabs >}}


> If you don't like to hassle around with your system, so do us, why not have a try using Docker? >> [Docker | WebP Server Documentation](https://docs.webp.sh/usage/docker/)

## Create config file

Please refer to [Configuration](CONFIGURATION.md) page.

## Run

```sh
./webp-server --config=/path/to/config.json
```
To keep this program running, refer to [Supervisor](SUPERVISOR.md) section.

## Nginx Example

We should only allow images to send to WebP Server Go, other extensions should just send the original file.

```
location ~* \.(?:jpg|jpeg|gif|png|svg|heic|bmp|nef|webp)$ {
    proxy_pass http://127.0.0.1:3333;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_hide_header X-Powered-By;
    proxy_set_header HOST $http_host;
    add_header Cache-Control 'no-store, no-cache, must-revalidate, proxy-revalidate, max-age=0';
}
```

If you use Caddy, you may refer to [优雅的让 Halo 支持 webp 图片输出](https://halo.run/archives/halo-and-webp).

If there is a CDN in front of your website, please refer to [Use with CDN](CDN.md)
