---
title: CentOS7 Apache2.4.23 + php-5.6.30 安装 配置
tags: [php5.6安装,apache2.4安装,centos7]
categories: linux
---
### 1、Apache2.4.23下载
下载Apache安装包  httpd-2.4.23.tar.gz
下载地址：http://apache.fayea.com/httpd/

### 2、Apache 安装要求
必须安装APR、APR-Util、PCRE，gcc、gcc-c++等包

### 3、安装gcc 和 gcc-c++
采用yum安装的方式
``` bash
yum -y install gcc gcc-c++
```

编译命令：（除了指定Apache的安装目录外，还要安装apr、apr-util、pcre，并指定参数）
``` bash
[root@bogon software]# wget http://archive.apache.org/dist/httpd/httpd-2.4.23.tar.gz
[root@bogon software]# tar -zxvf httpd-2.4.23.tar.gz
[root@bogon software]# cd httpd-2.4.23
[root@bogon httpd-2.4.23]# ./configure --prefix=/usr/local/apache2 \
--with-apr=/usr/local/apr \
--with-apr-util=/usr/local/apr-util/ \
--with-pcre=/usr/local/pcre
checking for chosen layout... Apache
checking for working mkdir -p... yes
checking for grep that handles long lines and -e... /usr/bin/grep
checking for egrep... /usr/bin/grep -E
checking build system type... x86_64-unknown-linux-gnu
checking host system type... x86_64-unknown-linux-gnu
checking target system type... x86_64-unknown-linux-gnu
configure:
configure: Configuring Apache Portable Runtime library...
configure:
checking for APR... configure: error: the --with-apr parameter is incorrect. It must specify an install prefix, a build directory, or an apr-config file.
[root@bogon httpd-2.4.23]#
```
在编译Apache时出现了上面问题，因为我还没有安装apr、apr-util、pcre，接下来依次安装依赖

http://apr.apache.org/download.cgi  下载apr-1.5.2.tar.gz、apr-util-1.5.4.tar.gz
http://www.pcre.org/ 官网
https://sourceforge.net/projects/pcre/files/pcre/  选择pcre下载，不用pcre2
下载最新版本pcre-8.39.tar.gz

解决apr问题
``` bash
[root@bogon software]# wget http://archive.apache.org/dist/apr/apr-1.5.2.tar.gz
[root@bogon software]# tar -zxvf  apr-1.5.2.tar.gz
[root@bogon software]# cd apr-1.5.2/
[root@bogon apr-1.5.2]# ./configure --prefix=/usr/local/apr
[root@bogon apr-1.5.2]# make && make install
```

解决APR-util问题
``` bash
[root@bogon software]# wget http://archive.apache.org/dist/apr/apr-util-1.5.4.tar.gz
[root@bogon software]# tar -zxvf apr-util-1.5.4.tar.gz
[root@bogon software]# cd apr-util-1.5.4/
[root@bogon apr-util-1.5.4]# ./configure --prefix=/usr/local/apr-util \
-with-apr=/usr/local/apr/bin/apr-1-config
[root@bogon apr-util-1.5.4]# make && make install
```

make过程如果遇到下面这个错误，可能缺expat的开发库，安装expat库试试。yum install expat-devel
``` bash
[root@jdu4e00u53f7 apr-util-1.6.0]# make
make[1]: Entering directory `/liuzhen/source/apr-util-1.6.0'
/bin/sh /liuzhen/apr/build-1/libtool --silent --mode=compile gcc -g -O2 -pthread   -DHAVE_CONFIG_H  -DLINUX -D_REENTRANT -D_GNU_SOURCE   -I/liuzhen/source/apr-util-1.6.0/include -I/liuzhen/source/apr-util-1.6.0/include/private  -I/liuzhen/apr/include/apr-1    -o xml/apr_xml.lo -c xml/apr_xml.c && touch xml/apr_xml.lo
xml/apr_xml.c:35:19: fatal error: expat.h: No such file or directory
 #include <expat.h>
