* *master* 版本用于生产环境  
* *dev* 版本用于开发环境（安装了xdebug扩展，dnsmasq容器)  只适用于linux与mac  
* *windows-dev* 版本用于windows的开发环境

本项目用来基于docker技术快速建立Drupal的开发环境。
其中php-fpm的镜像，来自于自定义镜像：

* [Docker hub => docker-php7](https://hub.docker.com/r/zterry95/docker-php7/)
* [Github Dockerfile => docker-php7](https://github.com/terryzwt/docker-php7)

里面已经包含了执行Drupal所需的基本php扩展，比如gd,pdo_mysql等。

下面主要介绍dev版本，master版本和dev一样，只是少了dnsmasq和xdebug  

### 前置条件
* 安装好docker和docker-compose

### 目录简介

|目录|简介|
| ----- | ----- |
|memcached|包含php memcached的环境|
|redis|包含php reids的环境|
|oci8|包含php oci8+memcached的环境|
|app/web|代码放置目录|
|etc|nginx, php, mariadb, dnsmasq自定义文件的放置目录|
|mariadb-data|mysql数据存放目录，对应/var/lib/mysql/data|

### 安装
* *首要步骤：拷贝 /compose-lnmp-drupal/etc 到你需要的环境版本base或memcached或redis或oci8*  
* 如果需要包含memcached的环境，执行如下命令:  

```bash
git clone git@github.com:terryzwt/compose-lnmp-drupal.git
cd compose-lnmp-drupal/memcached
docker-compose up -d
```
* 如果需要包含redis的环境，执行如下命令：

```bash
git clone git@github.com:terryzwt/compose-lnmp-drupal.git
cd compose-lnmp-drupal/redis
docker-compose up -d
```
* 如果需要包含oci8的环境，执行如下命令：

```bash
git clone git@github.com:terryzwt/compose-lnmp-drupal.git
cd compose-lnmp-drupal/oci8
docker-compose up -d
```

### 代码目录说明
* 代码目录都存放在[memcached|redis|oci8]/app/web，如果该文件夹不存在，则手工创建。
* 例如，需要访问的地址是example.dev,则[memcached|redis|oci8]/app/web/example是该drupal网站的根目录。
* 如果有app/web/test这个目录，则可以通过test.dev可以访问。
* 假如你的drupal网站是composer方式创建的，网站执行目录在web文件夹下，则访问test.devc
* 提醒：*.dev与 *.devc都在etc/dnsmasq/dnsmasq.conf下做了相应的配置，当然你也可以自定义,下面是代码

```bash
#address=/double-click.net/127.0.0.1
address=/dev/127.0.0.1
address=/devc/127.0.0.1
```

### 数据库文件目录说明
* 数据库文件目录是mariadb-data。你可以将已经存在的mariadb数据库文件，软连接过来，命令如下

```bash
rm -r mariadb-data
ln -s /var/lib/mysql mariadb-data
```
* 如果不介意，则可以直接使用该文件夹存储mysql的数据库文件。

### 参考
最终的目录结构如下(假设当前位于[memcached|redis|oci8]目录之一）
<pre>
├── README.md
├── app
│   └── web -> /Users/terry/www
├── docker-compose.yml
├── etc
│   ├── dnsmasq
│   │   └── dnsmasq.cnf
│   ├── mariadb
│   │   │    └──conf.d 
│   │   └── my.cnf
│   ├── nginx
│   │   ├── fastcgi.conf
│   │   ├── fastcgi.conf.default
│   │   ├── fastcgi_params
│   │   ├── fastcgi_params.default
│   │   ├── koi-utf
│   │   ├── koi-win
│   │   ├── mime.types
│   │   ├── mime.types.default
│   │   ├── nginx.conf
│   │   ├── nginx.conf.default
│   │   ├── scgi_params
│   │   ├── scgi_params.default
│   │   ├── servers
│   │   │   ├── drupal8.conf
│   │   │   └── wild-dev.conf
│   │   ├── uwsgi_params
│   │   ├── uwsgi_params.default
│   │   └── win-utf
│   └── php
|___ mysql-data -> /Users/terry/docker/storage/mysql-data

</pre>  

### xdebug

