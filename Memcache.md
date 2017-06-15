wget https://github.com/websupport-sk/pecl-memcache/archive/php7.zip 
unzip php7.zip 
cd  pecl-memcache-php7
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install

在php.ini的最下面添加

[memcache]
extension_dir = "/usr/local/php/lib/php/extensions/no-debug-non-zts-20160306/"
extension = "memcache.so"

配置Memcached的步骤 
首先安装Libevent事件触发管理器。

# wget https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz
tar vxf libevent-2.0.21-stable.tar.gz
cd libevent-2.0.21-stable
./configure -prefix=/usr/local/libevent
# make && make install
# yum install libevent-devel

编译安装Memcache

wget http://memcached.org/files/memcached-1.4.25.tar.gz
tar vxf memcached-1.4.25.tar.gz
cd memcached-1.4.25
./configure -with-libevent=/usr/local/libevent   # ./configure
# make && make install

启动Memcache

# /usr/local/bin/memcached -d -m 128 -l 127.0.0.1 -p 11211 -u root   # (128为内存, 11211为端口,root为用户组)

重启php-fpm ，重新加载nginx

浏览器访问test.php,文件内容，检测是否整合php

<?php
    $mem = new memcache;
    $mem->connect('localhost', 11211);
    $mem->set('key', 'cbam_liangshu!');
    $val = $mem->get('key');
    echo $val;
?>