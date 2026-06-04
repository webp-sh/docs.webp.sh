---
title: Remote Backend
type: docs
bookToc: false
weight: 23
---

# Remote Backend

WebP Server Go can fetch original images from a remote HTTP or HTTPS origin, optimize them, and cache the results locally.

## Automatic detection

Remote (proxy) mode is enabled when the resolved image source is an `http://` or `https://` URL:

| Configuration | Remote when                                                                                              |
| ------------- | -------------------------------------------------------------------------------------------------------- |
| `IMG_PATH`    | Value is an `http://` or `https://` URL (for example `https://cdn.example.com/assets`)                 |
| `IMG_MAP`     | A matched mapping value is an `http://` or `https://` URL (see [MultiPath](/configuration/multipath/)) |

Otherwise the server serves and converts images from local filesystem paths.

## Requirements

When using a remote backend, the origin must be reachable from the WebP Server Go instance and should:

- Return an `ETag` header (used for cache validation and rebuild)
- Support `HEAD` requests (used to check existence and freshness)

See [MultiPath — Note when using a remote backend](/configuration/multipath/#note-when-using-a-remote-backend) for tested storage backends and header details.

## Related configuration

| Field            | Role                                                                                                                  |
| ---------------- | --------------------------------------------------------------------------------------------------------------------- |
| `CACHE_TTL`      | How long (in **minutes**) remote `HEAD` metadata is cached before re-checking the origin. `0` means cache forever. |
| `MAX_CACHE_SIZE` | When set, limits size of `./metadata`, `./exhaust`, and `./remote-raw` (remote mode) on disk.                      |

Example `config.json` with a remote default source:

```json
{
  "IMG_PATH": "https://img.example.com/static",
  "EXHAUST_PATH": "./exhaust",
  "CACHE_TTL": 4320
}
```

With [MultiPath](/configuration/multipath/), you can mix local directories and remote hosts in `IMG_MAP`.
