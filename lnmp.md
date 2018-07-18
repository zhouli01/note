# 搭建LNMP环境

## 1、Docker

### （1）、Pull Centos镜像

`docker pull centos:7`

### （2）、运行Centos镜像

`docker run -d -p 8000:8000 centos:7 /sbin/init`

### （3）、进入Centos镜像

```
docker stats  // 查看运行状态

CONTAINER           CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
e3ee577d5e1b        136.24%             133.8MiB / 1.952GiB   6.69%               321MB / 6.43MB      53.2kB / 106MB      11

docker exec -it 49f7960eb7e4 /bin/bash

[root@e3ee577d5e1b /]#

```

### （4）、安装LNMP镜像

[LNMP环境](https://lnmp.org/install.html)

安装wget和LNMP环境

```
[root@e3ee577d5e1b /]# yum install -y wget

[root@e3ee577d5e1b /]# wget http://soft.vpser.net/lnmp/lnmp1.5.tar.gz -cO lnmp1.5.tar.gz && tar zxf lnmp1.5.tar.gz && cd lnmp1.5 && ./install.sh lnmp
```


lnmp使用命令

```
Usage: lnmp {start|stop|reload|restart|kill|status}
Usage: lnmp {nginx|mysql|mariadb|php-fpm|pureftpd} {start|stop|reload|restart|kill|status}
Usage: lnmp vhost {add|list|del}
Usage: lnmp database {add|list|edit|del}
Usage: lnmp ftp {add|list|edit|del|show}
Usage: lnmp ssl add
Usage: lnmp {dnsssl|dns} {cx|ali|cf|dp|he|gd|aws}
```

### 安装yaf扩展

下载

```
wget http://pecl.php.net/get/yaf-3.0.7.tgz

tar -zvxf yaf-3.0.7.tgz

cd yaf-3.0.7

```

编译安装

```
[root@e3ee577d5e1b yaf-3.0.7]# phpize
Configuring for:
PHP Api Version:         20170718
Zend Module Api No:      20170718
Zend Extension Api No:   320170718
```

执行完`phpize`后会生成.configure文件

```
[root@e3ee577d5e1b yaf-3.0.7]# ./configure --with-php-config=/usr/local/php/bin/php-config


[root@e3ee577d5e1b yaf-3.0.7]# make

[root@e3ee577d5e1b yaf-3.0.7]# make install

```

编辑php.ini文件，将yaf.so 添加到php.ini中

```
[root@e3ee577d5e1b ~]# vim /usr/local/php/etc/php.ini

[Yaf]
extension=yaf.so
yaf.environ="product"
```

添加vhost

```
[root@e3ee577d5e1b ~]# lnmp vhots add    // 添加vhost

[root@e3ee577d5e1b ~]# lnmp vhots list   // 列出所有vhost文件
```


修改vhost

```
[root@e3ee577d5e1b ~]# vim /usr/local/nginx/conf/vhost/yaha.com.conf


server
    {
        listen 80;
        server_name yaha.com;
        index index.php;
        root  /home/work/test;

        include enable-php-pathinfo.conf;

	if (!-e $request_filename) {
    	    rewrite ^/(.*)  /index.php/$1 last;
  	}
        access_log  /home/work/log/yaha.com.log.wf;
    }

```