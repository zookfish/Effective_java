
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
		+ EnumTypeHandle 枚举的映射Handler
	+ environments 环境 配置数据源
	
+ mybatis核心组件映射器
	+ 映射器标签
		+ select / update / insert / delete
			+ 查询里面的一些子属性
				+ id 与mapper的namespace组合,全局唯一
				+ parameterType  类全限定名  别名
				+ resultType     类全限定名  别名
				+ resultMap      自定义映射规则
				+ flushCache     查询之前是否清空之前查询的本地缓存和为二级缓存
				+ useCache       启动二级缓存的开关  
				
				+ keyProperty    指定那个列为主键
				+ useGeneratedKeyes   是否使用数据库内部生成的主键回填我们插入的数据喽
				> mybatis里面的别名 类名的首字母小写/mybatis内部自己定义的 
		+ sql 定义一个部分的sql,在其它标签里面做引用
			+ 就是封装一些常用的字段,结合<include refid=""/> 简化sql
		+ resultMap 从数据库的结果集与定义的pojo做映射
			+ 用来做级联
				+ association 一对一的级联关系
				+ collection  一对多的级联关系
		+ #  $
			+ # #{} 处理参数
			+ $ 相当于取字符串本身.会有sql安全问题 相当于我定义一个字符串变量,${变量} 就是取值

