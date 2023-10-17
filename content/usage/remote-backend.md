# Remote Backend(Proxy Mode)

## Usage

If you'd like to run WebP Server Go as a proxy for remote backend services, you could define your configuration file like this:

```json
{
  "HOST": "127.0.0.1",
  "PORT": "3333",
  "QUALITY": "80",
  "IMG_PATH": "https://test.webp.sh",
  "EXHAUST_PATH": "",
  "ALLOWED_TYPES": ["jpg","png","jpeg","bmp","gif","svg","heic"],
}
```

Please pay attention to the `IMG_PATH` config, in the example above, you suppose your files are at `https://test.webp.sh/path/to/images.png`.

## Tips

Please be aware that the `IMG_PATH` must:

* Begin with `https://` or `http://`
* Does not contain trailing slash.
	* Correct: `https://test.webp.sh`
	* Wrong: `https://test.webp.sh/`

## Requirements for backend

We use `Etag` header from backend to fetch and refresh/rebuild cache, so it's a need that your backend's response will contain a `Etag` header.

We use `HEAD` request to get remote image info, so your backend needs to support `HEAD` request.

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
