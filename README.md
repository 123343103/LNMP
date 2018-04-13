### apache和nginx处理机制区别
1. apache是select轮询机制；  
2. nginx是rpoll事件机制；
### linux安装软件方式
1. 编译安装；  
1).配置 ./configure 或 cmake；  
2).编译 make；  
3).编译安装 make install；  
4).备注：源代码保存位置 /usr/local/src，软件安装位置 /usr/local
2. 源安装（yum install nginx）；
### linux常用命令
1. netstat -nlpt（查看端口）
### install nginx
1. cd /usr/local/src
2. wget http://nginx.org/download/nginx-1.12.2.tar.gz  
3. tar -zxvf nginx-1.12.2.tar.gz  
4. /usr/local/src/nginx-1.12.2/configure --prefix=/usr/local/nginx
5. make
6. make install
7. /usr/local/nginx/sbin/nginx 
8. ps aux | grep nginx  
9. 备注：启动./nginx 停止./nginx -s stop 重启./nginx -s reload  
10. 修改配置文件：/usr/local/nginx/conf/nginx.conf  
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
1. cd /usr/local/src
2. wget http://hk1.php.net/get/php-7.2.4.tar.gz/from/this/mirror  
3. tar -zxvf mirror
4. /usr/local/src/php-7.2.4/configure --prefix=/usr/local/php --enable-fpm
5. make  
6. make install
7. /usr/local/php/sbin/php-fpm
8. cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
9. /usr/local/php/sbin/php-fpm
10. cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf  
11. /usr/local/php/sbin/php-fpm  
12. ps aux | grep php-fpm
### install mysql
1. yum -y install gcc gcc-c++ cmake ncurses-devel  
2. groupadd mysql  
3. useradd -r -g mysql -s /bin/false mysql  
4. mkdir -p /usr/local/mysql  
5. mkdir -p /data/mysql  
6. cd /usr/local/src  
7. wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-boost-5.7.21.tar.gz  
8. tar -zxvf mysql-boost-5.7.21.tar.gz  
9. cd mysql-5.7.21  
10. cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/data/mysql \
-DSYSCONFDIR=/etc \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_MEMORY_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BOOST=boost  
11. make
12. make install  
13. cd /usr/local/mysql  
14. bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql  
15. #MvVr,sen2ty  
16. cp support-files/mysql.server /etc/init.d/mysql  
17. service mysql start  
18. ln -s /usr/local/mysql/bin/mysql /usr/bin  
19. mysql -uroot -p#MvVr,sen2ty  
20. set password for 'root'@'localhost'=password('root');  
21. grant all privileges on *.* to 'root'@'%' identified by 'root';  
22. flush privileges;  
23. exit  
24. firewall-cmd --zone=public --add-port=3306/tcp --permanent  
25. systemctl restart firewalld   
备注：cmake失败时，需rm CMakeCache.txt   
vi /etc/firewalld/zones/public.xml  
explicit_defaults_for_timestamp=1  
rpm -qa | grep mariadb  
rpm -e --nodeps mariadb-libs-5.5.56-2.el7.x86_64
