---
title: 几种查看CentOS系统版本和位数的方法
tags: [centos,linux,系统版本]
categories: [linux,centos]
---

## 查看系统版本：
``` bash
[root@bogon ~]# cat /etc/redhat-release
CentOS Linux release 7.2.1511 (Core)
[root@bogon ~]#
```

``` bash
[root@bogon ~]# cat /proc/version
Linux version 3.10.0-327.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.3 20140911 (Red Hat 4.8.3-9) (GCC) ) #1 SMP Thu Nov 19 22:10:57 UTC 2015
[root@bogon ~]#
```

``` bash
[root@bogon ~]# uname -a
Linux bogon 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
[root@bogon ~]#
```

``` bash
[root@bogon ~]# cat /etc/issue
\S
Kernel \r on an \m

[root@bogon ~]#
```


## 查看64位还是32位：
``` bash
[root@bogon ~]# getconf LONG_BIT
64
[root@bogon ~]#
```

``` bash
[root@bogon ~]# file /bin/ls
/bin/ls: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=aa7ff68f13de25936a098016243ce57c3c982e06, stripped
[root@bogon ~]#
```

