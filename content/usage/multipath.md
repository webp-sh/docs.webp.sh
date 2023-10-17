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
  "HOST": "127.0.0.1",
  "PORT": "3333",
  "IMG_PATH": "./pics",
  "EXHAUST_PATH": "./exhaust",
  "IMG_MAP": {
    "/2": "./pics2",
    "/3": "./pics3",
    "http://www.example.com": "https://docs.webp.sh"
  },
  "ALLOWED_TYPES": ["jpg","png","jpeg","bmp","gif","svg","heic"],
  "ENABLE_AVIF": false,
  "ENABLE_EXTRA_PARAMS": false
}
```

> Remember to change `"HOST": "127.0.0.1",` to `"HOST": "0.0.0.0",` if you'd like to use config inside Docker.

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
