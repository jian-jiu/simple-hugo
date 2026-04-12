---
title: nginx
date: 2026-03-09
slug: nginx
categories:
    - nginx
tags:
    - nginx
---

```shell
#配置worker进程运行用户 nobody也是一个linux用户，一般用于启动程序，没有密码
user  nobody;  
#配置工作进程数目，根据硬件调整，通常等于CPU数量或者2倍于CPU数量
worker_processes  1;  

#配置全局错误日志及类型，[debug | info | notice | warn | error | crit]，默认是error
error_log  logs/error.log;  
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

pid        logs/nginx.pid;  #配置进程pid文件 


###====================================================


#配置工作模式和连接数
events {
    worker_connections  1024;  #配置每个worker进程连接数上限，nginx支持的总连接数就等于worker_processes * worker_connections
}

###===================================================


#配置http服务器,利用它的反向代理功能提供负载均衡支持
http {
    #配置nginx支持哪些多媒体类型，可以在conf/mime.types查看支持哪些多媒体类型
    include       mime.types;  
    #默认文件类型 流类型，可以理解为支持任意类型
    default_type  application/octet-stream;  
    #配置日志格式 
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #配置access.log日志及存放路径，并使用上面定义的main日志格式
    #access_log  logs/access.log  main;

    sendfile        on;  #开启高效文件传输模式
    #tcp_nopush     on;  #防止网络阻塞

    #keepalive_timeout  0;
    keepalive_timeout  65;  #长连接超时时间，单位是秒

    #gzip  on;  #开启gzip压缩输出
	
	###-----------------------------------------------
	

		#配置虚拟主机
		server {
        listen       80;  #配置监听端口
        server_name  localhost;  #配置服务名

        #charset koi8-r;  #配置字符集

        #access_log  logs/host.access.log  main;  #配置本虚拟主机的访问日志

		#默认的匹配斜杠/的请求，当访问路径中有斜杠/，会被该location匹配到并进行处理
        location / {
	    #root是配置服务器的默认网站根目录位置，默认为nginx安装主目录下的html目录
            root   html;  
	    #配置首页文件的名称
            index  index.html index.htm;  
        }		

        #error_page  404              /404.html;  #配置404页面
        # redirect server error pages to the static page /50x.html
        #error_page   500 502 503 504  /50x.html;  #配置50x错误页面
        
		#精确匹配
		location = /50x.html {
            root   html;
        }

		#PHP 脚本请求全部转发到Apache处理
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

		#PHP 脚本请求全部转发到FastCGI处理
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

		#禁止访问 .htaccess 文件
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

	
	#配置另一个虚拟主机
    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

	
	#配置https服务，安全的网络传输协议，加密传输，端口443，运维来配置
	#
    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
}
```
## [设置gzip压缩](https://www.cnblogs.com/Renyi-Fan/p/11047490.html)

## web管理（部署后自带nginx无需自己部署）
[nginxwebui](https://www.nginxwebui.cn/)
[nginxproxymanager](https://nginxproxymanager.com/)

# 配置文件
检测
docker exec nginx nginx -t
加载
docker exec nginx nginx -s reload

# nginx之location（root/alias）
[参考](https://www.cnblogs.com/sunjiguang/p/6227518.html)

```shell
upstream kuaifuzhi{
	server 127.0.0.1:8888 down;
	server 127.0.0.1:8889 ;
}

####################   负载均衡（实现前端修改代码不间断运行）   ####################
server {
	listen       8888;
	server_name  127.0.0.1;
	
	location / {
		root   /static/simplevue8888;
		index  index.html index.htm;
		try_files $uri $uri/ /index.html;
	}

	location /prod-api/{
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header REMOTE-HOST $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_pass http://119.23.211.46:8080/;
	}

	location /dev-api/{
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header REMOTE-HOST $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_pass http://119.23.211.46:8080/;
	}
	
	error_page   500 502 503 504  /50x.html;
	location = /50x.html {
		root   html;
	}
}

server {
	listen       8889;
	server_name  127.0.0.1;
	
	location / {
		root   /static/simplevue8889;
		index  index.html index.htm;
		try_files $uri $uri/ /index.html;
	}

	location /prod-api/{
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header REMOTE-HOST $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_pass http://119.23.211.46:8080/;
	}

	location /dev-api/{
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header REMOTE-HOST $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_pass http://119.23.211.46:8080/;
	}
	
	error_page   500 502 503 504  /50x.html;
	location = /50x.html {
		root   html;
	}
}

```

```shell
####################   kuaifuzhi.com   ####################
server {
	listen       80;
	server_name  kuaifuzhi.com;
	return 301 https://$server_name$request_uri;
}
server {
	listen       443 ssl http2;
	server_name  kuaifuzhi.com;
	
	ssl_certificate      /etc/nginx/https/kuaifuzhi.com/full_chain.pem;
	ssl_certificate_key  /etc/nginx/https/kuaifuzhi.com/private.key;
	ssl_session_timeout 5m;
	ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
	ssl_prefer_server_ciphers on;

	#开启和关闭gzip模式
	gzip on;
	#gizp压缩起点，文件大于1k才进行压缩
	gzip_min_length 1k;
	# gzip 压缩级别，1-9，数字越大压缩的越好，也越占用CPU时间
	gzip_comp_level 6;
	# 进行压缩的文件类型。application/vnd.ms-fontobject开始为字体类型
	gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/xml text/javascript application/json image/png image/gif image/jpeg application/vnd.ms-fontobject font/ttf font/x-woff image/svg+xml;
	#nginx对于静态文件的处理模块，开启后会寻找以.gz结尾的文件，直接返回，不会占用cpu进行压缩，如果找不到则不进行压缩
	# gzip_static on|off
	# 是否在http header中添加Vary: Accept-Encoding，建议开启
	gzip_vary on;
	# 设置压缩所需要的缓冲区大小，以4k为单位，如果文件为7k则申请2*4k的缓冲区 
	# 缓冲(压缩在内存中缓冲几块? 每块多大?)
	gzip_buffers 32 4k;
	# 设置gzip压缩针对的HTTP协议版本，默认1.1
	#	gzip_http_version 1.1;
	#配置禁用gzip条件，支持正则。此处表示ie6及以下不启用gzip（因为ie低版本不支持）
	gzip_disable "MSIE [1-6]\.";

	location / { # 指定上游服务器负载均衡服务器
		proxy_pass http://kuaifuzhi;
		index  index.html index.htm;
	}
	
	location ~ .*.simplescript.* {
		root /static;
	}

	location ~ .*.simple.* {
		root /static;
	}
	
#	location ~ .*\.(js|css|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|pdf|xls|mp3|wma|apk) {
#		root /static;
#	}

	location ~ .*dny001.*\.pdf {
		valid_referers none blocked *.kuaifuzhi.com kuaifuzhi.com;
		if ($invalid_referer) {
			#防盗链
			return 404;
			break;
		}
		root /static;
	}
}
```
