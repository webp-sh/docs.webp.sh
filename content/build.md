---
title: Build
type: docs
weight: 14
---

# Build WebP Server Go

Install latest version of golang, enable go module, install `libaom` (This is for AVIF support, you can install it by`apt install libaom-dev` on Ubuntu, `yum install libaom-devel` on CentOS) clone the repo, and then...

```sh
make
```

**Due to the limitations of webp module, you can't cross compile this tool. 
But the binary will work instantly on your platform and arch**