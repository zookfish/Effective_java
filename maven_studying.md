
### MAVAN实战学习笔记

#### maven的安装
+ ln -s src target  创建一个符号链接指向s
+ %mvn%/conf/setting.xml 是全局的配置  ./m2/settings.xml 是用户级别的配置
+ mvn help::System

----

#### maven基本命令
+ maven clean compile 编译
+ maven clean test    测试
+ maven clean package 打包
+ maven clean install 安装到本地仓库
> 上述每一个命令都会运行前一个 eg:mvn clean install 会先编译->测试->打包->安装到本地仓库
+ 使用 mvn archetype:generate 命令生成基本的maven工程骨架
+ 号外 : java -jar 文件名     运行jar文件

----

#### maven的依赖配置
+ <groupId></groupId>        项目
+ <artifactId></artifactId>  模块
+ <version></version>        版本
+ <type></type>      不用配置默认为jar
+ <scope></scope>    依赖范围
	+ compile  编译依赖范围,对编译测试运行都有效
	+ test     测试依赖范围,只对测试的时候有效,编译和运行均无效
	+ provided 已提供依赖范围,对测试和编译有效,运行无效  eg:servlet-api 运行的容器已经提供
	+ runtime  运行时依赖范围,对测试和运行有效,编译无效  eg:jdbc驱动,编译时质押jdk的接口即可
	+ system   系统依赖范围, 跟provided一样,往往jar包不是通过maven仓库下载,是本地的jar包,要配合systemPath一起使用
		```xml
			<scope>system</scope>
			<systemPath>XXXXX</systemPath>
		```

+ <optional></optional>      依赖是否可选
+ <exclusions><exclusion></exclusion></exclusions>  排除依赖传递
+ maven的传递性依赖
	+ A依赖B,B依赖C,A是B的第一直接依赖,B是C的第二直接依赖
			<table>
    			<tr>
        			<td></td>
        			<td>compile</td>
        			<td>test</td>
        			<td>provided</td>
        			<td>runtime</td>
    			</tr>
    			<tr>        			
        			<td>compile</td>
        			<td>test</td>
        			<td>----</td>
        			<td>----</td>
        			<td>runtime</td>
    			</tr>
    			<tr>
        			<td>test</td>
        			<td>test</td>
        			<td>----</td>
        			<td>----</td>
        			<td>test</td>
    			</tr>
    			<tr>
        			<td>provided</td>
        			<td>provided</td>
        			<td>----</td>
        			<td>provided</td>
        			<td>provided</td>
    			</tr>
    			<tr>
        			<td>runtime</td>
        			<td>runtime</td>
        			<td>----</td>
        			<td>----</td>
        			<td>runtime</td>
    			</tr>
			</table>
	+ 依赖调解
		+ 路径最近者优先
		+ 第一声明者优先(如果依赖路径一样长,则以pom文件的声明顺序,最前面的优先)	
	+ 排除依赖
		+ 排除某些依赖的jar,项目直接引入
	+ 归类依赖
		+ <properties>标签同意jar包version
	+ 查看依赖
		+ mvn dependency:list  列出所有依赖
		+ mvn dependency:tree  一树状结构列出
		+ mvn dependency:analyze 分析依赖

----

#### 远程仓库
+ pom默认会继承一个超级pom,配置了远程仓库地址,想用其它的远程仓库,在pom中自己定义如下
	```xml
		<repositories>
			<repository>
				<id>唯一id</id>
				<name>描述信息</name>
				<url>远程地址</url>
				<releases>
					<enabled>true</enabled>
				</releases>
				<snapshots>
					<enabled>false</enabled>
				</snapshots>
			</repository>
		</repositories>
	```
+ 远程仓库认证 (一般在公司用私服需要配置setting.xml)
	```
		<servers>
			<server>
				<id>与上面仓库的id对应</id>
				<username></username>
				<password></password>
			</server>
		</servers>
	```
+ 部署到远程仓库:配置如下 执行 mvn clean deploy 命令
	```
		<distributionManagement>
			<repository>
				<id>repository唯一id</id>
				<name>描述</name>
				<url>地址</url>
			</repository>
			<snapshotRepository>
				<id></id>
				<name></name>
				<url></url>
			</snapshotRepository>
		</distributionManagement>
	```
+ 仓库镜像
	+ 阿里云公共镜像 百度即可得到

----

