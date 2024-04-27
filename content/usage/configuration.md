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

This is the full configuration with default values of WebP Server Go(except the `IMG_MAP` part), you can copy it to `config.json` and modify it to your needs, or if you just want to change some of the values, you can use environment variables to override the specific values.

You can use environment variables to override the configuration in `config.json`, the environment variables are added by using prefix `WEBP_` and uppercase the field name, for example, `HOST` will be `WEBP_HOST`, `IMG_PATH` will be `WEBP_IMG_PATH`.

`WEBP_ALLOWED_TYPES` are separated by `,`, for example, `WEBP_ALLOWED_TYPES=jpg,png,webp`.
`WEBP_CONVERT_TYPES` are separated by `,`, for example, `WEBP_CONVERT_TYPES=webp,avif,jxl`.


```json
{
  "HOST": "0.0.0.0",
  "PORT": "3333",
  "QUALITY": "80",
  "IMG_PATH": "./pics",
  "EXHAUST_PATH": "./exhaust",
  "IMG_MAP": {},
  "ALLOWED_TYPES": ["jpg", "png", "jpeg", "bmp", "gif", "svg", "heic", "nef", "webp"],
  "CONVERT_TYPES": ["webp"],
  "STRIP_METADATA": true,
  "ENABLE_EXTRA_PARAMS": false,
  "EXTRA_PARAMS_CROP_INTERESTING": "InterestingAttention",
  "READ_BUFFER_SIZE": 4096,
  "CONCURRENCY": 262144,
  "DISABLE_KEEPALIVE": false,
  "CACHE_TTL": 259200
}
```

The `IMG_MAP` example as below:

```
"IMG_MAP": {
  "/2": "./pics2",
  "/3": "./pics3",
  "http://www.example.com": "https://docs.webp.sh"
}
```

Description of each field:

| Field                 | Environment variables      | Type   | Description                                                                                                                                                                                                                   |
| --------------------- | -------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `HOST`                | `WEBP_HOST`                | string | Host to listen on                                                                                                                                                                                                             |
| `PORT`                | `WEBP_PORT`                | string | Port to listen on                                                                                                                                                                                                             |
| `QUALITY`             | `WEBP_QUALITY`             | string | Quality of image, from 0 to 100, 100 means lossless conversion.                                                                                                                                                               |
| `IMG_PATH`            | `WEBP_IMG_PATH`            | string | Path to the image directory(of original images), if you'd like to use a remote backend(such as external Nginx served static site, Aliyun OSS or Tencent COS), please refer to [Remote Backend](REMOTE_BACKEND.md).            |
| `EXHAUST_PATH`        | `WEBP_EXHAUST_PATH`        | string | Path to the cache directory(of WebP images), for example, with `EXHAUST_PATH` set to `/var/cache/webp`, your `webp` image will be saved at `/var/cache/webp/pics/tsuki.jpg.1582558990.webp`.                                  |
| `IMG_MAP`             | /                          | dict   | Map of URI/Host to image, if this is present then `IMG_PATH` and `EXHAUST_PATH` will be ignored, see more on [MultiPath](/usage/multipath/) page                                                         |
| `ALLOWED_TYPES`       | `WEBP_ALLOWED_TYPES`       | list   | List of allowed image types                                                                                                                                                                                                   |
| `CONVERT_TYPES`         | `WEBP_CONVERT_TYPES`         | string | The image types list that WebP Server will try to convert to, default is `["webp"]` which means it will only try to convert image to WebP, available options: `["webp","avif","jxl"]`. |
| `STRIP_METADATA` | `WEBP_STRIP_METADATA` | bool | Whether to Strip EXIF metadata from images. |
| `ENABLE_EXTRA_PARAMS` | `WEBP_ENABLE_EXTRA_PARAMS` | bool   | Means whether to enable Extra Parameters, basically it allows you to do some transform on images like `https://img.webp.sh/path/tsuki.jpg?width=20`, you can find more info on [Extra Parameters](/usage/extra-params/) page. |
| `EXTRA_PARAMS_CROP_INTERESTING` | `WEBP_EXTRA_PARAMS_CROP_INTERESTING` | string | Defines how should WebP Server crop image when `ENABLE_EXTRA_PARAMS` is on and image requests contains both `width` and `height`, available options are: "InterestingNone", "InterestingEntropy", "InterestingCentre", "InterestingAttention", "InterestringLow", "InterestingHigh", "InterestingAll", you can find more info on [Extra Parameters](/usage/extra-params/) page. |
| `READ_BUFFER_SIZE`    | `WEBP_READ_BUFFER_SIZE`    | number | per-connection buffer size for requests’ reading. This also limits the maximum header size. Increase this buffer if your clients send multi-KB RequestURIs and/or multi-KB headers (for example, BIG cookies).                |
| `CONCURRENCY`         | `WEBP_CONCURRENCY`         | number | Maximum number of concurrent connections                                                                                                                                                                                      |
| `DISABLE_KEEPALIVE`   | `WEBP_DISABLE_KEEPALIVE`   | string | Disable keep-alive connections, the server will close incoming connections after sending the first response to the client                                                                                                     |
| `CACHE_TTL`   | `WEBP_CACHE_TTL`   | number | Cache TTL(minutes) for Remote Backends(Proxy Mode), we use `HEAD` request to get remote image info, so your backend needs to support `HEAD` request, after first successfully `HEAD` request, it will be cached for `CACHE_TTL` minutes, during that period, we will not send `HEAD` request again and use local cache for rendering. Setting this value to 0 means cache forever.          |

## Configuration example

Suppose your website and image has the following pattern:

| Image Path                            | Website Path                         |
| ------------------------------------- | ------------------------------------ |
| `/var/www/img.webp.sh/path/tsuki.jpg` | `https://img.webp.sh/path/tsuki.jpg` |

And you wish to enable [Extra Parameters](/usage/extra-params/), then

- `./pics` should be changed to `/var/www/img.webp.sh`
- `./exhaust` is cache folder for output images, by default it will be in `exhaust` directory alongside with `docker-compose.yml` file, if you'd like to keep cached images in another folder as , you can change `./exhaust` to `/some/other/path/to/exhaust`
- `ENABLE_EXTRA_PARAMS` can be passed in by environment variable `WEBP_ENABLE_EXTRA_PARAMS=true`

Your `docker-compose.yml` file can be written as follows:

```yaml
version: '3'

services:
  webp:
    image: webpsh/webp-server-go
    restart: always
    environment:
      - WEBP_ENABLE_EXTRA_PARAMS=true
    volumes:
      - /var/www/img.webp.sh:/opt/pics
      - ./exhaust:/opt/exhaust
      - ./metadata:/opt/metadata
    ports:
      - 127.0.0.1:3333:3333
```