compilation terminated.
make[1]: *** [xml/apr_xml.lo] Error 1
make[1]: Leaving directory `/liuzhen/source/apr-util-1.6.0'
make: *** [all-recursive] Error 1
```

解决pcre-config问题
``` bash
[root@bogon software]# wget https://sourceforge.net/projects/pcre/files/pcre/8.39/pcre-8.39.tar.gz
[root@bogon software]# tar -zxvf pcre-8.39.tar.gz
[root@bogon software]# cd pcre-8.39
[root@bogon pcre-8.39]# ./configure --prefix=/usr/local/pcre
[root@bogon pcre-8.39]# make && make install
```

再次编译httpd-2.4.23：
``` bash
[root@bogon software]# cd httpd-2.4.23
[root@bogon httpd-2.4.23]# ./configure --prefix=/usr/local/apache2 \
--with-apr=/usr/local/apr \
--with-apr-util=/usr/local/apr-util/ \
--with-pcre=/usr/local/pcre
[root@bogon httpd-2.4.23]# make && make install
```

编辑 httpd.conf 文件
``` bash
[root@localhost liuzhen]# vi /usr/local/apache2/conf/httpd.conf
```

找到：
＃ServerName www.example.com:80
修改为：
ServerName 127.0.0.1:80或者ServerName localhost:80
记得要去掉前面的“＃”

启动Apache
``` bash
[root@localhost liuzhen]# /usr/local/apache2/bin/apachectl start
```

停止Apache
``` bash
[root@localhost liuzhen]# /usr/local/apache2/bin/apachectl stop
```

重启Apache
``` bash
[root@localhost liuzhen]# /usr/local/apache2/bin/apachectl restart
```

在浏览器中通过http://localhost:80，如果看到页面中显示“It works!”字样，则代表Apache验证通过。

接下来是PHP相关配置

编辑 httpd.conf 文件
``` bash
[root@localhost liuzhen]# vi /usr/local/apache2/conf/httpd.conf
```
找到：
``` xml
<IfModule dir_module>
DirectoryIndex index.html
</IfModule>
```
添加 <font color=red >index.php</font>：
``` xml
<IfModule dir_module>
DirectoryIndex index.php index.html
</IfModule>
```

找到：
AddType  application/x-compress .Z
AddType application/x-gzip .gz .tgz
在后面添加（使Apcche支持PHP）：
AddType application/x-httpd-php .php
AddType application/x-httpd-php-source .php5

修改默认的Web站点目录

默认的目录为  "/usr/local/apache2/htdocs"，修改apache的配置文件httpd.conf，比如在新建一个 /home/gyw/WebSite的目录作为apache的站点目录
找到DocumentRoot这一行修改为：DocumentRoot "/home/gyw/WebSite"
找到 <Directory> 这一行修改为：<Directory "/home/gyw/WebSite">

测试:修改到文件夹出现错误:
<font color='#ff0000'>“You don't have permission to access /index.html on this server.”</font>

解决方法:
更改文件权限；chmod 755 index.html

打开apache配置文件httpd.conf，找到这么一段：
``` xml
<Directory />
     Options FollowSymLinks
     AllowOverride None
     Order deny,allow
     deny from all
     Satisfy all
</Directory>
```

安装PHP
我在php开发过程中需要使用gd2，所以顺便把安装gd2安装上
``` bash
[root@localhost liuzhen]# yum -y install libpng libpng-devel php-gd
```

PHP下载地址：http://www.php.net/downloads.php
``` bash
[root@localhost liuzhen]# wget -O ./php-5.6.30.tar.gz http://hk1.php.net/get/php-5.6.30.tar.gz/from/this/mirror
[root@localhost liuzhen]# tar -zxvf php-5.6.30.tar.gz
[root@localhost liuzhen]# cd php-5.6.30
[root@localhost php-5.6.30]# ./configure --prefix=/usr/local/php \
--with-apxs2=/usr/local/apache2/bin/apxs \
--with-mysql=mysqlnd \
--with-gd
```

