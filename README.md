### apache和nginx处理机制区别
1. apache是select轮询机制；  
2. nginx是rpoll事件机制；
### linux安装软件方式
1. 编译安装；  
1).配置 ./configure 或 cmake；  
2).编译 make；  
3).编译安装 make install；  
2. 源安装（yum install nginx）；
### linux常用命令
1. netstat -nlpt（查看端口）
### install nginx
1. wget http://nginx.org/download/nginx-1.12.2.tar.gz  
2. tar -zxvf nginx-1.12.2.tar.gz  
3. /root/nginx-1.12.2/configure --prefix=/usr/local/nginx
4. make
5. make install
6. /usr/local/nginx/sbin/nginx 
7. ps aux | grep nginx  
8. 备注：启动./nginx 停止./nginx -s stop 重启./nginx -s reload  
9. 修改配置文件：/usr/local/nginx/conf/nginx.conf  
location ~ \.php {  
fastcgi_pass    127.0.0.1:9000;  
fastcgi_index   /index.php;  
include         /usr/local/nginx/conf/fastcgi_params;  
fastcgi_split_path_info            ^(.+\.php)(/.+)$;  
fastcgi_param   PATH_INTO          $fastcgi_path_info;  
fastcgi_param   PATH_TRANSLATED    $document_root$fastcgi_path_info;  
fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;  
}
### install php
1. wget http://hk1.php.net/get/php-7.2.4.tar.gz/from/this/mirror  
2. tar -zxvf mirror
3. /root/php-7.2.4/configure --prefix=/usr/local/php --enable-fpm
4. make  
5. make install
6. /usr/local/php/sbin/php-fpm
7. cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
8. /usr/local/php/sbin/php-fpm
9. cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf  
10. /usr/local/php/sbin/php-fpm  
11. ps aux | grep php-fpm
### install mysql
1. wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.21.tar.gz  
2. tar -zxvf mysql-5.7.21.tar.gz  
3. /root/mysql-5.7.21/cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/mydata/mysql/data \
-DSYSCONFDIR=/etc \
-DMYSQL_USER=mysql \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_MEMORY_STORAGE_ENGINE=1 \
-DWITH_READLINE=1 \
-DMYSQL_UNIX_ADDR=/var/run/mysql/mysql.sock \
-DMYSQL_TCP_PORT=3306 \
-DENABLED_LOCAL_INFILE=1 \
-DENABLE_DOWNLOADS=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DEXTRA_CHARSETS=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_DEBUG=0 \
-DMYSQL_MAINTAINER_MODE=0 \
-DWITH_SSL:STRING=bundled \
-DWITH_ZLIB:STRING=bundled 
