# Basic Usage

> Note: There is a potential memory leak problem with this server and remains unsolved, we recommend using Docker to mitigate this problem, refer to [Docker](./DOCKER.html).
> Related discussion: https://github.com/webp-sh/webp_server_go/issues/75

## Download or build the binary
Download the `webp-server-go` from [release](https://github.com/webp-sh/webp_server_go/releases) page.

## Dump config file

```
./webp-server -dump-config > config.json
```

The default `config.json` may look like this.
```json
{
  "HOST": "127.0.0.1",
  "PORT": "3333",
  "QUALITY": "80",
  "IMG_PATH": "/path/to/pics",
  "EXHAUST_PATH": "/path/to/exhaust",
  "ALLOWED_TYPES": ["jpg","png","jpeg","bmp"],
  "ENABLE_AVIF": false
}
```

> AVIF support is disabled by default as converting images to AVIF is CPU consuming.


### Config Example

In the following example, the image path and website URL.

| Image Path                            | Website Path                         |
| ------------------------------------- | ------------------------------------ |
| `/var/www/img.webp.sh/path/tsuki.jpg` | `https://img.webp.sh/path/tsuki.jpg` |

The `config.json` should be like:

| IMG_PATH               |
| ---------------------- |
| `/var/www/img.webp.sh` |

`EXHAUST_PATH` is cache folder for output `webp` images, with `EXHAUST_PATH` set to `/var/cache/webp` 
in the example above, your `webp` image will be saved at `/var/cache/webp/pics/tsuki.jpg.1582558990.webp`.

If you'd like to use a remote backend(such as external Nginx served static site, Aliyun OSS or Tencent COS), please refer to [Remote Backend](REMOTE_BACKEND.md).

## Run

```sh
./webp-server --config=/path/to/config.json
```
To keep this program running, refer to [Supervisor](SUPERVISOR.md) section.

## Nginx Example

We should only allow images to send to WebP Server Go, other extensions should just send the original file.

```
location ~* \.(?:jpg|jpeg|gif|png)$ {
    proxy_pass http://127.0.0.1:3333;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_hide_header X-Powered-By;
    proxy_set_header HOST $http_host;
    add_header Cache-Control 'no-store, no-cache, must-revalidate, proxy-revalidate, max-age=0';
}
```

If you use Caddy, you may refer to [优雅的让 Halo 支持 webp 图片输出](https://halo.run/archives/halo-and-webp).

If there is a CDN in front of your website, please refer to [Use with CDN](CDN.md)
