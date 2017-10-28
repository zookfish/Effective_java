#### IOC容器的实例化
	+ 定位BeanDefinition,从该资源中去加载Bean的定义信息,然后在初始化这些bean。主要看Refresh() 这个方法


#### FactoryBean / BeanFactory
	+ FactoryBean 只是个Bean,用来实现一些bean的实例化的个性化需求
	+ beanFactory 是个bean工厂是IOC容器的基础。很多上下文都是继承这个

#### AOP的实现
	+ 利用java的代理和FactoryBean来实现
		getObject()方法初始化拦截器链,创建了代理对象(jdk动态代理/cglib代理,代理里面通过DedaultAdvisorChainFactory#adapter装饰拦截器链),代理对象调用invoke方法(获取拦截器链,递归调用方法实现通知)
		+ BeforeAdvice
		+ AfterAdvice
	+ pointCut 切点  定义需要通知的点
	+ advisor 把通知advice和pointcut结合起来


#### spring mvc
	+ 服务器启动的时候触发监听器的init事件
		+ 生成根上下文对象，并保存在servletContext中
			+ 准备Applicatition上下文
			+ 初始化BeanFactory
			+ 初始化BeanDefination (解析xml中定义的bean)
			+ 初始化BeanPostProcess 处理器
			+ 初始化MeassageSource 
			+ 完成beanDefination的实例(利用反射,初始化的是非懒加载的bean,这里会调用前面的bean后置处理器,bean执行各种操作 )
			+ 初始化完成 finishRefresh 将实现了Lifecycle接口的bean设置为running
	+ 初始化dispatherservlet,initStrategies中完成对上传组件(MultipartResolve),请求映射(HandlerMapping),视图解析器的初始化
		+ HandlerMapping的初始化<就是看HandlerMapping接口 #getHandler>
			+ 根据url映射注册handler，生成一个map<url->handler>,加上定义好的interceptor，封装为HandlerExecutionChain
			+ getHandler完成了url到Handler的实例的过程(解析请求url,从map中找到对应的handler.再从容器中拿到对应的实例)
		+ spring对请求的分发处理
			+ dispather本身是HttpServlet的子类,doservice里面处理请求的逻辑(根据url取得handler,调用方法得到modelandview,view里面去做页面渲染的逻辑view.render())
			+ 在调用前执行上面封装的interceptor的前置处理handle方法之后执行后置处理
			+ 常见的页面解析