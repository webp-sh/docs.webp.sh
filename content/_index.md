---
title: Introduction
type: docs
---

<p align="center">
	<img src="/images/webp_server.jpg"/>
</p>

This is a Server based on Golang, which allows you to serve WebP images on the fly. 

It aims to optimize and compress original images to more modern formats  without changing the original URL. This solves the problem of servers  having additional bandwidth overhead and slow loading speeds due to the  output of large old format images (such as JPG, PNG) on websites,  thereby improving webpage response speed and enhancing the Pagespeed  score.

Suppose you have a website: https://example.com with many pictures and photos at https://example.com/pics/someimages.jpg, ranging from 1M~5M in size. If you directly put these pictures on the  webpage, it will inevitably cause the webpage to load very slowly. At  this time, using WebP Server Go can:

1. Keep the original image address unchanged
2. Reduce the size of the image and output it in a more modern image format (such as `webp`, `avif`)

## Quick start

All my pictures are stored in the `/path/to/pics` directory, and I hope to have a simple HTTP service that can output optimized images after access.

At present, the simplest way is to directly use Docker container deployment. We just need to mount three directories into it. Create a `docker-compose.yml` file and write the following content:

```yml
version: '3'

services:
  webp:
    image: webpsh/webp-server-go
    restart: always
    volumes:
      - /path/to/pics:/opt/pics
      - ./exhaust:/opt/exhaust
      - ./metadata:/opt/metadata
    ports:
      -  127.0.0.1:3333:3333
```

Among them, `/path/to/pics` is the directory where our pictures are located. The remaining `./exhaust` and `./metadata` are the directories for storing optimized images and source information, respectively. For easy maintenance, they are directly placed in the same directory as `docker-compose.yml`.

Then start it with `docker-compose up -d`, and now WebP Server has started on `http://127.0.0.1:3333`.

Suppose our actual picture is `/path/to/pics/DSC05955.jpg`, then we can visit `http://127.0.0.1:3333/DSC05955.jpg` to see the optimized picture.

![](/images/quick.png)

* Content-Type: image/webp indicates that the picture has been output in `webp` format
* X-Compression-Rate: 0.13 indicates that the current picture size is 13% of the original picture

How about it, is it easy to understand? Next, just put Nginx/Caddy and other Web Servers in front of it and it can be used externally.