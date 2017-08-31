
### mybatis源码学习
---

#### 入门
+ mybatis几个重要的类
	+ SqlSessionFactoryBuilder
		+ 构建 SqlSessionFactory
	+ SqlSessionFactory
		+ 用来创建SqlSession 单例
	+ SqlSession 相当于一个Connection对象
		+ 每次请求完数据库就应该释放

+ mybatis-config.xml的配置属性
	+ properties
	+ settings 
	+ typeAliases  
		+ 通常通过包扫描会将类的名称改为小写作为别名
	+ typeHandle 做一些类型处理(javatype->jdbcType | jdbcType ->javatype)

	