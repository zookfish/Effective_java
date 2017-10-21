
#### nginx知识点整理

+ 编译运行环境
```
	yum -y install gcc gcc-c++ autoconf pcre pcre-devel make automake
	yum -y install wget httpd-tools vim
```
+ 为什么是nginx
	+ io多路复用epoll
		+ epoll与select，poll的区别
			+ epoll 每次轮训的是就绪表,select和poll是轮训所有的fd(文件描述符)
	+ 功能模块少,轻量级
	+ cpu的亲和
	+ sendFile 直接通过内核发送给socket

+ nginx的安装
	+ [安装连接](http://nginx.org/en/linux_packages.html#stable)

	+ rpm -ql nginx 查看yum 方式安装nginx的文件目录
	
	+ nginx -v 查看版本  -V 查看运行的参数
	+ ./usr/sbin/nginx  运行nginx
	+ 常用的目录
		+ /etc/lograte.d/nginx 配置日志输出方式
		+ /etc/nginx/nginx.conf  /etc/conf.d/default.conf 配置文件
		+ /etc/mime.types  设置http Conent-type与对应的数据类型
		+ /usr/lib/systemd/  守护进程启动  针对centos7
		+ /usr/share/nginx  nginx的帮助文档
		+ /var/cache/nginx   用于缓存
		+ /var/log/nginx     nginx的日志目录

+ nginx的基本配置
	+ /etc/nginx/nginx.conf
		+ nginx -t -c /etc/nginx/nginx.conf 检查当前的配置文件是否有语法错误
		+ nginx -s reload /etc/nginx/nginx.conf  重载配置文件

	+ nginx的模块
		+ --with-http_stub_status_module 模块
			+ 配置的语法规则
			```
				context location、server  配置的模块在server或者location下
				stub_staus; 配置的规则
			```

		+ --with-http_sub_module http响应内容的替换
			```
				sub_filter '原始' '替换后'
				sub_filter_once on|off 只替换第一个还是全局替换

				--sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -pie'
			```

		+ -limit_conn_module   连接请求限制

		+ -limit_req_module    request请求限制

		+ http_access_module   ip连接限制  基于客户端的ip,它只能识别访问的ip.如果客户端通过代理访问,服务端只能识别代理的ip
		+ http_x_forworded_for 可以改善http_access_module 的局限
		+ http_auth_basic_module   用户的访问认证


	+ nginx的静态web资源服务
		+ 接受客户端的一些静态资源的请求 (js/css/jpg/txt)
		+ gzip模块设置压缩
		+ 设置cache
		+ 跨域访问    Access-Control-Allow-Origin 请求地址  在服务端设置允许跨域访问的域名
		+ 防盗链   K.;
	+ nginx的代理服务
		+ proxy_pass
		+ 正向代理
		+ 反向代理

	+ nginx的负载均衡

		+ 定义upstream 配置proxy_pass 指向配置的upstream。默认的负载是轮询的策略

	+ nginx的动态缓存


