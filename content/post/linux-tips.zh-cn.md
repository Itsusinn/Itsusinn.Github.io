---
title: "Linux Tips"
description: 
date: 2023-08-27T21:55:07+08:00
image: 
math: 
license: 
hidden: false
comments: true
---

### 创建顶级Btrfs子卷

```bash
$ fdisk -l
$ sudo mount /dev/<part> -o subvolid=5 /mnt
$ cd /mnt
$ sudo btrfs subvolume create <name>
$ sudo btrfs subvolume list /
```

<part>: Btrfs分区名
<name>: 子卷名称

### 挂载Btrfs子卷为交换区

```bash
$ sudo vim /etc/fstab
UUID=<UUID>                        /swap          btrfs   subvol=/<name>,defaults 0 0
/swap/swapfile                     swap           swap    defaults 0 0
$ cd /
$ sudo mkdir swap
$ sudo mount -a
$ sudo btrfs filesystem mkswapfile --size 8g --uuid clear /swap/swapfile
$ sudo swapon /swap/swapfile


$ sudo mount -a
```

<UUID> : Btrfs分区UUID

<name>: 子卷名称

### [Docker UFW](https://github.com/chaifeng/ufw-docker?tab=readme-ov-file#ufw-docker-%E5%B7%A5%E5%85%B7)

如果希望允许外部网络访问 Docker 容器提供的服务，比如有一个容器的服务端口是 80。那就可以用以下命令来允许外部网络访问这个服务：

ufw route allow proto tcp from any to any port 80

这个命令会允许外部网络访问所有用 Docker 发布出来的并且内部服务端口为 80 的所有服务。

### FFMPEG 将所有wmv转为MP4
```bash
for files in $(ls *.wmv)
do
  ffmpeg -i $files -c:v libx264 -crf 23 -c:a aac -q:a 100 ${files%%.*}.mp4
done
```
