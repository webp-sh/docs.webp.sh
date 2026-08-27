---
title: Headers
type: docs
weight: 10
---

# Headers

WebP Server Go returns the following response headers:

* `vary`
    * Image response example: `Accept`
    * Negotiated image responses use `Vary: Accept`, allowing compatible shared caches to keep separate variants selected from the request's `Accept` header.
    * The CDN must honor `Vary` or include `Accept` in its cache key. See [Use with CDN](/deployment/cdn/).

* `etag`
    * Example: `W/"87328-49BF2BCE"`
    * This header is produced by `W/"<content length>-<CRC Checksum of the file>"`

* `x-compression-rate`
    * Example: `0.87`
    * This header is produced by `size(returned_image)/size(original_image)`. If the optimized image is larger than the original, WebP Server Go can return the smaller original image instead.
