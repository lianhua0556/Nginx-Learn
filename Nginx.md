# Nginx学习之路 #
## 1.搭建环境 使用centos系统  ##

	1.1在开始安装Nginx之前，首先需要安装一些依赖项，以确保Nginx编译和运行正常。打开终端并执行以下命令：

	yum install -y wget gcc-c++ pcre-devel zlib-devel openssl-devel
	
	1.2下载nginx 1.24.0版本
	wget https://nginx.org/download/nginx-1.24.0.tar.gz
## 2.解压Nginx ##

	2.1解压下载的Nginx源代码包：
	tar -zxvf nginx-1.24.0.tar.gz
## 3.编译和安装 ##
		3.1切换到 Nginx 解压目录
		cd nginx-1.24.0

		3.2编译前的配置和依赖检查
		./configure

		3.3编译安装
		make && make install
## 4.Nginx安装完成后，默认自动创建 /usr/local/nginx 目录 ##

## 5.关闭防火墙设置 ##
		5.1查看防火墙状态
		systemctl status firewalld

		5.2关闭防火墙
		systemctl stop firewalld

		5.3开机禁用防火墙
		systemctl disable firewalld
## 6.运行nginx ##

		6.1进入Nginx的安装目录：
		cd /usr/local/nginx/sbin

		6.2启动Nginx服务器：
		./nginx
## 7.测试是否允许成功 ##
		通过浏览器访问您的服务器的IP地址来验证Nginx服务
## 8.检查nginx状态##
		systemctl status nginx

		**若产生以下报错,则需要配置Nginx服务文件**
    systemctl status nginx.service
    ● nginx.service - The nginx HTTP and reverse proxy server
       Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
       Active: failed (Result: exit-code) since 五 2024-01-26 11:27:34 CST; 2min 11s ago
      Process: 90655 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=1/FAILURE)
      Process: 90653 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
    
    1月 26 11:27:34 localhost.localdomain systemd[1]: Starting The nginx HTTP and reverse proxy server...
    1月 26 11:27:34 localhost.localdomain nginx[90655]: nginx: [alert] could not open error log file: open() "/var/log/nginx/error.log" f...denied)
    1月 26 11:27:34 localhost.localdomain nginx[90655]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    1月 26 11:27:34 localhost.localdomain nginx[90655]: 2024/01/26 11:27:34 [emerg] 90655#90655: open() "/var/log/nginx/error.log" failed...denied)
    1月 26 11:27:34 localhost.localdomain nginx[90655]: nginx: configuration file /etc/nginx/nginx.conf test failed
    1月 26 11:27:34 localhost.localdomain systemd[1]: nginx.service: control process exited, code=exited status=1
    1月 26 11:27:34 localhost.localdomain systemd[1]: Failed to start The nginx HTTP and reverse proxy server.
    1月 26 11:27:34 localhost.localdomain systemd[1]: Unit nginx.service entered failed state.
    1月 26 11:27:34 localhost.localdomain systemd[1]: nginx.service failed.
    Hint: Some lines were ellipsized, use -l to show in full



	8.1在 /etc/systemd/system/ 目录下创建一个新的服务文件，例如 nginx.service：
		vi /etc/systemd/system/nginx.service
添加以下内容：

    [Unit]
    Description=Nginx HTTP Server
    After=network.target
    [Service]
    Type=forking
    ExecStart=/usr/local/nginx/sbin/nginx
    ExecReload=/usr/local/nginx/sbin/nginx -s reload
    ExecStop=/usr/local/nginx/sbin/nginx -s stop
    PrivateTmp=true
    [Install]
    WantedBy=multi-user.target
执行以下命令重新加载 systemd 配置文件：

    systemctl daemon-reload
重启Nginx服务

		systemctl restart nginx

如果你希望 Nginx 在系统启动时自动启动，可以执行以下命令设置开机自启动：

    systemctl enable nginx
这样，Nginx 将在系统启动时自动启动。