出现下面错误
Sorry, I cannot run apxs.  Possible reasons follow:

1. Perl is not installed
2. apxs was not found. Try to pass the path using --with-apxs2=/path/to/apxs
3. Apache was not built using --enable-so (the apxs usage page is displayed)

The output of /liuzhen/apache2/bin/apxs follows:
./configure: /liuzhen/apache2/bin/apxs: /replace/with/path/to/perl/interpreter: bad interpreter: No such file or directory
configure: error: Aborting

解决步骤：
1、根据不能run apxs 。cd 到apache的bin目录下运行./apxs 运行结果

<span>---------------------------------------------------------</span>
bash: ./apxs: /replace/with/path/to/perl/interpreter: bad interpreter: No such file or directory
<span>---------------------------------------------------------</span>
2、vim apxs文件 找“/replace/with/path/to/perl/interpreter”关键字
在第一个行 ：#!/replace/with/path/to/perl/interpreter -w
根据perl的安装目录 /usr/bin/perl
修改为：#! /usr/bin/perl -w

重新执行php configure
``` bash
[root@localhost php-5.6.30]# ./configure --prefix=/usr/local/php \
--with-apxs2=/usr/local/apache2/bin/apxs \
--with-mysql=mysqlnd \
--with-gd
```

出现错误: <font color='#ff0000'>configure: error: xml2-config not found. Please check your libxml2 installation.</font>
安装libxml2和libxml2-devel
``` bash
[root@localhost php-5.6.30]# yum -y install libxml2 libxml2-devel
```

再次执行./configure命令有可能还会出现下面这个错误
PEAR package PHP_Archive not installed: generated phar will require PHP's phar extension be enabled.
clicommand.inc
directorytreeiterator.inc
invertedregexiterator.inc
directorygraphiterator.inc
pharcommand.inc
phar.inc

Build complete.
Don't forget to run 'make test'.

-bash: $'\343\200\200make': command not found

php 的编译时需要依赖pear package ，目前的问题错误"PEAR package PHP_Archive not installed"，已经明显报出这个问题。
因此编译使用参数 --without-pear   将pear 屏蔽掉编译安装后，再进行安装；同时因为phar 属于pear的一个库 ，所以不将phar关闭掉，同时还会报这个错误，
同时需要使用 --disable-phar   编译参数.
``` bash
./configure --without-pear  --disable-phar
make
make install
```

成功编译安装完成后，再安装pear
``` bash
wget  http://pear.php.net/go-pear.phar
/usr/local/bin/php go-pear.phar
```

重新运行上面的./configure命令
``` bash
[root@localhost php-5.6.30]# ./configure --prefix=/usr/local/php \
--with-apxs2=/usr/local/apache2/bin/apxs \
--with-mysql=mysqlnd \
--with-gd
[root@localhost php-5.6.30]# make &&　make install
```

--with-mysql=mysqlnd 加上此参数支持mysql
--with-gd 支持gd
    注意这里有一个-with-apxs2=/usr/local/apache2/bin/apxs选项，其中apxs是在安装Apache时产生的，apxs是一个为Apache HTTP服务器编译和安装扩展模块的工具，使之可以用由mod_so提供的LoadModule指令在运行时加载到Apache服务器中。我的理解是通过这个工具把PHP模块动态加载到Apache中

把原来位于源代码里面的php.ini-development拷贝到/usr/local/php/lib/php.ini下，并且重命名为php.ini
``` bash
[root@localhost liuzhen]# cp /liuzhen/php-5.3.29/php.ini-development /usr/local/php/lib/php.ini
```

编辑/usr/local/php/lib/php.ini文件，找到<font color="#ff0000">;extension=php_gd2.dll将前面分号去掉

