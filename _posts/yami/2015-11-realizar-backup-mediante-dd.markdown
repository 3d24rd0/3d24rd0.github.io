---
img_path: /assets/yami/

categories:
- Blog
- やみのきし

tags:
- Guía
- Linux
- Shell

date: 2015-11-07 00:25:10+00:00
draft: false
title: Realizar backup mediante dd

---

Detectar el disco

``` sh
sudo fdisk -l
```

Realizar la copia con dd

``` sh
sudo dd if=/dev/sdX | gzip -9 > ./recalbox-20150411-sdb.img.gz
```

Restaurar la copia

``` sh
gunzip ./recalbox-20150411-sdb.img.gz | sudo dd of=/dev/sdX
```
