---
title: Configuration
type: docs
bookToc: false
weight: 24
---

# Configuration

WebP Server Go can be configured by the following ways:

- `config.json` file
- Environment variables

Priority: Environment variables > `config.json` file.

## Full configuration

This is the full configuration with default values of WebP Server Go, you can copy it to `config.json` and modify it to your needs.

You can use environment variables to override the configuration in `config.json`, the environment variables are added by using prefix `WEBP_` and uppercase the field name, for example, `HOST` will be `WEBP_HOST`, `IMG_PATH` will be `WEBP_IMG_PATH`.

`WEBP_ALLOWED_TYPES` are separated by `,`, for example, `WEBP_ALLOWED_TYPES=jpg,png,webp`.

```json
{
  "HOST": "127.0.0.1",
  "PORT": "3333",
  "IMG_PATH": "./pics",
  "EXHAUST_PATH": "./exhaust",
  "IMG_MAP": {
    "/2": "./pics2",
    "/3": "./pics3",
    "http://www.example.com": "https://docs.webp.sh"
  },
  "ALLOWED_TYPES": ["jpg", "png", "jpeg", "bmp", "gif", "svg", "heic", "nef"],
  "ENABLE_AVIF": false,
  "ENABLE_EXTRA_PARAMS": false,
  "READ_BUFFER_SIZE": 4096,
  "CONCURRENCY": 262144,
  "DISABLE_KEEPALIVE": false
}
```

Description of each field:

| Field                 | Environment variables      | Type   | Description                                                                                                                                                                                                                                        |
| --------------------- | -------------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `HOST`                | `WEBP_HOST`                | string | Host to listen on                                                                                                                                                                                                                                  |
| `PORT`                | `WEBP_PORT`                | string | Port to listen on                                                                                                                                                                                                                                  |
| `IMG_PATH`            | `WEBP_IMG_PATH`            | string | Path to the image directory(of original images), if you'd like to use a remote backend(such as external Nginx served static site, Aliyun OSS or Tencent COS), please refer to [Remote Backend](REMOTE_BACKEND.md).                                                                                                                                                                                                    |
| `EXHAUST_PATH`        | `WEBP_EXHAUST_PATH`        | string | Path to the cache directory(of WebP images), for example, with `EXHAUST_PATH` set to `/var/cache/webp`, your `webp` image will be saved at `/var/cache/webp/pics/tsuki.jpg.1582558990.webp`.                                                                                                                                                                                                       |
| `IMG_MAP`             | /                          | dict   | Map of URI/Host to image, if this is present then `IMG_PATH` and `EXHAUST_PATH` will be ignored, see more on [MultiPath](http://localhost:1313/usage/multipath/) page                                                                              |
| `ALLOWED_TYPES`       | `WEBP_ALLOW_TYPES`         | list   | List of allowed image types                                                                                                                                                                                                                        |
| `ENABLE_AVIF`         | `WEBP_ENABLE_AVIF`         | bool   | Enable AVIF support,it’s disabled by default as converting images to AVIF is CPU consuming.                                                                                                                                                        |
| `ENABLE_EXTRA_PARAMS` | `WEBP_ENABLE_EXTRA_PARAMS` | bool   | Means whether to enable Extra Parameters, basically it allows you to do some transform on images like `https://img.webp.sh/path/tsuki.jpg?width=20`, you can find more info on [Extra Parameters](http://localhost:1313/usage/extra-params/) page. |
| `READ_BUFFER_SIZE`    | `WEBP_READ_BUFFER_SIZE`    | string | per-connection buffer size for requests’ reading. This also limits the maximum header size. Increase this buffer if your clients send multi-KB RequestURIs and/or multi-KB headers (for example, BIG cookies).                                     |
| `CONCURRENCY`         | `WEBP_CONCURRENCY`         | string | Maximum number of concurrent connections                                                                                                                                                                                                           |
| `DISABLE_KEEPALIVE`   | `WEBP_DISABLE_KEEPALIVE`   | string | Disable keep-alive connections, the server will close incoming connections after sending the first response to the client                                                                                                                          |

Suppose your website and image has the following pattern.

| Image Path                            | Website Path                         |
| ------------------------------------- | ------------------------------------ |
| `/var/www/img.webp.sh/path/tsuki.jpg` | `https://img.webp.sh/path/tsuki.jpg` |

Then

- `./pics` should be changed to `/var/www/img.webp.sh`
- `./exhaust` is cache folder for output images, by default it will be in `exhaust` directory alongside with `docker-compose.yml` file, if you'd like to keep cached images in another folder as , you can change `./exhaust` to `/some/other/path/to/exhaust`
