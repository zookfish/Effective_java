
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
		
		+ 按需加载 N+1问题  对于级联查询  我查询学生的信息,会把学习关联的成绩也查出来,影响性能
			+ lazyLoadingEnabled 开启懒加载  
			+ aggressiveLazyLoading false取消层级加载
			> 上述两个属性是全局的定义
			+ feachType="lazy/egear"  局部的定义是否延迟加载


		+ 另外一种实现级联效果的方式
			+ 直接编写sql,通过各种连接查询得到想要的结果

		+ cache mybatis缓存
			+ 默认情况下,只会开启一级缓存
				+ 一级缓存的范围是在sqlSession级别,即同一个会话中
			+ 二级缓存 需要手动开启 
				+ <cache eviction="回收策略" flushInterval="刷新间隔" size="缓存大小" />
				+ ***并且映射的pojo要实现序列化接口****
				
				+ 缓存的回收策略 按照过期时间来执行
					+ LRU 最近最少使用
					+ FIFO 先进先出 按顺序移除
					+ SOFT 基于垃圾回收
					+ WEAK 基于垃圾回收和弱引用  相当于 LRU

				+ 自定义缓存实现.上述的缓存都是在本机上,我们可以依赖redis来自定义缓存。只要实现cache接口即可
					


				> 二级缓存是sqlSessionFactory级别的,各个session之间是可以共享的



		+ \#  $
			+ \# \#{} 处理参数
			+ $ 相当于取字符串本身.会有sql安全问题 相当于我定义一个字符串变量,${变量} 就是取值

