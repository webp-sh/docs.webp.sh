---
title: Build
type: docs
weight: 14
---

# Build WebP Server Go

Install latest version of golang, enable go module, install `libaom` and clone the repo, then...

```sh
make
```

> (`libaom` is for AVIF support, you can install it by `apt install libaom-dev` on Ubuntu, `yum install libaom-devel` on CentOS)
>
> If you are using Intel Mac, you can install it by `brew install aom`
>
> If you are using Apple Silicon, you need to `brew install aom && export CPATH=/opt/homebrew/opt/aom/include/;LIBRARY_PATH=/opt/homebrew/opt/aom/lib/`, more references can be found at [在M1 Mac下开发WebP Server Go | 土豆不好吃](https://dmesg.app/m1-aom.html).

**Due to the limitations of webp module, you can't cross compile this tool. 
But the binary will work instantly on your platform and arch**