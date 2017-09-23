---
title: 配置本地yum源
tags: [yum]
categories: [linux]
---

1、挂载系统安装光盘
``` bash
# mount /dev/cdrom /mnt/cdrom/
```

2、配置本地yum源
``` bash
# cd /etc/yum.repos.d/
```
``` bash
# ls
```
会看到四个repo 文件

CentOS-Base.repo 是yum 网络源的配置文件
CentOS-Media.repo 是yum 本地源的配置文件

修改CentOS-Media.repo
``` bash
# cat CentOS-Media.repo
```

``` bash
# CentOS-Media.repo
#
# This repo is used to mount the default locations for a CDROM / DVD on
#  CentOS-5.  You can use this repo and yum to install items directly off the
#  DVD ISO that we release.
#
# To use this repo, put in your DVD and use it with the other repos too:
#  yum --enablerepo=c5-media [command]
#
# or for ONLY the media repo, do this:
#
#  yum --disablerepo=\* --enablerepo=c5-media [command]


[c5-media]
name=CentOS-$releasever - Media
baseurl=file:///media/CentOS/
        file:///mnt/cdrom/
        file:///media/cdrecorder/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5
```

在baseurl 中修改第2个路径为/mnt/cdrom（即为光盘挂载点）

将enabled=0改为1

3、禁用默认的yum 网络源

将yum 网络源配置文件改名为CentOS-Base.repo.bak，否则会先在网络源中寻找适合的包，改名之后直接从本地源读取。

4、执行yum 命令
``` bash
# yum install postgresql
```