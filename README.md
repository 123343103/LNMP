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
26. systemctl enable mysql  
备注：cmake失败时，需rm CMakeCache.txt   
vi /etc/firewalld/zones/public.xml  
explicit_defaults_for_timestamp=1  
rpm -qa | grep mariadb  
rpm -e --nodeps mariadb-libs-5.5.56-2.el7.x86_64  
在华为云上要设置安全组，开放3306端口。  
### install php
1. yum -y install gcc gcc-c++ libxml2-devel 
2. cd /usr/local/src  
3. wget http://hk1.php.net/get/php-7.2.4.tar.gz/from/this/mirror  
4. tar -zxvf mirror  
5. cd php-7.2.4  
6. ./configure --prefix=/usr/local/php --enable-fpm  
7. make  
8. make install  
9. cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm  
10. chmod 777 /etc/init.d/php-fpm  
11. touch /usr/local/php/var/run/php-fpm.pid  
12. vi /usr/local/php/etc/php-fpm.conf 启用pid
13. service php-fpm start
14. vi /etc/profile  
    在文件末尾添加如下两行：  
    PATH=$PATH:/usr/local/php/bin  
    export PATH  
15. source /etc/profile  
16. chkconfig --add php-fpm  
17. chkconfig php-fpm on
### install nginx
1. yum -y install gcc gcc-c++ pcre-devel  
2. cd /usr/local/src
3. wget http://nginx.org/download/nginx-1.12.2.tar.gz  
4. tar -zxvf nginx-1.12.2.tar.gz  
5. cd nginx-1.12.2  
6. ./configure --prefix=/usr/local/nginx
7. make
8. make install
9. /usr/local/nginx/sbin/nginx 
10. ps aux | grep nginx  
11. 启动/usr/local/nginx/sbin/nginx 停止/usr/local/nginx/sbin/nginx -s stop 重启/usr/local/nginx/sbin/nginx -s reload  
12. vi /etc/init.d/nginx  
    打开连接https://www.nginx.com/resources/wiki/start/topics/examples/redhatnginxinit/复制脚本
    修改nginx="/usr/sbin/nginx"为nginx="/usr/local/nginx/sbin/nginx"  
    修改NGINX_CONF_FILE="/etc/nginx/nginx.conf"为NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"  
13. chmod 777 /etc/init.d/nginx
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
