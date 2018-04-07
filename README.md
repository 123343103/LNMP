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
