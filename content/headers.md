---
title: Headers
type: docs
weight: 10
---

# Headers

WebP Server Go will render the following header in response:

* `etag`
    * Example: `W/"87328-49BF2BCE"`
    * This header is produced by `W/"<content length>-<CRC Checksum of the file>"` 

* `x-compression-rate`
    * Example: `0.87`
    * This header is produced by `size(webp_image)/size(original_image)`, if the value is bigger than 1, it means the converted webp image is bigger than the original image, hence the original image is return to the visitor.