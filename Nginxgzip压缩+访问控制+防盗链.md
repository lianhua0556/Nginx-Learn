# nginx gzip压缩 #
作用:节省流量

	gzip on; #开启gzip压缩
    gzip_min_length 1k; #最小压缩文件
    gzip_buffers 4 32k;  #内存缓冲
    gzip_http_version 1.1;   #http版本
    gzip_comp_level 9;   #压缩等级
    gzip_types text/html text/css text/xml application/javascript;  #压缩w类型
    gzip_vary on;#http响应头添加gzip标识
    gzip_disable "MSIE [1-7]\.";   #遇到IE浏览器1-7取消gzip压缩

# Nginx访问控制 #
作用:限制ip访问

    主配置文件中设置
    allow
    deny

# 防盗链 #
    location / {
     if ( $http_referer !~* '域名' ) {
     rewrite 302 http://192.168.18.25/gun.html;
	#rewrite跳转，相当于触发超链接
     } 
       }