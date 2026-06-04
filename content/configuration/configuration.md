---
title: Configuration
type: docs
bookToc: false
weight: 24
---

# Configuration

> This is the unified documentation, please use the latest version of WebP Server Go if possible as some configuration might not be available in previous versions.

WebP Server Go can be configured by the following ways:

- `config.json` file
- Environment variables

Priority: Environment variables > `config.json` file.

## Configuration changelog

{{< hint "info" >}}

- Remote mode is detected automatically from `http(s)` URLs in `IMG_PATH` / `IMG_MAP`; `ProxyMode` has been removed.
- Security-related invalid paths and `meta=full` on missing resources now return `404` (see [Security](/reference/security/) and [Query parameters](/reference/query-parameters/)).
- `MAX_CACHE_SIZE` is added at version 0.12.0
- `EXTRA_PARAMS_CROP_INTERESTING` is added at version 0.11.2
- `STRIP_METADATA`, `CONVERT_TYPES` is added at version 0.11.0, dropped `ENABLE_AVIF` entry.
- `CACHE_TTL` is added at version 0.10.7
- `READ_BUFFER_SIZE`, `CONCURRENCY` and `DISABLE_KEEPALIVE` is added at version 0.9.12
  {{< /hint >}}

## Remote backend detection

WebP Server Go enables remote (origin-fetch) mode **automatically** when the resolved image source is an `http://` or `https://` URL:

- Set `IMG_PATH` to a remote base URL, or
- Map a URI prefix or `Host` to a remote URL in `IMG_MAP` (see [MultiPath](/configuration/multipath/) and [Remote Backend](/configuration/remote-backend/)).

There is no `ProxyMode` switch. Local mode is used for filesystem paths.

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
  "ALLOWED_TYPES": [
    "jpg",
    "png",
    "jpeg",
    "bmp",
    "gif",
    "svg",
    "heic",
    "nef",
    "webp"
  ],
  "CONVERT_TYPES": ["webp"],
  "STRIP_METADATA": true,
  "ENABLE_EXTRA_PARAMS": false,
  "EXTRA_PARAMS_CROP_INTERESTING": "InterestingAttention",
  "READ_BUFFER_SIZE": 4096,
  "CONCURRENCY": 262144,
  "DISABLE_KEEPALIVE": false,
  "CACHE_TTL": 259200,
  "MAX_CACHE_SIZE": 0
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

| Field                           | Environment variables                | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------- | ------------------------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `HOST`                          | `WEBP_HOST`                          | string | Host to listen on                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `PORT`                          | `WEBP_PORT`                          | string | Port to listen on                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `QUALITY`                       | `WEBP_QUALITY`                       | string | Quality of image, from 0 to 100, 100 means lossless conversion.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `IMG_PATH`                      | `WEBP_IMG_PATH`                      | string | Path to the local image directory, **or** an `http://` / `https://` base URL for a [Remote Backend](/configuration/remote-backend/). Remote mode is selected automatically when this value is a URL.                                                                                                                                                                                                                                                                                        |
| `EXHAUST_PATH`                  | `WEBP_EXHAUST_PATH`                  | string | Path to the cache directory(of WebP images), for example, with `EXHAUST_PATH` set to `/var/cache/webp`, your `webp` image will be saved at `/var/cache/webp/pics/tsuki.jpg.1582558990.webp`.                                                                                                                                                                                                                                                                                                |
| `IMG_MAP`                       | /                                    | dict   | Map of URI/Host to image, if this is present then `IMG_PATH` will be ignored, see more on [MultiPath](/configuration/multipath/) page                                                                                                                                                                                                                                                                                                                                                       |
| `ALLOWED_TYPES`                 | `WEBP_ALLOWED_TYPES`                 | list   | List of allowed image types, if you'd like to allow all image types, just use `["*"]` here.                                                                                                                                                                                                                                                                                                                                                                                                 |
| `CONVERT_TYPES`                 | `WEBP_CONVERT_TYPES`                 | string | The image types list that WebP Server will try to convert to, default is `["webp"]` which means it will only try to convert image to WebP, available options: `["webp","avif","jxl"]`.                                                                                                                                                                                                                                                                                                      |
| `STRIP_METADATA`                | `WEBP_STRIP_METADATA`                | bool   | Whether to Strip EXIF metadata from images.                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `ENABLE_EXTRA_PARAMS`           | `WEBP_ENABLE_EXTRA_PARAMS`           | bool   | Means whether to enable Extra Parameters, basically it allows you to do some transform on images like `https://img.webp.sh/path/tsuki.jpg?width=20`, you can find more info on [Extra Parameters](/reference/extra-params/) page.                                                                                                                                                                                                                                                           |
| `EXTRA_PARAMS_CROP_INTERESTING` | `WEBP_EXTRA_PARAMS_CROP_INTERESTING` | string | Defines how should WebP Server crop image when `ENABLE_EXTRA_PARAMS` is on and image requests contains both `width` and `height`, available options are: "InterestingNone", "InterestingEntropy", "InterestingCentre", "InterestingAttention", "InterestringLow", "InterestingHigh", "InterestingAll", you can find more info on [Extra Parameters](/reference/extra-params/) page.                                                                                                         |
| `READ_BUFFER_SIZE`              | `WEBP_READ_BUFFER_SIZE`              | number | per-connection buffer size for requests’ reading. This also limits the maximum header size. <br> Increase this buffer if your clients send multi-KB RequestURIs and/or multi-KB headers (for example, BIG cookies).                                                                                                                                                                                                                                                                         |
| `CONCURRENCY`                   | `WEBP_CONCURRENCY`                   | number | Maximum number of concurrent connections                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `DISABLE_KEEPALIVE`             | `WEBP_DISABLE_KEEPALIVE`             | string | Disable keep-alive connections, the server will close incoming connections after sending the first response to the client                                                                                                                                                                                                                                                                                                                                                                   |
| `CACHE_TTL`                     | `WEBP_CACHE_TTL`                     | number | Cache TTL **(minutes)** for [remote backends](/configuration/remote-backend/) (when `IMG_PATH` or a matched `IMG_MAP` target is an `http(s)` URL). <br> We use `HEAD` request to get remote image info, so your backend needs to support `HEAD` request, after first successfully `HEAD` request, it will be cached for `CACHE_TTL` minutes, during that period, we will not send `HEAD` request again and use local cache for rendering. <br> Setting this value to 0 means cache forever. |
| `MAX_CACHE_SIZE`                | `WEBP_MAX_CACHE_SIZE`                | number | With unit of MiB, the default value is 0, which not clean local cached files, if this value is set, for example 50, then a background task will run alongside with WebP Server Go once per minute to make sure the local cached directories(`./metadata`, `./exhaust` and `./remote-raw`(if using remote backend)) will be below 50MiB, seperately.                                                                                                                                         |

## Configuration examples

For scenario-based configuration templates, see [Configuration Examples](/configuration/examples/).
