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
1. netstat -nlpt（查看端口占用情况）  
2. firewall-cmd --list-ports（查看开放的端口）
### install mysql
1. rpm -qa | grep mariadb  
2. rpm -e --nodeps mariadb-libs-5.5.56-2.el7.x86_64  
3. yum -y install gcc gcc-c++ cmake ncurses-devel  
4. groupadd mysql  
5. useradd -r -g mysql -s /bin/false mysql  
6. mkdir -p /usr/local/mysql  
7. mkdir -p /data/mysql  
8. cd /usr/local/src  
9. wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-boost-5.7.21.tar.gz  
10. tar -zxvf mysql-boost-5.7.21.tar.gz  
11. cd mysql-5.7.21  
12. cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/data/mysql \
-DSYSCONFDIR=/etc \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_MEMORY_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BOOST=boost  
13. make
14. make install  
13. cd /usr/local/mysql  
14. bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql  
15. #MvVr,sen2ty  
16. cp support-files/mysql.server /etc/init.d/mysql  
17. service mysql start  
18. ln -s /usr/local/mysql/bin/mysql /usr/bin  
19. mysql -u root -p  
20. #MvVr,sen2ty
21. set password for 'root'@'localhost'=password('root');   
22. grant all privileges on *.* to 'root'@'%' identified by 'root';  
23. flush privileges;  
24. exit  
25. firewall-cmd --zone=public --add-port=3306/tcp --permanent  
26. systemctl restart firewalld  
27. systemctl enable mysql  
备注：cmake失败时，需rm CMakeCache.txt   
vi /etc/firewalld/zones/public.xml  
explicit_defaults_for_timestamp=1  
在华为云上要设置安全组，开放3306端口。  
### install php
1. yum -y install gcc gcc-c++ libxml2-devel 
2. cd /usr/local/src  
3. wget http://cn2.php.net/get/php-7.2.4.tar.gz/from/this/mirror  
4. tar -zxvf mirror  
5. cd php-7.2.4  
6. ./configure --prefix=/usr/local/php \
   --enable-fpm \
   --with-mysqli=/usr/local/mysql/bin/mysql_config \
   --with-pdo-mysql=/usr/local/mysql  
7. make  
8. make install  
9. cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm  
10. chmod 777 /etc/init.d/php-fpm  
11. cd /usr/local/php
12. cp etc/php-fpm.conf.default etc/php-fpm.conf  
13. cp etc/php-fpm.d/www.conf.default etc/php-fpm.d/www.conf  
14. touch /usr/local/php/var/run/php-fpm.pid  
15. vi /usr/local/php/etc/php-fpm.conf 启用pid = run/php-fpm.pid
16. service php-fpm start
17. vi /etc/profile  
    在文件末尾添加如下两行：  
    PATH=$PATH:/usr/local/php/bin  
    export PATH  
18. source /etc/profile  
19. chkconfig --add php-fpm  
20. chkconfig php-fpm on
### install nginx
1. yum -y install gcc gcc-c++ pcre-devel zlib-devel  
2. cd /usr/local/src
3. wget http://nginx.org/download/nginx-1.12.2.tar.gz  
4. tar -zxvf nginx-1.12.2.tar.gz  
5. cd nginx-1.12.2  
6. ./configure --prefix=/usr/local/nginx
7. make
8. make install  
9. vi /etc/init.d/nginx  
   打开连接https://www.nginx.com/resources/wiki/start/topics/examples/redhatnginxinit/复制脚本  
   将 # pidfile:     /var/run/nginx.pid 修改为 # pidfile:     /usr/local/nginx/logs/nginx.pid  
   将 nginx="/usr/sbin/nginx" 修改为 nginx="/usr/local/nginx/sbin/nginx"  
   将 NGINX_CONF_FILE="/etc/nginx/nginx.conf" 修改为 NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"  
10. chmod 777 /etc/init.d/nginx  
11. 启动service nginx start 停止service nginx stop  
12. chkconfig --add nginx
13. chkconfig nginx on  
14. firewall-cmd --zone=public --add-port=80/tcp --permanent
15. systemctl restart firewalld
16. 配置PHP：打开/usr/local/nginx/conf/nginx.conf配置文件中的PHP代码，并将/scripts修改为$document_root  
17. 备注：启动/usr/local/nginx/sbin/nginx 停止/usr/local/nginx/sbin/nginx -s stop 重启/usr/local/nginx/sbin/nginx -s reload 
