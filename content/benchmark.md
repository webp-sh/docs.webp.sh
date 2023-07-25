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

`config.json` used for testing is as follows:

```json
{
    "HOST": "127.0.0.1",
    "PORT": "3333",
    "QUALITY": "80",
    "IMG_PATH": "./pics",
    "EXHAUST_PATH": "./exhaust",
    "ALLOWED_TYPES": ["jpg","png","jpeg","bmp","gif"],
    "ENABLE_AVIF": false,
    "ENABLE_EXTRA_PARAMS": false
}
```

For more info on benchmark framework, or you'd like to do a benchmark yourself, visit https://github.com/webp-sh/webp_bench.


## Overall test results

![](/images/benchmark/webp_bench.svg)

> For time consumed(shorter is better)
> 
> For RAM usage(smaller is better)


| Version | RAM Usage | Time (seconds) |
|---------|---------------|---------------|
| 0.8.0   | 709           | 49            |
| 0.8.1   | 522           | 49            |
| 0.8.2   | 538           | 48            |
| 0.8.3   | 553           | 48            |
| 0.8.4   | 541           | 49            |
| 0.9.0   | 535           | 73            |
| 0.9.1   | 758           | 156           |
| 0.9.2   | 1089          | 159           |
| 0.9.3   | 1051          | 158           |
| 0.9.4   | 806           | 159           |
| 0.9.5   | 595           | 50            |
| 0.9.6   | 538           | 50            |
| 0.9.7   | 628           | 80            |
| 0.9.8   | 605           | 80            |

* * *

X axis is time consumed(shorter is better), Y axis is RAM usage(smaller is better).

## Detailed test results

### 0.9.8
![](/images/benchmark/0.9.8.png)

### 0.9.7
![](/images/benchmark/0.9.7.png)

### 0.9.6
![](/images/benchmark/0.9.6.png)

### 0.9.5
![](/images/benchmark/0.9.5.png)

### 0.9.4
![](/images/benchmark/0.9.4.png)

### 0.9.3
![](/images/benchmark/0.9.3.png)

### 0.9.2
![](/images/benchmark/0.9.2.png)

### 0.9.1
![](/images/benchmark/0.9.1.png)

### 0.9.0
![](/images/benchmark/0.9.0.png)

### 0.8.4
![](/images/benchmark/0.8.4.png)

### 0.8.3
![](/images/benchmark/0.8.3.png)

### 0.8.2
![](/images/benchmark/0.8.2.png)

### 0.8.1
![](/images/benchmark/0.8.1.png)

### 0.8.0
![](/images/benchmark/0.8.0.png)


# Old Benchmark on convert

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
