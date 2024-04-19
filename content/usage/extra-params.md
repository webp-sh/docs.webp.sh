# Extra Parameters

You can turn it on by setting `"ENABLE_EXTRA_PARAMS": true` in `config.json`.

Currently, we support the following params:

* height
* width
* max_width
* max_height

both are in px.

## Example on `height` and `width`

{{< columns >}}

<img src="/images/nasa.jpg"/>

`http://some-host.tld/path/to/nasa.jpg`

<--->
<img src="/images/nasa-200.jpg"/>

`http://some-host.tld/path/to/nasa.jpg?width=200`

{{< /columns >}}

If you set both `width` and `height`, your image will be cropped using attention crop by default maintain your width-height ratio, like this:

{{< columns >}}

<img src="/images/origin.png"/>

`http://some-host.tld/path/to/origin.png`

<--->
<img src="/images/attention_800.png"/>

`http://some-host.tld/path/to/origin.png?width=400&height=800`

<--->
<img src="/images/attention_400.png"/>

`http://some-host.tld/path/to/origin.png?width=400&height=400`

{{< /columns >}}

> attention crop: look for features likely to draw human attention.

You can use `EXTRA_PARAMS_CROP_INTERESTING` config to change crop interesting, configuration example at [Configuration](/usage/configuration/) page.

More examples can be found on our blog post: [How does different VipsInteresting values in libvips determine the cropping position of an image?](https://blog.webp.se/vips-crop-en/).

## Note on `max_height` and `max_width`

These two parameters are used to limit the maximum size of the image  and will limit large-size images within these two dimensions while  keeping small-size images unchanged.

For example, we have a 500x500px image called `big.jpg`. When you visit `/big.jpg?max_height=200&max_width=100`, since `max_width` is smaller, the image will be scaled to 100x100px.

If we have an 80x80px image called `small.jpg`, when you visit `/small.jpg?max_height=200&max_width=100`, since the length and width of the image are within range, this image will not be processed.

This new feature is mainly in response to the demand at: https://github.com/webp-sh/webp_server_go/issues/305.

## More details

### Non Remote Backend

When you're not using Remote Backend, suppose your image is at `/home/webp_server/exhaust/path/to/tsuki.jpg`, and `ENABLE_EXTRA_PARAMS` is enabled.

* When requesting image at `http://some-host.tld/path/to/tsuki.jpg`, the optimized image will be at `/home/webp_server/exhaust/path/to/tsuki.jpg.1582558990.webp_width=0&height=0`
* When requesting image at `http://some-host.tld/path/to/tsuki.jpg?width=200`, the image will be compressed to width of 200px, height/width ratio will remain the same, and the optimized image will be at `/home/webp_server/exhaust/path/to/tsuki.jpg.1582558990.webp_width=200&height=0`

### Remote Backend

`ENABLE_EXTRA_PARAMS` is currently available in Remote Backend.