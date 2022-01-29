---
title: Use with CDN
type: docs
weight: 15
---

# Use with CDN

If you use CDN(such as Cloudflare) for your website, we'd recommend you to add a `private` header like the example below, this will prevent your images from being cached and webp images rendered to Safari users(which will of course cause some troubles).

## Cloudflare

Cloudflare will cache all the images(by extension) from your website, which will the `webp` format of the image and use it for all visitors, which the cached image cannot be shown on Safari browser.

So you need to add a custom header to prevent Cloudflare from caching those images, like this:

```nginx
location ^~ /wp-content/uploads/ {
    add_header Cache-Control 'private';
    proxy_pass http://127.0.0.1:3333;
}
```
