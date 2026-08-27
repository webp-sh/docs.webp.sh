---
title: Use with CDN
type: docs
weight: 15
---

# Use with CDN

WebP Server Go negotiates image formats from the request's `Accept` header. Starting from version `0.16.0`, negotiated image responses include:

```
Vary: Accept
```

This allows a variant-aware CDN to cache WebP, AVIF, JPEG XL, and original-format responses for the same URL without serving an unsupported format to another client.

WebP Server Go only enables a modern format when `Accept` explicitly lists its media type with a quality value greater than zero. For example, `image/jxl;q=0` does not enable JPEG XL. A missing header, `image/*`, or `*/*` alone uses the original-format fallback. User-Agent detection is not used.

## What `Vary: Accept` does

Under [RFC 9111](https://www.rfc-editor.org/rfc/rfc9111.html#section-4.1), a shared cache must not reuse a stored response with `Vary: Accept` unless the new request's `Accept` value matches the request that created that response, after any valid normalization.

The response header is only one half of the configuration. Before enabling CDN caching, verify that the CDN:

- forwards `Accept` to WebP Server Go;
- honors `Vary: Accept`, or explicitly includes equivalent image capabilities in its cache key;
- keeps resize query parameters such as `width`, `height`, `max_width`, and `max_height` in the cache key; and
- has purged image responses cached before variant handling was enabled.

Raw `Accept` headers have many equivalent permutations. If the CDN supports normalization, normalize the header to the formats WebP Server Go understands instead of using every raw header value. This avoids unnecessary cache fragmentation.

## Cloudflare

Cloudflare does not use arbitrary `Vary` values in caching decisions by default. Configure a Cache Rule with the [Vary setting](https://developers.cloudflare.com/cache/concepts/vary/) for the hostname or image path served by WebP Server Go.

In **Caching > Cache Rules**:

1. Create a rule matching the relevant hostname or image path.
2. Set **Cache eligibility** to **Eligible for cache**.
3. Under **Vary**, set the default action to **Bypass**.
4. Add `accept`, select **Normalize**, and allow these media types:
   - `image/avif`
   - `image/webp`
   - `image/jxl` (Needs to be entered manually, cannot be added via the selector)
5. Keep query strings in the cache key if you use resizing parameters.
6. Choose an Edge TTL appropriate for your source update and purge strategy.

![](/images/cache-vary-cloudflare.png)

The relevant Cache Rules action fragment is:

```json
{
  "cache": true,
  "vary": {
    "default": {
      "action": "bypass"
    },
    "headers": {
      "accept": {
        "action": "normalize",
        "media_types": ["image/avif", "image/webp", "image/jxl"]
      }
    }
  }
}
```

See Cloudflare's [Cache Rules Vary settings](https://developers.cloudflare.com/cache/how-to/cache-rules/settings/#vary) for the complete Rulesets API and Terraform configuration. Do not enable device-type or User-Agent cache segmentation for format negotiation; it is unnecessary and creates extra variants.

Cloudflare also offers a separate [Vary for Images](https://developers.cloudflare.com/cache/advanced-configuration/vary-for-images/) feature. It requires extension-based image URLs and an explicit variants rule. The general Cache Rules Vary setting above maps more directly to WebP Server Go's `Accept`-based behavior, including JPEG XL.

If you cannot configure either mechanism, bypass Cloudflare caching for the image route. This is the safe fallback, but it gives up CDN cache hits.

## Fastly

Fastly [supports `Vary` according to the HTTP specification](https://www.fastly.com/documentation/reference/http/http-headers/Vary/). No separate cache-key rule is required as long as `Vary: Accept` reaches Fastly unchanged.

Fastly does not normalize `Accept` by default. Sites with diverse clients should consider [normalizing `Accept`](https://www.fastly.com/documentation/reference/http/http-headers/Accept/#normalization) in VCL or Compute so semantically equivalent headers share a cache entry. Any normalization must preserve explicit `q=0` exclusions.

## Amazon CloudFront

Do not rely on the origin's `Vary` response header alone. Create a CloudFront cache policy for the image behavior and configure **Headers > Include the following headers > `Accept`**. Headers included in a cache policy are also forwarded to the origin.

CloudFront includes the full selected header value in the cache key. This is correct but can reduce the cache hit ratio when browsers send many equivalent `Accept` values. If this becomes significant, normalize image capabilities at the edge before cache lookup. See AWS's documentation on [cache policies and cache keys](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cache-key-understand-cache-policy.html).

## Other CDNs and reverse proxies

For another CDN or caching proxy, use the first supported option:

1. Honor the origin's `Vary: Accept` response header.
2. Add `Accept`, preferably normalized to `image/avif`, `image/webp`, and `image/jxl` capabilities, to the cache key and forward it to the origin.
3. If neither is possible, bypass caching for routes handled by WebP Server Go or return `Cache-Control: private` from the reverse proxy.

Never cache only by URL while ignoring `Accept`. The first cached AVIF, WebP, or JPEG XL response could otherwise be served to a client that cannot decode it.

## Verify the configuration

Purge old entries first, then request the same image with different capabilities:

```bash
IMAGE_URL='https://example.com/images/photo.jpg'

curl -sSI -H 'Accept: image/avif,image/webp,image/*;q=0.8' "$IMAGE_URL"
curl -sSI -H 'Accept: image/jpeg,image/png' "$IMAGE_URL"
curl -sSI -H 'Accept: image/jxl;q=0,image/jpeg' "$IMAGE_URL"
```

Check that:

- every negotiated image response contains `Vary: Accept`;
- `Content-Type` is compatible with each request;
- the `q=0` request is never served as `image/jxl`;
- repeated requests become cache hits according to the CDN's cache-status header; and
- switching between capability sets still returns the correct variant.

`Vary` controls variant selection, not freshness. Configure TTLs and purging separately according to how source images are updated.
