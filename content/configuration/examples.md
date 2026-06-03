---
title: Configuration Examples
type: docs
bookToc: false
weight: 25
---

# Configuration Examples

Use this page as a quick selector for common deployment patterns. Pick the closest scenario, copy the minimal config, then adjust paths and domains.

## Scenario overview

{{< mermaid >}}
flowchart TD
  startNode["Choose image source pattern"] --> singleLocal["Single local directory"]
  startNode --> multiLocal["Multiple local directories"]
  startNode --> remoteOnly["Remote origin only"]
  startNode --> mixedMode["Local + remote mixed"]

  singleLocal --> useImgPath["Use IMG_PATH as local path"]
  multiLocal --> useImgMapLocal["Use IMG_MAP with local prefixes"]
  remoteOnly --> useImgPathRemote["Use IMG_PATH as https URL"]
  mixedMode --> useImgMapMixed["Use IMG_MAP with local and https targets"]

  useImgPath --> localExample["See Local single directory"]
  useImgMapLocal --> multiExample["See Local multi-directory with IMG_MAP"]
  useImgPathRemote --> remoteExample["See Remote backend address"]
  useImgMapMixed --> mixedExample["See Mixed local and remote mappings"]
{{< /mermaid >}}

## Local single directory

Use this when all source images live under one local root directory, such as a single website or one static assets folder.

{{< mermaid >}}
flowchart LR
  clientReq["Client request /path/tsuki.jpg"] --> webpServer["WebP Server Go"]
  webpServer --> imgPathLocal["IMG_PATH=/var/www/img.webp.sh"]
  imgPathLocal --> localFile["/var/www/img.webp.sh/path/tsuki.jpg"]
  localFile --> convertAndCache["Convert and cache output"]
{{< /mermaid >}}

Example `config.json`:

```json
{
  "IMG_PATH": "/var/www/img.webp.sh",
  "EXHAUST_PATH": "./exhaust",
  "ENABLE_EXTRA_PARAMS": true
}
```

Quick check:

```shell
curl "http://127.0.0.1:3333/path/tsuki.jpg?width=320"
```

Expected behavior: requests like `/path/tsuki.jpg` are resolved from `/var/www/img.webp.sh/path/tsuki.jpg`, then converted and cached locally.

## Local multi-directory with `IMG_MAP`

Use this when different URL prefixes should map to different local folders.

{{< mermaid >}}
flowchart LR
  clientReq["Client request"] --> webpServer["WebP Server Go"]
  webpServer --> imgMap["IMG_MAP prefix match"]
  imgMap --> avatarsMap["/avatars -> /data/app/avatars"]
  imgMap --> productsMap["/products -> /data/app/products"]
  avatarsMap --> localAvatars["Read local avatars files"]
  productsMap --> localProducts["Read local products files"]
{{< /mermaid >}}

Example `config.json`:

```json
{
  "IMG_PATH": "./pics",
  "IMG_MAP": {
    "/avatars": "/data/app/avatars",
    "/products": "/data/app/products"
  },
  "EXHAUST_PATH": "./exhaust"
}
```

Quick check:

```shell
curl "http://127.0.0.1:3333/avatars/u1.png"
curl "http://127.0.0.1:3333/products/p1.jpg"
```

Expected behavior: each prefix is served from its mapped directory. Keep longer prefixes above shorter ones when they overlap (see [MultiPath](/configuration/multipath/)).

## Remote backend address

Use this when source images are hosted on an HTTP(S) origin (CDN, object storage, or another image host).

{{< mermaid >}}
flowchart LR
  clientReq["Client request /path/to/image.jpg"] --> webpServer["WebP Server Go"]
  webpServer --> imgPathRemote["IMG_PATH=https://img.example.com/static"]
  imgPathRemote --> remoteOrigin["Fetch from remote origin"]
  remoteOrigin --> convertAndCache["Convert and cache output"]
{{< /mermaid >}}

Example `config.json`:

```json
{
  "IMG_PATH": "https://img.example.com/static",
  "EXHAUST_PATH": "./exhaust",
  "CACHE_TTL": 4320
}
```

Quick check:

```shell
curl "http://127.0.0.1:3333/path/to/image.jpg"
```

Expected behavior: WebP Server Go auto-detects remote mode, fetches source images from the remote origin, and returns `404` when the origin resource does not exist.

See [Remote Backend](/configuration/remote-backend/) for origin requirements (`ETag` and `HEAD` support).

## Mixed local and remote mappings

Use this when some paths are still local while others already moved to remote storage.

{{< mermaid >}}
flowchart LR
  clientReq["Client request"] --> webpServer["WebP Server Go"]
  webpServer --> imgMap["IMG_MAP prefix match"]
  imgMap --> legacyMap["/legacy -> /srv/legacy-images"]
  imgMap --> cdnMap["/cdn -> https://img.example.com/static"]
  legacyMap --> localPath["Read local filesystem"]
  cdnMap --> remoteFetch["Fetch remote HTTP(S) source"]
  localPath --> sharedPipeline["Shared conversion and cache pipeline"]
  remoteFetch --> sharedPipeline
{{< /mermaid >}}

Example `config.json`:

```json
{
  "IMG_MAP": {
    "/legacy": "/srv/legacy-images",
    "/cdn": "https://img.example.com/static"
  },
  "EXHAUST_PATH": "./exhaust",
  "CACHE_TTL": 1440
}
```

Quick check:

```shell
curl "http://127.0.0.1:3333/legacy/a.jpg"
curl "http://127.0.0.1:3333/cdn/b.jpg"
```

Expected behavior: local-mapped requests read from filesystem paths, remote-mapped requests fetch via HTTP(S), and both share the same conversion and cache pipeline.

See [MultiPath](/configuration/multipath/) for prefix matching details and ordering rules.
