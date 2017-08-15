### git在学习 again

#### 版本控制的历史
+ 本地建立仓库来管理  缺点:不能多人协作
+ 建立一个中央仓库  svn  缺点:单点故障
+ git分布式管理     每个人的本地是一个仓库,一个中央仓库用来同步

#### git三个概念
+ 本地文件
+ 暂存区
+ 本地仓库
+ 远端仓库

#### git的常用命令
```
	# 修改全局配置
	git config --global user.name yanhaijing
	git config --global user.email yanhaijing@yeah.net
	# 定义一些快捷键
	git config --global alias.st status #git st
	git config --global alias.co checkout #git co
	git config --global alias.br branch #git br
	git config --global alias.ci commit #git ci	

	# 生成公钥
	ssh-keygen -t rsa -C 861712654@qq.com
	cat ~/.ssh/id_rsa_pub  的内容复制到github的设置中去

```
+ .gitignore 文件将不需要的文件不与远程仓库关联
 + 配置规则
 ```
	# 注释
	.idea  表示.idea 文件夹不被关联
	*.iml  表示以.iml结尾的所有文件不关联
	!.java !表示非  即所有非.java结尾的文件都不关联
	? 代表一个字符
	[] 代表集合
 ```
> 以下origin分支只带的是远程主机   可以使用git remote add XXX 来指定远程主机

+ git branch test 新建分支
+ git branch -d test 删除分支  如果当前head必须指向test
+ git checkout test  切换分支到test
+ git checkout -b test 新建并切换分支
+ git pull origin 远程分支名:本地分支名  将远程分支master与当前master分支进行合并,如果是与当前分支进行合并,则:master可以省略
+ git push  origin master    将本地master分支提交到远程主机master上
+ git remote add 远程主机 url 新增一个远程主机
+ git remote remove 远程主机   删除一个远端
+ git diff file 查看文件差异



+ git commit --amend 继续上一次提交|运行这个命令的前提是你没有push到远端,

+ git reflog 查看最近提交的commit id






