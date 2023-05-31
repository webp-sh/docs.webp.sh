# Basic Usage

{{< hint "info" >}}
Note: There is a potential memory leak problem with this server and remains unsolved, we recommend using Docker to mitigate this problem, refer to [Docker](./DOCKER.html).
Related discussion: https://github.com/webp-sh/webp_server_go/issues/75
{{< /hint >}}

## Download or build the binary
Download the `webp-server-go` from [release](https://github.com/webp-sh/webp_server_go/releases) page.

## Install dependencies

### If you are using version after 0.6.0

Install `libvips` on your machine, more info [here](https://github.com/davidbyttow/govips)

{{< tabs "after" >}}
{{< tab "macOS" >}} Run `brew install vips pkg-config` {{< /tab >}}
{{< tab "Ubuntu" >}} Run `apt install libvips-dev` {{< /tab >}}
{{< /tabs >}}

### If you are using version before 0.6.0

If you'd like to run binary directly on your machine, you need to install `libaom`:

Without this library, you may encounter error like this: `libaom.so.3: cannot open shared object file: No such file or directory`

{{< tabs "before" >}}
{{< tab "Apple Silicon" >}} `brew install aom && export CPATH=/opt/homebrew/opt/aom/include/;LIBRARY_PATH=/opt/homebrew/opt/aom/lib/`, more references can be found at [在M1 Mac下开发WebP Server Go | 土豆不好吃](https://dmesg.app/m1-aom.html). {{< /tab >}}
{{< tab "Ubuntu" >}} Run `apt install libaom-dev` {{< /tab >}}
{{< tab "CentOS" >}} Run `yum install libaom-devel` {{< /tab >}}
{{< /tabs >}}

> If you don't like to hassle around with your system, so do us, why not have a try using Docker? >> [Docker | WebP Server Documentation](https://docs.webp.sh/usage/docker/)

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
  "ENABLE_AVIF": false,
  "ENABLE_EXTRA_PARAMS": false
}
```

> `ENABLE_AVIF` means AVIF support, it's disabled by default as converting images to AVIF is CPU consuming.
>
> `ENABLE_EXTRA_PARAMS` means whether to enable Extra Parameters, basically it allows you to do some transform on images like `https://img.webp.sh/path/tsuki.jpg?width=20`, you can find more info on [Extra Parameters](./extra-params.md) page.


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
