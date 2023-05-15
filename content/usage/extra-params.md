# Extra Parameters

You can turn it on by setting `"ENABLE_EXTRA_PARAMS": true` in `config.json`.

Currently, we support the following params:

* height
* width

both are in px.

## Example

{{< columns >}}

<img src="/images/nasa.jpg"/>

`http://some-host.tld/path/to/nasa.jpg`

<--->
<img src="/images/nasa-200.jpg"/>

`http://some-host.tld/path/to/nasa.jpg?width=200`

{{< /columns >}}


## More details

### Non Remote Backend

When you're not using Remote Backend, suppose your image is at `/home/webp_server/exhaust/path/to/tsuki.jpg`, and `ENABLE_EXTRA_PARAMS` is enabled.

* When requesting image at `http://some-host.tld/path/to/tsuki.jpg`, the optimized image will be at `/home/webp_server/exhaust/path/to/tsuki.jpg.1582558990.webp_width=0&height=0`
* When requesting image at `http://some-host.tld/path/to/tsuki.jpg?width=200`, the image will be compressed to width of 200px, height/width ratio will remain the same, and the optimized image will be at `/home/webp_server/exhaust/path/to/tsuki.jpg.1582558990.webp_width=200&height=0`

### Remote Backend

`ENABLE_EXTRA_PARAMS` is currently available in Remote Backend.