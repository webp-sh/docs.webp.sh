---
title: Benchmark
type: docs
weight: 12
bookToc: false
---

# Benchmark

Benchmarks are done with `AMD Ryzen 7 PRO 5850U` with a customized image set: totaling 2.6 GB. Around 80% of the images were taken with a Sony A7 camera, with file sizes averaging around 15 MiB. The remaining 20% were smaller images with sizes ranging from 1 MiB to 5 MiB.

```
[Nova@WebP webp_bench]$ du -ch pics
17M	pics/pics/MC
6.1M	pics/pics/Wallpapers
2.0M	pics/pics/elk
151M	pics/pics/TUNA-SFD
32M	pics/pics/ThinkPad
322M	pics/pics
5.2M	pics/exif-orientation-examples
2.6G	pics
2.6G	total
```

For more info on benchmark framework, or you'd like to do a benchmark yourself, visit https://github.com/webp-sh/webp_bench.


## Overall test results

![](/images/benchmark/webp_bench.svg)

![](/images/benchmark/webp_bench_performance.svg)
> For time consumed(shorter is better)

![](/images/benchmark/webp_bench_ram.svg)
> For RAM usage(smaller is better)


| Version | RAM Usage(jemalloc) | Time (jemalloc) | RAM Usage(glibc) | Time (glibc) | RAM Usage(tcmalloc) | Time (tcmalloc) |
| ------- | ------------------- | --------------- | ---------------- | ------------ | ------------------- | --------------- |
| 0.8.0  | 821  | 59   | 884  | 53   | 776  | 54   |
| 0.8.1  | 534  | 60   | 562  | 53   | 557  | 55   |
| 0.8.2  | 567  | 60   | 582  | 54   | 565  | 60   |
| 0.8.3  | 586  | 60   | 559  | 55   | 558  | 68   |
| 0.8.4  | 569  | 61   | 552  | 60   | 596  | 62   |
| 0.9.0  | 543  | 94   | 536  | 90   | 543  | 97   |
| 0.9.1  | 931  | 198  | 893 | 199  | 911  | 203  |
| 0.9.2  | 1127 | 203  | 1114 | 196  | 1094 | 200  |
| 0.9.3  | 1165 | 196  | 1043 | 195  | 973  | 194  |
| 0.9.4  | 1134 | 189  | 1069 | 195  | 1111 | 196  |
| 0.9.5  | 626  | 61   | 603  | 60   | 565  | 61   |
| 0.9.6  | 621  | 61   | 510  | 60   | 586  | 61   |
| 0.9.7  | 644  | 97   | 643  | 95   | 677  | 97   |
| 0.9.8  | 627  | 97   | 666  | 92   | 625  | 99   |
| 0.9.9  | 613  | 96   | 652  | 88   | 624  | 102  |
| 0.9.10 | 638  | 99   | 679  | 100  | 635  | 98   |

* * *


## Detailed test results on each version

X axis is time consumed(shorter is better), Y axis is RAM usage(smaller is better).


