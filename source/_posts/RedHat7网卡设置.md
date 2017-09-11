---
title: Red Hat Enterprise Linux 7 64 位 网卡配置
tags: [Red Hat7,网卡配置]
categories: [linux]
---

编辑网卡配置文件
``` bash
[root@localhost /]# vi /etc/sysconfig/network-scripts/ifcfg-eno16777736
```

HWADDR=00:0C:29:B8:40:8D
TYPE=Ethernet
BOOTPROTO=dhcp   ##dhcp：自动获取IP，static：静态IP，none：不用协议
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
NAME=eno16777736
UUID=5e66154a-c337-4dfa-a555-9b8ae0bd27ed
`ONBOOT=yes`   ##启动network服务时是否启用该网卡，yes为启用

重启网络服务
``` bash
[root@localhost /]# service network restart
```