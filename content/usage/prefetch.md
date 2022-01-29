# Prefetch

Prefetch will convert all your images to WebP. Don't worry, WebP Server will start, you don't have to wait until prefetch completes.
```sh
./webp-server -prefetch
```
If you want to control threads to use while prefetching, add `-jobs=4`. 
By default, it will utilize all your CPU cores.
```sh
# use 4 cores
./webp-server -prefetch -jobs=4
```
