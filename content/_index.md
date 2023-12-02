---
title: Introduction
type: docs
---

<p align="center">
	<img src="/images/webp_server.jpg"/>
</p>

This is a Server based on Golang, which allows you to serve WebP images on the fly. 
It will convert `jpg,jpeg,png` files by default, this can be customized by editing the `config.json`.

Currently supported image format: JPEG, PNG, BMP, GIF, SVG, HEIC, NEF, WEBP

> e.g When you visit `https://your.website/pics/tsuki.jpg`，it will serve as `image/webp`/`image/avif` format without changing the URL.
>
> GIF image will not be converted to AVIF format even with `ENABLE_AVIF` to `true`, because the converted AVIF image is not animated.