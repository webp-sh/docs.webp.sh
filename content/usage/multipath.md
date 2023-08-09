# MultiPath

Starting from version 0.9.9, we're supporting multipath, thanks to contribution by [@bugfest](https://github.com/bugfest)❤️.

If you'd like to use multiple path for serving different images, you can use the new `config.json` format.


```json
{
  "HOST": "127.0.0.1",
  "PORT": "3333",
  "IMG_PATH": "./pics",
  "EXHAUST_PATH": "./exhaust",
  "IMG_MAP": {
    "/2": "./pics2",
    "/3": "./pics3",
    "www.example.com": "http://origin.example.com:8000",
    "http://www.example.com": "https://docs.webp.sh"
  },
  "ALLOWED_TYPES": ["jpg","png","jpeg"],
  "ENABLE_AVIF": false,
  "ENABLE_EXTRA_PARAMS": false
}
```

For example, you have some images under `./pics`, `./pics2` and `./pics3`, as below:

* `./pics` contains `1.jpg`, `2.jpg`, `3.jpg`
* `./pics2` contains `example.png`
* `./pics3` contains `webp_server.jpg`

Then you can access them via:

* `http://localhost:3333/1.jpg`, `http://localhost:3333/2.jpg`, `http://localhost:3333/3.jpg`
* `http://localhost:3333/2/example.png`
* `http://localhost:3333/3/webp_server.jpg`
