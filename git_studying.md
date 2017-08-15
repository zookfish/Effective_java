### git在学习 again

#### 版本控制的历史
+ 本地建立仓库来管理  缺点:不能多人协作
+ 建立一个中央仓库  svn  缺点:单点故障
+ git分布式管理     每个人的本地是一个仓库,一个中央仓库用来同步

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

+ git branch test 新建分支
+ git branch -d test 删除分支  如果当前head必须指向test
+ git checkout test  切换分支到test
+ git checkout -b test 新建并切换分支
+ git pull origin    
