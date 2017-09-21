
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
			```


