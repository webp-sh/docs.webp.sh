---
title: Query Parameters
type: docs
bookToc: false
weight: 21
---

# Query Parameters

Besides [Extra Parameters](/reference/extra-params/) (`width`, `height`, and related options), WebP Server Go supports metadata query parameters on image requests.

## `meta=full`

Request full metadata for a resource as JSON:

```
GET /path/to/image.jpg?meta=full
```

Behavior:

| Condition | HTTP status |
| --------- | ----------- |
| Valid path and source exists (local file or reachable remote) | `200` with JSON body |
| Malformed or illegal path | `404 Not Found` |
| Valid path but source does not exist | `404 Not Found` |

Previously, `meta=full` could return metadata from cache even when the source file was missing or the path was invalid. Current versions validate the path and confirm the source exists **before** returning metadata.

For path validation and status code policy, see [Security](/reference/security/).
