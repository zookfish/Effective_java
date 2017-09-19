
#### nginx知识点整理

+ 编译运行环境
```
	yum -y install gcc gcc-c++ autoconf pcre pcre-devel make automake
	yum -y install wget httpd-tools vim
```
+ 为什么是nginx
	+ io多路复用epoll
	+ 功能模块少,轻量级
	+ cpu的亲和
	+ sendFile 直接通过内核发送给socket

+ nginx的安装
	+ [安装连接](http://nginx.org/en/linux_packages.html#stable)

	+ rpm -ql nginx 查看yum 方式安装nginx的文件目录

	+ ./usr/sbin/nginx  运行nginx

