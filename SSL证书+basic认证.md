# Nginx开启basic认证 #
	用户访问敏感文件时设置basic认证
	auth_basic "qstack.com.cn";
    auth_basic_user_file /etc/nginx/htpasswd;
    #/etc/nginx/htpasswd 在该文件内设置账号密码 访问时可以登录

## ssl证书配置 ##

    server {
    #SSL 访问端口号为 443
       listen 443 ssl; 
    #填写绑定证书的域名
       server_name ;#自己的域名 
    #证书文件名称
       ssl_certificate ;#.crt文件的路径 
    #私钥文件名称
       ssl_certificate_key ; #.key文件的路径 
       ssl_session_timeout 5m;#超时时间
      #请按照以下协议配置
       ssl_protocols TLSv1 TLSv1.1 TLSv1.2; 
    	#请按照以下套件配置，配置加密套件，写法遵循 openssl 标准。
       ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; 
       ssl_prefer_server_ciphers on;
       location / {
    #网站主页路径。此路径仅供参考，具体请您按照实际目录操作。
       root html; 
       index index.html index.htm;
       }
    }

## return跳转 ##
作用:http和https的相互跳转以及其他敏感文件的跳转
让用户在切换的时候仍然能保持原来的访问路径

    #使用return跳转
    server {
       access_log off;
       listen   80;
       server_name blog.oldqiang.com;
       location / {
       return   302 https://blog.oldqiang.com$request_uri;
       }
     }
        