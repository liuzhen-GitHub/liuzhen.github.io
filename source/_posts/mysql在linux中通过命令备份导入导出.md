---
title: mysql在linux中通过命令备份 导入导出
tags: [mysql备份,mysql导入导出]
categories: linux
---

## 连接到远程linux数据库
mysql -hIP -u用户名 -p密码
``` bash
[root@localhost liuzhen]#  mysql -h192.168.1.180 -uroot -p123456
```

## 列出有哪些数据库
``` bash
mysql> show databases;
```

## 创建数据库 create database xxx数据库名;
``` bash
mysql> create database lmcmsv4.0;
```

## 切换数据库
use xxx数据库名;
``` bash
mysql> use lmcmsv4.0;
```

## 列出数据库中所有表
``` bash
mysql> show tables;
```

## 备份数据库
mysqldump -hIP -u用户名 -p密码 数据库名 > 备份路径
``` bash
mysqldump -h192.168.1.180 -uroot -p123456 lmcms_160718 > /usr/local/119_sql_bak_liuzhen/119_lmcms_160718.2016-7-31.bak
```

## 导入数据库
第一种方式：mysql -hIP -u用户名 -p密码 数据库 < 备份地址
``` bash
[root@localhost liuzhen]# mysql -h192.168.1.180 -uroot -p123456  lmcmsv4.0 < lmcmsv4.0.sql
```

第二种方式：source  备份地址

## 查看表结构
``` bash
show create table cms_user;
```

## 授权
例1、增加一个用户user001密码为123456，让他可以在任何主机上登录，并对所有数据库有查询、插入、修改、删除的权限。首先用以root用户连入MySQL，然后键入以下命令：
``` bash
mysql> grant select,insert,update,delete on *.* to user001@"%" Identified by "123456";
```

例2、增加一个用户user002密码为123456,让此用户只可以在localhost上登录,也可以设置指定IP，并可以对数据库test进行查询、插入、修改、删除的操作 (localhost指本地主机，即MySQL数据库所在的那台主机）

这样用户即使用知道user_2的密码，他也无法从网上直接访问数据库，只能通过MYSQL主机来操作test库。
首先用以root用户连入MySQL，然后键入以下命令：
``` bash
mysql>grant select,insert,update,delete on test.* to user002@localhost identified by "123456";
```