#### mavan生命周期和插件
+ 三套生命周期
	+ clean生命周期  清理项目
		+ pre clean阶段  清理前的工作
		+ clean阶段      清理的工作
		+ post clean阶段 清理后的工作
	+ default生命周期 构建项目
		+ validate
		+ initialize
		+ generate-sources 
		+ process-sources 处理主目录的资源文件 src/main/resources
		+ generate-resources
		+ process-resources
		+ compile 编译主目录下的java文件到主classpath路径
		+ process-classes
		+ generate-test-sources
		+ process-test-sources
		+ generate-test-resources
		+ process-test-resources
		+ test-compile 编译项目的测试代码
		+ process-test-classes
		+ test 使用单元测试框架运行测试,测试代码不会被打包部署
		+ prepare-package 
		+ package 接受编译好的代码,打包成可发布的格式
		+ pre-integration-test
		+ integration-test   集成测试
		+ post-integration-test
		+ verify
		+ install 安装到本地maven仓库
		+ deploy 部署到远程仓库
	+ site生命周期   建立项目站点
		+ pre-site 站点生成之前的工作
		+ site 生成项目站点
		+ post-site 生成之后的工作
		+ site-deploy 生成的站点发布到服务器上
	> 三个生命周期互不干涉,每个生命周期的阶段都会执行前面的阶段. eg: **clean阶段 会执行 pre-clean clean**.Maven的基本命令都是基于这些不同生命周期的不同阶段组合而成的！

+ 插件绑定
	+ 插件与maven定义的生命周期阶段绑定,由插件来执行相应的任务,插件可以用来指定不用的目标。
	```xml
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-antrun-plugin</artifactId>
			<version>1.3</version>
			<!--  -->
			<executions>
				<!-- 一个execution对应一个特定阶段配置 -->
				<execution>
					<id>ant-validate</id>
					<phase>validate</phase>
					<goals>
						<goal>run</goal>
					</goals>
					<configuration>
						<tasks>
							<echo>bound to validate phase</echo>
						</tasks>
					</configuration>
				</execution>
				<execution>
					<id>ant-verify</id>
					<phase>verify</phase>
					<goals>
						<goal>run</goal>
					</goals>
					<configuration>
						<tasks>
							<echo>bound to verify phase</echo>
						</tasks>
					</configuration>
				</execution>
			</executions>
		</plugin>
	```

+ 插件解析机制
	+ 同样也有插件仓库  <pluginRepoeitories></pluginRepoeitories>,maven会从这些仓库去下载插件。maven插件前缀也是通过metadate.xml来解析出完整的插件名称 [详情](http://repo1.maven.org/maven2/org/apache/maven/plugins/)

----

#### 聚合和继承
+ 聚合: 为了多个模块编译
+ 继承: 将各个模块之间的依赖抽离出来,方便公用管理版本
	+ dependencyManagement/pluginManagement 配置的依赖都是方便管理版本,如果子工程自己不引入里面配置的dependency,就不会引入该jar包。所以该引的还是得引入。其实就是避免了子工程继承父工程,引入一些不需要的jar包

----

#### nexus 私服
+ 在nexus官网下载unix版本,在虚拟机中解压.进入bin 运行./nexus start。然后访问 localhost:8081/nexus。登陆admin/admin123 即可看到管理界面
	+ 若访问不到 考虑端口是否开放。
	+ 提示需要licence key填写一个正确的邮箱即可收到
+ nexus 模块
	+ 仓库 Repositories
		+ hosted 宿主仓库    用来放置内部的jar
		+ proxy  代理仓库    代理访问外部的jar包
		+ group  仓库组      添加仓库到组下,方便几个仓库一起对外提供服务

	+ 本地maven配置私服地址
	```
    	<!-- 配置本地maven私服 -->
    	<profile>
    		<id>nexus</id>
    		<repositories>
    		    <repository>
    		      <id>nexus</id>
    		      <name>Nexus</name>
    		      <url>http://192.168.237.128:8081/nexus/content/groups/public/</url>
    		      <releases>
    		        <enabled>true</enabled>
    		      </releases>
    		      <snapshots>
    		        <enabled>true</enabled>
    		      </snapshots>
    		    </repository>
    		</repositories>
    		<pluginRepositories>
    		    <pluginRepository>
    		      <id>nexus</id>
    		      <name>Nexus</name>
    		      <url>http://192.168.237.128:8081/nexus/content/groups/public/</url>
    		      <releases>
    		        <enabled>true</enabled>
    		      </releases>
    		      <snapshots>
    		        <enabled>true</enabled>
    		      </snapshots>
    		    </pluginRepository>		
    		</pluginRepositories>	
    	</profile>

    	<!-- 激活该profile -->
    	<activeProfiles>
    		<activeProfile>nexus</activeProfile>
  		</activeProfiles>

  		<!-- 配置镜像全部从nexus私服下载 -->
  		<mirror>
      		<id>nexus</id>
      		<name>nexus</name>
      		<url>http://192.168.237.128:8081/nexus/content/groups/public/</url>
      		<mirrorOf>*</mirrorOf>        
    	</mirror>
	```
	+ nexus私服搜索、上传jar包,权限管理

----

#### maven集成测试
+ maven-surefire-plugin
	```
		<plugin>
			<groupId>org.apache.maven.plugin</groupId>
			<artifactId>maven-surefire-plugin</artifactId>
			<version>2.3</version>
			<configuration>
				<!-- 跳过运行测试用例 -->
				<skip>true</skip>
			<!-- 包含哪些测试用例 -->
				<includes>

					<include>**/*tests.java</include>
				</includes>
				<!-- 排除哪些测试用例 -->
				<excludes>
					<exclude>**/*XXX.java</exclude>
				</excludes>
			</configuration>
		</plugin>
	```




