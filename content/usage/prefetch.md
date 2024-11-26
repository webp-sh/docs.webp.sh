# Prefetch

Prefetch is useful if you'd like to have all your images converted in advance, especially suitable for large amount of images with high traffic volume, using prefetch will convert your images and cache the converted images, hence future requests will not consume computing power.

## Start server with prefetch

Start WebP Server with prefetch enabled will convert all your images to WebP on startup in background, the following things will happen in this mode:
1. Start WebP Server Go Server
2. Convert all images in background
3. WebP Server Go keeps running as normal

```sh
./webp-server -prefetch
```
If you want to control threads to use while prefetching, add `-jobs=4`. 
By default, it will utilize all your CPU cores.
```sh
# use 4 cores
./webp-server -prefetch -jobs=4
```

## Prefetch only

{{< hint "info" >}}
* `-prefetch-foreground` is added at version 0.12.3
{{< /hint >}}

If you have a running instance of WebP Server Go, and you wish to prefetch images now without interfering the running process, you can use `-prefetch-foreground`, the following things will happen in this mode:
1. Convert all images in foreground

```sh
./webp-server -prefetch-foreground
```