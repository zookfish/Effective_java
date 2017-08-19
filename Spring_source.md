#### IOC容器的实例化
	+ 定位BeanDefinition,从该资源中去加载Bean的定义信息,然后在初始化这些bean。主要看Refresh() 这个方法


#### FactoryBean / BeanFactory
	+ FactoryBean 只是个Bean,用来实现一些bean的实例化的个性化需求
	+ beanFactory 是个bean工厂是IOC容器的基础。很多上下文都是继承这个

#### AOP的实现
	+ 利用java的代理和FactoryBean来实现


#### spring mvc
	+ 服务器启动的时候触发监听器的init事件
		+ 生成根上下文对象，并保存在servletContext中

	+ 初始化dispatherservlet,initStrategies中完成对上传组件(MultipartResolve),请求映射(HandlerMapping),视图解析器的初始化
		+ HandlerMapping的初始化<就是看HandlerMapping接口 #getHandler>
			+ 根据url映射注册handler，生成一个map<url->handler>,加上定义好的interceptor，封装为HandlerExecutionChain
			+ getHandler完成了url到Handler的实例的过程(解析请求url,从map中找到对应的handler.再从容器中拿到对应的实例)
		+ spring对请求的分发处理
			+ dispather本身是HttpServlet的子类,doservice里面处理请求的逻辑(根据url取得handler,调用方法得到modelandview,view里面去做页面渲染的逻辑view.render())
			+ 常见的页面解析