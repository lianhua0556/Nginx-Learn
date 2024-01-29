# 一.Nginx的多站点配置 #
## 1.1多端口的配置 ##
	编辑主配置文件 nginx.conf
	vim nginx.conf
	

    worker_processes  2;
    events {
       worker_connections  1024;
    }
    http {
       include   mime.types;
       default_type application/octet-stream;
       charset utf-8;
       server {
       listen   80;
       server_name localhost;
       location / {
       root   /html/one;
       index index.html index.htm;
       }

       }
       server {
       listen   81;
       server_name localhost;
       location / {
       root   /html/two;
       index index.html index.htm;
       }

       }
       server {
       listen   82;
       server_name localhost;
       location / {
       root   /html/three;
       index index.html index.htm;
       }
       }
    }

一个server代表一个域名的端口链接

## 2.1多ip的设置 ##
    ifconfig ens33:1 10.0.0.12/24 up
    ifconfig ens33:2 10.0.0.13/24 up
	直接在命令窗口添加即可
设置之后可以通过不同的ip访问(端口不变还是80端口)
## 3.1多域名的设置 ##
    server {
       listen   80;
       server_name a.com;
       location / {
       root   /html/one;
       index index.html index.htm;
       }

       }
       server {
       listen   80;
       server_name b.com;
       location / {
       root   /html/two;
       index index.html index.htm;
       }

       }
       server {
       listen   80;
       server_name c.com;
       location / {
       root   /html/three;
       index index.html index.htm;
       }

       }
    
	server中的server_name是域名
 	##内网访问时需要在hosts解析中添加相应的ip和域名
# 二.include配置文件 #
include理解为引入文件
	#include引入文件代替以前的server配置

	[root@root nginx]# cat nginx.conf
	worker_processes  2;
	events {
	   worker_connections  1024;
	}
	http {
	   include   mime.types;
	   default_type application/octet-stream;
	   charset utf-8;
	   include /etc/nginx/conf.d/*.conf;
	}

	[root@root conf.d]# cat d_com.conf 
	   server {
	   listen   80;
	   server_name d.com;
	   location / {
	   root   /html/four;
	   index index.html index.htm;
	   }
	   }
# 三.nginx日志 #
nginx目录下的logs文件夹下

    日志文件
	access.log #访问日志
    error.log #错误日志