---
title: linux jdk源码安装
tags: [jdk,源码安装]
categories: [linux]
---

jdk源码包下载地址：http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
获取源码包jdk-8u101-linux-x64.tar.gz

解压
``` bash
[root@localhost tmp]# tar -zxvf jdk-8u101-linux-x64.tar.gz
```

创建源码存放目录
``` bash
[root@localhost /]# mkdir /usr/local/java
```

移动解压后的源码
``` bash
[root@localhost /]# mv /tmp/jdk1.8.0_101/ /usr/local/java/
```

设置环境变量
``` bash
[root@localhost /]# vi /etc/profile
```

打开之后，在文件末尾加入环境变量定义
``` bash
export JAVA_HOME=/usr/local/java/jdk1.8.0_101
export JRE_HOME=/usr/local/java/jdk1.8.0_101/jre
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
export PATH=$PATH:$JAVA_HOME/bin
```
保存配置

让配置生效
``` bash
[root@localhost /]#  source /etc/profile
```

验证是否有效
``` bash
[root@localhost /]# java -version
java version "1.8.0_101"
Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)
[root@localhost /]#
```

看到`java version "1.8.0_101"`表示jdk已经安装成功