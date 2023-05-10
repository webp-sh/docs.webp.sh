---
title: Benchmark
type: docs
weight: 12
---

# Benchmark on convert

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