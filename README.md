# OSSFS
Docker Image for [OSSFS](https://github.com/aliyun/ossfs) based on Alpine Linux

## Changelog

对比 denverdino/ossfs-image
- OSSFS 版本更新至 v1.91.1
- alpine 使用 latest 镜像
- 编译时新增了 mysql-client tzdata
- 容器启动后执行 OSSFS 中的 run.sh

```
#!/bin/sh
rm -rf /tmp/*
mysqldump -hXXX -P3306 -uXXX -pXXX XXX > /tmp/XXX_$(date "+%Y%m%d_%H%M%S").sql
tar cz /tmp | openssl enc -aes-256-cbc -e -pbkdf2 -pass pass:XXX > /data/ossfs/XXX_$(date +%Y%m%d_%H%M%S).tar.gz.enc
```

## Build

```
docker build -t sliots/ossfs-mysql-client-image:latest .
```

## Run

```
version: '3.3'
services:
  mysqldump:
      container_name: mysqldump
      privileged: true
      tty: true
      stdin_open: true
      cap_add:
          - SYS_ADMIN
      devices:
          - /dev/fuse
      environment:
          - 'OSS_URL=XXX'
          - ACCESS_KEY=XXX
          - ACCESS_SECRET=XXX
          - OSS_BUCKET=XXX
          - OSSFS_OPTIONS=-oallow_other -ononempty
          - TZ=Asia/Shanghai
      image: ghcr.io/sliots/ossfs-mysql-client-image:master
```

## Reference
[https://github.com/BWITS/s3fs](https://github.com/BWITS/s3fs)