先停止apache后重启apache
``` bash
[root@localhost liuzhen]# /usr/local/apache2/bin/apachectl stop
[root@localhost liuzhen]# /usr/local/apache2/bin/apachectl restart
```

测试
在apache的htdocs下建立一个php文件test.php，里面的内容如下：
<?php
phpinfo();
?>
然后在浏览器里输入http://127.0.0.1/test.php
如果出现php的相关配置，成功，如果什么都没有输入，说明失败，重新以上步骤或者查找原因

如果决定在安装后改变配置选项，只需重复最后的三步configure, make, 以及 make install，然后需要重新启动 Apache 使新模块生效。Apache不需要重新编译。

<font color="#ff0000">下面是我留的笔记就不用看了</font>
==========================
安装gd2，安装gd2的时候顺带安装了jpeg,png,zlib

1、安装zlib

http://www.zlib.net/
http://www.zlib.net/zlib-1.2.11.tar.gz

tar xf zlib-1.2.7.tar.gz

cd zlib-1.2.7
./configure
make
make install


2、安装 jpeg
https://jpeg.org/
http://www.ijg.org/
http://www.ijg.org/files/jpegsrc.v9b.tar.gz

tar xf jpegsrc.v9a.tar.gz
cd jpeg-9a
./configure --prefix=/usr/local/jpeg --enable-shared --enable-static
make
make install


3、安装 libpng
http://www.libpng.org/pub/png/libpng.html
http://download.sourceforge.net/libpng/libpng-1.6.28.tar.gz

http://blog.csdn.net/wang02011/article/details/7066361

tar xf libpng-1.6.12.tar.gz
cd libpng-1.6.12
./configure --prefix=/usr/local/libpng
make
make install


4、安装freetype
https://www.freetype.org/download.html
http://download.savannah.gnu.org/releases/freetype/
http://download.savannah.gnu.org/releases/freetype/freetype-2.7.1.tar.gz

tar xf freetype-2.5.3.tar.gz
cd freetype-2.5.3
./configure --prefix=/usr/local/freetype
make
make install

5、安装gd2
yum install php-gd
https://libgd.github.io/
https://github.com/libgd/libgd/archive/gd-2.2.4.tar.gz

tar xf libgd-2.1.0.gz
cd libgd-2.1.0
./configure --prefix=/usr/local/gd2 --with-jpeg=/usr/local/jpeg/ --with-png=/usr/local/png/ --with-zlib=/usr/local/zlib/ --with-freetype=/usr/local/freetype/
make
make install
(注，版本这里必须>=2.1.0，否则用php-5.6.0会报错，可能老版本的php不会报错)

安装php是参数调整
``` bash
[root@localhost php-5.6.30]# ./configure --prefix=/usr/local/php \
--with-apxs2=/usr/local/apache2/bin/apxs \
--with-php-config=/usr/local/php/bin/php-config \
--with-jpeg-dir=/usr/local/jpeg \
--with-png-dir=/usr/local/libpng \
--with-freetype-dir=/usr/local/freetype \
--with-mysql=mysqlnd \
--with-gd
[root@localhost php-5.6.30]# make &&　make install
```

安装curl
官网地址：https://curl.haxx.se/
下载地址：https://curl.haxx.se/download/curl-7.53.1.tar.gz
``` bash
[root@bogon liuzhen]# wget https://curl.haxx.se/download/curl-7.53.1.tar.gz
[root@bogon liuzhen]# tar -zxvf curl-7.53.1.tar.gz
[root@bogon liuzhen]# cd curl-7.53.1
[root@bogon curl-7.53.1]# ./configure --prefix=/usr/local/curl
[root@bogon curl-7.53.1]# make && make install
```

#vim /usr/local/php/lib/php.ini
添加如下一行：
extension=curl.so
安装php 只要打开开关 --with-curl=/usr/local/curl 就可以了。