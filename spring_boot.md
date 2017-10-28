
#### spring boot note 

+ properties、yaml配置文件
	+ 多环境属性配置文件支持
	+ 数据库配置
	+ jdbc 配置
	+ jpa配置
	+ h2内嵌数据库配置
	+ redis
	+ ecache/redis 缓存配置 
+ log4j、logback日志框架的支持
+ 不同的模板引擎支持 freemarker、thymeleaf
+ 添加自定义的servlet、filter、listener三种方式
+ CORS 跨域支持
+ 事物的支持
	+ 传播属性 7种 
	+ 隔离级别 4种 级别依次变高
		+ 读未提交    脏读 不可重复读 幻读
		>  脏读: 读取到另一个事物未提交的数据
		>  不可重读 : 一个事物内两次读取,另一个事物更新了某条记录,导致两次读不一致 
		>  幻读 : 一个事物两次读物,另一个事物新增了一些记录 ,导致不一致
		+ 读已提交    不可重复读  幻读
		+ 可重复读    幻读
		+ 串行化

+ 整合dubbo

+ spring boot的基本配置
	+ Spring的Java配置方式是通过 @Configuration 和 @Bean 这两个注解实现的：
		+ @Configuration 作用于类上，相当于一个xml配置文件；
		+ @Bean 作用于方法上，相当于xml配置中的<bean>
	+ 自动配置
		+ spring-boot-autoconfiguration  扫描包下面/META/spring.factories 配置的类 完成bean的自动生成  setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class)); 初始化SpringApplication的时候

 


