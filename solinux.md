
### 就该这么玩linux

> 工作马马虎虎,只想在兴趣和游戏中寻觅快活,充其量只能获得一时的快感,决不能尝到心底涌出的惊喜和快乐,但来自工作的喜悦并不像糖果那样---放进嘴里就甜味十足,而是需要从苦劳与艰辛中渗出,因此我们聚精会神,孜孜不倦,克服艰辛后的成就感,世界上没有哪种喜悦可以类比

+ yum 工具 简化rpm安装的工具<可以自动分析出软件包所需要的依赖>
	+ yum repolist all 列出所有可用的yum仓库
	+ yum list all 列出仓库所有可用的软件包
	+ yum install/reinstall/update/remove 包名称  安装/重新安装/更新/移除
	+ yum grouplist  系统安装的软件包组

> 内核用来驱动硬件,分配资源,管理活动。不能直接让用户来操作,所以开发人员需要基于系统的调用接口来操作内核,控制计算机

+ 常用的按键操作
	+ 空格向下翻页
	+ page-up 向上翻页
	+ /关键字  从上向下搜索
	+ ?关键字  从下向上搜索
	+ n 定位到下一个关键字
	+ N 上一个关键字

+ wget 下载文件
+ uname -a  查看系统内核版本
+ uptime 查看系统负载
+ free -m 查看内存运行状况  free -g
+ who 查看当前系统的登陆状况
+ last 系统所有的登录记录

+ 查看文件命令
	+ cat -n 文件名  显示行数查看
	+ cat -b 排除空行显示行数查看
	+ more -数字 以多少航为一页查看
	+ head -n 20 查看文件前20行
	+ tail -n 20 查看文件后20行
	+ tail -f 持续刷新显示文件 <查看日志很管用>

+ 文件操作
	+ touch 
	+ mkdir 创建目录
	+ cp 源文件 目标文件  复制   cp -r 递归复制
	+ mv 移动文件

+ 用户管理
	+ useradd/userdel/usermod 添加/删除/修改 用户
	+ groupadd 添加组

+ 压缩文件
	+ tar -xzvf 解压
	+ tar -czvf 压缩

+ 查找
	+ grep 查找内容
		+ grep -A 3 -i "example" demo_file 在文件中查找关键字以及之后的三行
		+ grep -r "example" * 递归查找
	+ find 地址 -参数 查找文件
		+ find -iname "a.js"   查找名称为a.js的文件
 	+ > 输出   man bash > 1.txt   man bash >> 1.txt 追加输入
	+ < 输入   wc -l < 1.txt
	+ tail 查看
		+ tail -n 300 demo.txt 查看文件的最后300行
		+ tail -f demo.txt  实时查看

+ 杂货
	+ echo `uname -a`  反引号里面的会被linux当做命令来执行
	+ \$ 会转译


+ 查看磁盘和内存、进程	
	+ df    查看磁盘使用情况   df -k -h
	+ free  查看内存使用
	+ ps -efH 查看
	+ kiss -9 进程id 杀死进程
	  
+ linux的一切都是文件，命令也是文件。执行一条命令
	+ 如果以绝对/相对路径输入命令则直接执行
	+ 检查命令是否是别名命令
	+ 由bash判断是内部命令还是外部命令
	+ 通过$PATH环境变量中定义的路径去做查找
	+ export 命令将定义好的变量定义为全局范围


+ vim 命令模式|末行模式|编辑模式
	+ 命令模式快捷键
		+ dd 删除光标所在行
		+ 5dd 删除光标所在的5行
		+ yy 复制光标所在行  5yy
		+ p 复制刚刚删除的或者复制的行
		+ /字符串  搜索字符串（从上往下）  ?字符串 -->从下往上

	+ 末行模式
		+ w 保存
		+ q 退出
		+ q! 强制退出（放弃修改）

+ [ -x test2.sh ] 判断test2.sh是否是可执行的  注意[]前后都有一个空格
	```
		#!/bin/bash
		ping -c 3 -i 0.2 -W 3 $1 &> /dev/null
		# $? 表示上一条命令执行的结果
		if [ $? -eq 0 ]
		then
		echo "HOST $1 up"
		else
		echo "HOST $1 down"
		fi
	```

+ at 时间 创建一个一次性的任务
+ crontab -e 创建定时任务
	+ * * * * * cron表达式的格式  分钟 小时 日期 月份 星期<星期的取值是0-7,0和7都表示周日>


+ 理解linux的文件权限
	+ 原理：文件有三种权限
		+ r 4  可读
		+ w 2  可写
		+ x 1  可运行
 	+ 文件描述符的第一位可以表示：
		+ -普通文件
		+ d 目录
		+ p 管道文件
		+ l 连接文件
		+ c 字符设备文件
		+ b 块状设备文件
	```
	 # - 普通文件  第一个-
	 # rw- 文件所有者可读可写  这里的-就是个占位符
	 # r-- 文件所在的用户组可读
	 # r-- 其它人可读
	 -rw-r--r-- 
	
	```
	+ 授权操作
		+ chown oracle:dba demo.sh 	将demo.sh文件授权给oracle用户dba组
		+ chmod ug+rwx demo.sh  同时改变文件所属用户和组以及读写执行权限



+ linux文件目录
	+ boot开机启动
	+ dev 外接设备
	+ etc 配置文件
	+ home 用户主目录
	+ bin 基本命令
	+ lib 函数库
	+ sbin 开机需要用到的
	+ media 挂在目录
	+ opt 放置三方软件
	+ root 系统管理员文件
	+ srv 网络服务数据
	+ tmp 临时文件目录
	+ usr 用户自定义目录
	+ var 经常变化的目录  日志文件
+ mount 文件系统  挂载目录
+ unmount 文件系统


+ 拾忆
	+ 自定义开放端口 
		+ /etc/sysconfig/iptables
		+ service iptables restart 使修改的端口生效
	+ 查看端口占用 
		+ netstat -anp
	+ centos默认使用firewall-cmd来管理端口
		+ 添加端口  firewall-cmd --zone=public --add-port=80/tcp --permanent
		+ firewall-cmd --reload 重启
		+ firewall-cmd --list -ports 查看开放的端口


+ needsomething todo

	 