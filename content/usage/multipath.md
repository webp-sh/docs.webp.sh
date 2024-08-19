# MultiPath

Starting from version `0.9.9`, we're supporting multipath, thanks to contribution by [@bugfest](https://github.com/bugfest) ❤️.

If you'd like to use multiple path for serving different images, you can use the new `config.json` format. You can specify additional paths mappings under `IMG_MAP` dict.

`IMG_MAP`'s keys are URIs or Hosts names will be translated to their corresponding mapped value. It works in two different modes:

* URI map: Keys have the form `/.*`
* Host map: Keys have the form `http?s://.*`

Keys not matching this format will be ignored

The values can be:

- Relative path to the current work directory
- Absolute path
- Remote HTTP or HTTPS host

Example:

```json
{
  "HOST": "0.0.0.0",
  "PORT": "3333",
  "QUALITY": "80",
  "IMG_PATH": "./pics",
  "EXHAUST_PATH": "./exhaust",
  "IMG_MAP": {
    "/2": "./pics2",
    "/3": "./pics3",
    "http://www.example.com": "https://docs.webp.sh"
  },
  "ALLOWED_TYPES": ["jpg","png","jpeg","bmp","gif","svg","heic","nef"],
  "CONVERT_TYPES": ["webp"],
  "STRIP_METADATA": true,
  "CACHE_TTL": 259200,
  "ENABLE_EXTRA_PARAMS": false
}
```

For example, you have some images under `./pics`, `./pics2` and `./pics3`, as below:

* `./pics` contains `1.jpg`, `2.jpg`, `3.jpg`
* `./pics2` contains `example.png`
* `./pics3` contains `webp_server.jpg`
* `https://docs.webp.sh` is a valid web service reachable by the local instance; hosts `/favicon.png`

Then you can access them via:

* `http://localhost:3333/1.jpg`, `http://localhost:3333/2.jpg`, `http://localhost:3333/3.jpg`
* `http://localhost:3333/2/example.png`
* `http://localhost:3333/3/webp_server.jpg`
* `http://www.example.com/favicon.png`

For the last example to work you need to pass the `Host` header as part of the request. If you use `curl` you can test it with the command `curl -H 'Host: www.example.com' http://localhost:3333/favicon.png`

## Note when using a remote backend

If you're using a remote backend, like `"http://www.example.com": "https://docs.webp.sh"` in the example above, you need to make sure the remote backend is reachable by the local instance and will satisfy the following requirements:

### Headers

We use `Etag` header from backend to fetch and refresh/rebuild cache, so it's a need that your backend's response will contain a `Etag` header.

We use `HEAD` request to get remote image info(to see if image still exist and whether it has changed), so your backend needs to support `HEAD` request, after first successfully `HEAD` request, it will be cached for `CACHE_TTL` **minutes**, during that period, we will not send `HEAD` request again and use local cache for rendering.

Supported(tested) backends:

* Aliyun OSS
	* `ETag`
* Tecent Cloud COS
	* `ETag`
* AWS S3
	* `etag`
* Nginx
	* You need to enable etag output by adding `etag on;` in your server block
	* `ETag`

## Note on `IMG_MAP`

If you have multiple paths sharing the same prefix, for example:

* `/image`
* `/image1`
* `/im`

Please be sure the longer prefix item is above the shorter one, for the example above, the `IMG_MAP` section should be defined as follows:

```json
"IMG_MAP": {
  "/image1": "./some-path",
  "/image": "./some-other-path",
  "/im": "https://some-image.webp.sh"
},
```