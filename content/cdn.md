---
title: Use with CDN
type: docs
weight: 15
---

# Use with CDN

If you use CDN(such as Cloudflare) for your website, since WebP Server Go will render different image on different devices, its vital to make sure CDN doesn't cache our outputs, for example, Cloudflare will cache all the images(by extension) from your website, which will cache the `webp` format of the image and use it for all visitors, this cached image cannot be shown on some old Safari browser.

There are two way to mitigate this problem

1. If you are using webserver like Nginx to reverse proxy, we'd recommend you to add a `private` header like the example below, this will prevent your images from being cached and webp images rendered to Safari users(which will of course cause some troubles).


```nginx
location ^~ /wp-content/uploads/ {
    add_header Cache-Control 'private';
    proxy_pass http://127.0.0.1:3333;
}
```

2. If you are not using webserver, you can add some rules on Cloudflare, example as below, let's say your images are under `https://example.com/wp-content/uploads/`

![](/images/cf-cache-rule.png)