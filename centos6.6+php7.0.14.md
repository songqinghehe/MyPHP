>系统环境

centos6.6 + VirtualBox + SecureCRT

>虚拟机+本机互通

* 本机查看相关IP ipconfig 记住IPv4地址（192.168.31.84）+默认网关（192.168.31.1）
* 虚拟机查看vi /etc/sysconfig/network-scripts/ifcfg-eth0，修改ONBOOT=yes   NM_CONTROLLED=no
新增IPADDR=192.168.31.69   GATEWAY=192.168.31.1
* 重启服务service network restart
* 设置VirtualBox中的连接方式->网卡一->桥接模式->一共两个选项(wifi / 有线)
* 在虚拟机ping 192.168.31.1 如果能ping通表明两者已经互通
* SecureCRT连接192.168.31.69即可
* 完毕！

>安装包准备

* nginx-1.11.6.tar.gz
* google-perftools-1.6.tar.gz
* libevent-2.0.22-stable.tar.gz
* libiconv-1.13.1.tar.gz
*  libmcrypt-2.5.8.tar.gz
* libmemcached-1.0.18.tar.gz
* m9php-php7.tar.gz
* mcrypt-2.6.8.tar.gz
* memcache-3.0.8.tgz
* memcached-1.4.34.tar.gz
* mhash-0.9.9.9.tar.gz
* pecl-memcache-php7.tar.gz
* php-7.0.14.tar.gz
* php-memcached-master.tar.gz
* phpredis-develop.tar.gz
* protobuf-master.tar.gz

>环境准备

* yum -y install lrzsz（为了运用rz命令把本地包上次上去）
*  yum -y install wget(为了下载包)
*  yum -y install zip unzip(为了解压包)
* yum -y install gcc(安装gcc)
*  yum -y install openssl openssl-devel(安装openssl)
* yum -y install pcre*(安装pcre)
*  yum -y install gcc gcc-c++(安装c++)
* yum -y install libxml2*(安装相关libxml2*)
* yum -y install curl*(安装curl*相关)
* yum -y install curl-devel(安装curl-devel)
* yum -y install libpng*(安装libpng相关)

####相管环境搭建
>安装nginx

tar zxf nginx-1.11.6.tar.gz</br>
cd nginx-1.11.6</br>
./configure --prefix=/usr/local/nginx --user=www --group=www --with-http_stub_status_module --with-http_flv_module --with-http_ssl_module</br>
make && make install</br>

>启动nginx

/usr/local/nginx/sbin/nginx</br>
报错：nginx: [emerg] getpwnam("www") failed</br>
解决方式1：在nginx.conf中 把user nobody的注释去掉既可</br>
解决方式2：/usr/sbin/groupadd -f www			/usr/sbin/useradd -g www www</br>

本机浏览器输入：http://192.168.31.69/</br>
成功标识：Welcome to nginx!</br>
失败标识：考虑关闭防火墙或者打开80端口即可</br>

>安装libiconv

tar zxf libiconv-1.13.1.tar.gz</br>
cd libiconv-1.13.1/</br>
./configure --prefix=/usr/local/libiconv</br>
make && make install</br>

>安装libevent

tar zxvf libevent-2.0.22-stable.tar.gz </br>
cd libevent-2.0.22-stable</br>
./configure --prefix=/usr/local/libevent</br>
make && make test && make install</br>

>安装libmcrypt

tar zxf libmcrypt-2.5.8.tar.gz</br>
cd libmcrypt-2.5.8/</br>
./configure</br>
make && make install</br>

>安装mhash

tar zxf mhash-0.9.9.9.tar.gz</br>
cd mhash-0.9.9.9</br>
./configure</br>
make && make install</br>

>开启软连接

ln -s /usr/local/lib/libmcrypt.la /usr/lib/libmcrypt.la</br>
ln -s /usr/local/lib/libmcrypt.so /usr/lib/libmcrypt.so</br>
ln -s /usr/local/lib/libmcrypt.so.4 /usr/lib/libmcrypt.so.4</br>
ln -s /usr/local/lib/libmcrypt.so.4.4.8 /usr/lib/libmcrypt.so.4.4.8</br>
ln -s /usr/local/lib/libmhash.a /usr/lib/libmhash.a</br>
ln -s /usr/local/lib/libmhash.la /usr/lib/libmhash.la</br>
ln -s /usr/local/lib/libmhash.so /usr/lib/libmhash.so</br>
ln -s /usr/local/lib/libmhash.so.2 /usr/lib/libmhash.so.2</br>
ln -s /usr/local/lib/libmhash.so.2.0.1 /usr/lib/libmhash.so.2.0.1</br>
ln -s /usr/local/bin/libmcrypt-config /usr/bin/libmcrypt-config</br>

