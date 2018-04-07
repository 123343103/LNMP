### apache和nginx处理机制区别
1.apache是select轮询机制；  
2.nginx是rpoll事件机制；
### linux安装软件方式
1.编译安装；  
1).配置 ./configure 或 cmake；  
2).编译 make；  
3).编译安装 make install；  
2.源安装（yum install nginx）；
### install nginx
1. wget http://nginx.org/download/nginx-1.12.2.tar.gz  
2. tar -zxvf nginx-1.12.2.tar.gz  
3. ./configure
4. make
5. make install
6. ./nginx