| Version | glibc                                  | jemalloc                         | tcmalloc                                  |
| ------- | -------------------------------------- | -------------------------------- | ----------------------------------------- |
| 0.8.0   | ![](/images/benchmark/glibc_0.8.0.png) | ![](/images/benchmark/0.8.0.png) | ![](/images/benchmark/tcmalloc_0.8.0.png) |
| 0.8.1   | ![](/images/benchmark/glibc_0.8.1.png) | ![](/images/benchmark/0.8.1.png) | ![](/images/benchmark/tcmalloc_0.8.1.png) |
| 0.8.2   | ![](/images/benchmark/glibc_0.8.2.png) | ![](/images/benchmark/0.8.2.png) | ![](/images/benchmark/tcmalloc_0.8.2.png) |
| 0.8.3   | ![](/images/benchmark/glibc_0.8.3.png) | ![](/images/benchmark/0.8.3.png) | ![](/images/benchmark/tcmalloc_0.8.3.png) |
| 0.8.4   | ![](/images/benchmark/glibc_0.8.4.png) | ![](/images/benchmark/0.8.4.png) | ![](/images/benchmark/tcmalloc_0.8.4.png) |
| 0.9.0   | ![](/images/benchmark/glibc_0.9.0.png) | ![](/images/benchmark/0.9.0.png) | ![](/images/benchmark/tcmalloc_0.9.0.png) |
| 0.9.1   | ![](/images/benchmark/glibc_0.9.1.png) | ![](/images/benchmark/0.9.1.png) | ![](/images/benchmark/tcmalloc_0.9.1.png) |
| 0.9.2   | ![](/images/benchmark/glibc_0.9.2.png) | ![](/images/benchmark/0.9.2.png) | ![](/images/benchmark/tcmalloc_0.9.2.png) |
| 0.9.3   | ![](/images/benchmark/glibc_0.9.3.png) | ![](/images/benchmark/0.9.3.png) | ![](/images/benchmark/tcmalloc_0.9.3.png) |
| 0.9.4   | ![](/images/benchmark/glibc_0.9.4.png) | ![](/images/benchmark/0.9.4.png) | ![](/images/benchmark/tcmalloc_0.9.4.png) |
| 0.9.5   | ![](/images/benchmark/glibc_0.9.5.png) | ![](/images/benchmark/0.9.5.png) | ![](/images/benchmark/tcmalloc_0.9.5.png) |
| 0.9.6   | ![](/images/benchmark/glibc_0.9.6.png) | ![](/images/benchmark/0.9.6.png) | ![](/images/benchmark/tcmalloc_0.9.6.png) |
| 0.9.7   | ![](/images/benchmark/glibc_0.9.7.png) | ![](/images/benchmark/0.9.7.png) | ![](/images/benchmark/tcmalloc_0.9.7.png) |
| 0.9.8   | ![](/images/benchmark/glibc_0.9.8.png) | ![](/images/benchmark/0.9.8.png) | ![](/images/benchmark/tcmalloc_0.9.8.png) |
| 0.9.9   | ![](/images/benchmark/glibc_0.9.9.png) | ![](/images/benchmark/0.9.9.png) | ![](/images/benchmark/tcmalloc_0.9.9.png) |
| 0.9.10   | ![](/images/benchmark/glibc_0.9.10.png) | ![](/images/benchmark/0.9.10.png) | ![](/images/benchmark/tcmalloc_0.9.10.png) |


# Benchmark on convert size(compression rate)

Benchmarks are done on version before 0.6.0, as for versions after 0.6.0, our internal benchmark shows about 5 times faster.

## 8 core

| file_size_range | file_num | src_size | dist_size |  total  |   user   | system | cpu  | core |
| :-------------: | :------: | :------: | :-------: | :-----: | :------: | :----: | :--: | :--: |
|  (10KB,500KB)   |   4600   |   1.3G   |   310M    | 1:44.49 | 905.41s  | 9.55s  | 875% |  8   |
|   (500KB,1MB)   |   3500   |   2.4G   |   361M    | 2:04.81 | 1092.50s | 7.98s  | 881% |  8   |
|    (1MB,4MB)    |   2000   |   3.8G   |   342M    | 2:32.64 | 1345.73s | 10.84s | 888% |  8   |
|   (4MB,32MB)    |   500    |   3.6G   |   246M    | 1:44.53 | 916.91s  | 12.03s | 888% |  8   |

## 1,2,4,8 core

| file_size_range | file_num | src_size | dist_size |    8    |    4    |    2    |    1    |
| :-------------: | :------: | :------: | :-------: | :-----: | :-----: | :-----: | :-----: |
|  (10KB,500KB)   |   4600   |   1.3G   |   310M    | 1:44.49 | 2:18.49 | 3:36.05 | 5:20.88 |
|   (500KB,1MB)   |   3500   |   2.4G   |   361M    | 2:04.81 | 2:49.46 | 4:16.41 | 6:28.97 |
|    (1MB,4MB)    |   2000   |   3.8G   |   342M    | 2:32.64 | 3:26.18 | 5:22.15 | 7:53.45 |
|   (4MB,32MB)    |   500    |   3.6G   |   246M    | 1:44.53 | 2:21.22 | 3:39.16 | 5:28.65 |