>安装memcached

tar -zxvf memcached-1.4.34.tar.gz</br>
cd memcached-1.4.34</br>
./configure --prefix=/usr/local/memcached  --with-libevent=/usr/local/libevent/</br>
make && make install</br>
启动memcached :</br>
/usr/local/memcached/bin/memcached -d -m 100 -u root -l 127.0.0.1 -p 11211 -c 256 -P /tmp/memcached.pid

>安装mcrypt

tar zvxf mcrypt-2.6.8.tar.gz</br>
cd mcrypt-2.6.8</br>
./configure</br>
报错：configure: error: *** libmcrypt was not found （其实我已经安装了libmcrypt）</br>
解决：export LD_LIBRARY_PATH=/usr/local/lib: LD_LIBRARY_PATH</br>
./configure</br>
make && make install</br>

>安装php7

tar zvxf php-7.0.14.tar.gz</br>
cd php-7.0.14</br>
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-fpm  --enable-pcntl --enable-mysqlnd --enable-opcache --enable-sockets --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-shmop --enable-zip --enable-ftp --enable-soap --enable-xml --enable-mbstring --disable-rpath --disable-debug --disable-fileinfo --with-mysqli --with-pdo-mysql --with-pcre-regex --with-iconv --with-zlib --with-mcrypt --with-gd --with-openssl --with-mhash --with-xmlrpc --with-curl --without-pear --enable-fileinfo --with-imap-ssl</br>
make && make install</br>
cp ./php.ini-development /usr/local/php/etc/php.ini</br>
cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf</br>
cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf</br>
cp -R ./sapi/fpm/php-fpm /etc/init.d/php-fpm</br>

>安装libmemcached

tar zxvf libmemcached-1.0.18.tar.gz</br>
cd libmemcached-1.0.18</br>
./configure --prefix=/usr/local/libmemcached  --with-memcached</br>
make && make install</br>

>安装php-memcached扩展

tar xvzf php-memcached-master.tar.gz</br>
cd php-memcached-master</br>
/usr/local/php/bin/phpize</br>
./configure --enable-memcached --with-php-config=/usr/local/php/bin/php-config --with-libmemcached-dir=/usr/local/libmemcached --disable-memcached-sasl</br>
make && make install</br>

>测试php-memcached扩展

vi /usr/local/php/etc/php.ini</br>
添加：extension=memcached.so wq!</br>
启动fpm:/usr/local/php/sbin/php-fpm -R</br>
验证：ps -ef | grep 'fpm'</br>
启动memcached:/usr/local/memcached/bin/memcached -d -m 100 -u root -l 127.0.0.1 -p 11211 -c 256 -P /tmp/memcached.pid</br>
验证：ps -ef | grep 'memcached'</br>
验证是否连接成功</br>
vi /tmp/memcached.php</br>
输入：</br>
<?php

	$m = new Memcached();
	$m->addServer('127.0.0.1', 11211);
	$m->set('int', 99);
	$m->set('string', 'a simple string');
	$m->set('array', array(11, 12));
	$m->set('object', new stdclass, time() + 300);

	var_dump($m->get('int'));
	var_dump($m->get('string'));
	var_dump($m->get('array'));
	var_dump($m->get('object'));
?></br>
wq!</br>
cd /tmp</br>
执行：/usr/local/php/bin/php memcached.php 打印成功即可</br>

>测试php-mysql扩展

vi /tmp/mysql.php</br>
<?php

	$pdo = new PDO("mysql:host=hostname;dbname=databasename","root","");
	$rs = $pdo -> query("select * from test");
	while($row = $rs -> fetch()){
		print_r($row);
	}
?></br>
wq!</br>
cd /tmp</br>
执行：/usr/local/php/bin/php mysql.php 打印成功即可

>安装php-redis扩展

tar xvf phpredis-develop.tar.gz</br>
cd phpredis-develop</br>
/usr/local/php/bin/phpize</br>
./configure --with-php-config=/usr/local/php/bin/php-config</br>
make && make install</br>
vi /usr/local/php/etc/php.ini</br>
输入：extension=redis.so</br>
wq!</br>
查看是否安装成功</br>
/usr/local/php/bin/php -m</br>







