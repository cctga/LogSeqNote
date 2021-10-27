alias:: Spring Framework

- 简介
  collapsed:: true
	- 分层，全栈，[[轻量级]]，胶水
	- [spring 官网](https://spring.io)
	- Spring 部分项目
		- Spring Framework
		  collapsed:: true
			- 核心
		- Spring Boot
		  collapsed:: true
			- Spring 项目脚手架
		- Spring Cloud
		  collapsed:: true
			- 微服务
		- Spring Data
		  collapsed:: true
			- 数据源管理
- Spring 优势
  collapsed:: true
	- 解耦，简化（单例，文件解析）
	- AOP
	- 声明式事务支持 `@Transactional`
	- 方便测试
	- 集成优秀框架
	- 降低 JavaEE API 的使用难度
	- 优秀的源码
- 核心结构
	-
- IOC
	- 什么是 IOC
	  collapsed:: true
		- Inversion of Control 控制反转
		- 优化 Java 对象的创建管理问题，对象统一交给 Spring 管理，由 Spring 容器控制 Java 对象（Bean）
	- 解决了什么问题
	  collapsed:: true
		- 对象之间的耦合问题
			- 如 Service 层依赖的是 Dao 层的借口，而不用依赖于具体的实现， ((616ed63a-72d0-4438-9331-f237f8a228ca))
	- DI 是什么
	  collapsed:: true
		- IOC 和 DI 描述的是同一件事情，但是出发点不一样，IOC 从对象角度出发，将对象的控制交给了容器，DI 从容器角度出发，为相互依赖的对象进行依赖的注入
	- Bean 容器管理实现
		- xml 配置 Bean 生成参数
		- [Factory](((616ed525-f391-408b-b265-d9e74f36f118))) 类读取 xml 配置，并反射创建对象（懒加载，饿加载）
		- 客户端通过 Factory 获取 Bean
	- 具体使用
	  collapsed:: true
		- 使用方式
			- 纯 xml，全部的 bean 都用 xml 配置
			- xml + 注解
			- 纯注解，全部的 bean 都用注解配置
		- 使用环境
			- SE，也就是单机应用，非 Web
			- Web，有 Web 容器，如 [[tomcat]]
		- spring ioc 的功能包 `spring-context`，web 功能包 `spring-web`，后者方便 ioc 在 web 项目中的使用
			- ```xml
			  <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
			  <dependency>
			      <groupId>org.springframework</groupId>
			      <artifactId>spring-context</artifactId>
			      <version>5.3.12</version>
			  </dependency>
			  
			  <!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
			  <dependency>
			      <groupId>org.springframework</groupId>
			      <artifactId>spring-web</artifactId>
			      <version>5.3.12</version>
			  </dependency>
			  
			  ```
		- 如何读取 XML 或 注解，根据 XML 或 注解生成 Bean
			- IOC 的顶层容器，也就是 Bean 管理工具：`BeanFactory`
			- 具体的实现
				- ClassPathXmlApplicationContext
					- 手动加载 applicationContext.xml 文件
					- ```java
					  ApplicationContext ac = 
					    	new ClassPathXmlApplicationContext("applicationContext.xml");
					  ac.getBean(Bean.class);
					  ```
				- FileSystemXmlApplicationContext
					- 从文件系统中加载 applicationContext.xml 文件
					- ```java
					  String filePath = "/User/cctga/Document/applicationContext.xml";
					  ApplicationContext ac = 
					    	new ClassPathXmlApplicationContext(filePath);
					  ac.getBean(Bean.class);
					  ```
				- AnnotationConfigApplicationContext
					- 加载 java 的配置类
					- ```java
					  ```
				- ContextLoaderListener 使用在 Web 项目中
					- 使用 spring-web 包中的 ContextLoaderListener 来配合使用上述的 IOC 容器，在 web 容器(如 [[tomcat]])启动完成后自动启用 IOC 容器
					- 在 web.xml 文件中配置
					- ```xml
					  <!-- 配置文件的路径 -->
					  <context-param>
					    <param-name>contextConfigLocation</param-name>
					    <param-value>/WEB-INF/applicationContext.xml</param-value>
					  </context-param>
					  
					  <listener>
					    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
					  </listener>
					  ```
					- 在 Web 项目中从 IOC 容器中获取 Bean
						- 可以使用 Spring 提供的工具类 WebApplicationContextUtils，通过 [[ServletContext]] 对象来获取 IOC 容器
						- ```java
						  WebApplicationContext wpc = 
						    WebApplicationContextUtils.getWebAPplicationContext(request.getSession().getServletContext());
						  wpc.getBean(Bean.class);
						    
						  ```
	- 实例化 IOC 的三种方式
	  collapsed:: true
		- 无参构造器，普通而广泛
			- 调用 Bean 的无参构造器来生成 Bean
			- ```xml
			  <bean id="beanId" class="top.maoyilan.beans.Bean" />
			  ```
		- 后两种方法一般用于将自己生成的对象放入 IOC 容器中，如一些配置文件
		- 静态方法，工厂获取
			- 自动调用工厂类的静态方法 getBean 来生成 Bean
			- ```xml
			  <bean id="beanId" class="top.maoyilan.factory.BeanFactory" 
			        factory-mothod="getBean"/>
			  ```
		- 实例化方法，实例化工厂
			- 适用于工厂类中的获取方法不是静态方法的实例化工厂，先实例化工厂对象，然后调用非静态的 getBean 方法
			- ```xml
			  <bean id="beanFactory" class="top.maoyilan.factory.BeanFactory"/>
			  <bean id="beanId" factory-bean="beanFactory" factory-method="getBean"/>
			  ```
	- Bean 标签属性
		- id
		- class
		- name 给 Bean 提供多个名称，允许重名
		- factory-bean
		- factory-method
		- scope
		- init-method
		- destroy-method 销毁方法，对象需要销毁时回调，如 scope=singleton 的 bean 在容器销毁时会回调，scope=prototype 的 bean 不会管理 bean 的生命周期，也就不会回调
	- scope 定义 Bean 的作用范围
	  collapsed:: true
		- singleton ((616ed514-1c55-4800-b435-88b83ef07b26)) IOC 容器中总是唯一的
		- prototype ((616ed51f-88ad-4d32-8eb6-a69d0c9f451d))，总是不唯一，每次获取都生成一个新的对象，获取后 IOC 不再管理 Bean 的生命周期，不负责销毁
		- web 应用中才会用到的范围，不过现在都前后端分离，几乎不用了
			- request 将 Bean 存放在 ServletRequest 上下文中
			- session 将 Bean 存放在 HttpSession 中
			- application 将 Bean 存放在 ServletContext 中，本质上和 singleton 没有本质区别
			- websocket 不清楚
- AOP
  id:: 616eb420-adf0-47f7-b0a9-01767e8cb189
	- Aspect oriented Programming 面向切面编程
	- AOP 是什么
		- 是 [[OOP]] 的延续，横向的方面的抽取共有的逻辑
	- 解决了什么问题
		- 业务代码和非业务代码解耦
		- 解决非业务代码（日志，鉴权等）重复问题
	- 实现方式
		- ((616ed5a4-cb44-4fe2-839e-3ecb9a7c055f))
- 声明式事务控制
  collapsed:: true
	- 使用 ((616eb420-adf0-47f7-b0a9-01767e8cb189)) 配合 JDBC 的提交回滚来实现
	- 结构
		- `TransactionManager`
			- `ThreadLocal<Connection>`
		- 业务方法
			- `TransactionManager.beginTransaction`
			- 业务逻辑
			- 业务结束
				- `TransactionManager.commit`
				- `TransactionManager.rollback`
	- 无论是 [[Mybatis]] 还是 [[Spring]]，和 [[数据库]] 的交互都是通过 [[JDBC]] 的，所以控制事务其实就是通过 [[JDBC]] 来控制事务
- 注解
  collapsed:: true
	- @Autowired
		- 根据类型注入
	- @Resource
		- 先根据类型注入，然后根据名称注入
	- @Autowired 和 @Resource 比较
		- @Resource 比较方便，自动读取变量应用名称然后注入相同 id 的 Bean
		- @Resource 属于 JDK 的注解，@Autowired 属于 Spring
		- @Autowired 混淆的时候不用保留变量引用的名称
	- @Qualifier
		- 指定注入 Bean 的 id
	- @Scope
	- @ApplicationScope
	- @SessionScope
	- @RequestScope
		- 指定 Bean 的作用范围，生命周期，默认 singleton
		- 参数
			- value(scpleName) 指定使用的 Bean 的范围
				- WebApplicationContext.SCOPE_APPLICATION="application"
				- WebApplicationContext.SCOPE_REQUEST="request"
				- WebApplicationContext.SCOPE_SESSION="session"
	- singleton （默认）
	- prototype
	- proxyMode 是否使用代理生成 Bean，默认 NO，不使用
		- ScopedProxyMode.DEFAULT(NO)
		- ScopedProxyMode.INTERFACES 基于接口，也就是 ((616ed5b1-2b24-4af0-b189-42e13665039e))
		- ScopedProxyMode.TARGET_CLASS 子类，也就是 ((616ed5aa-445f-4b02-a7d6-38dd034dbf87))
	-
-