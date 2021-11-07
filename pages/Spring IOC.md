- 什么是 IOC
  title:: Spring IOC
	- Inversion of Control 控制反转
	- 优化 Java 对象的创建管理问题，对象统一交给 Spring 管理，由 Spring 容器控制 Java 对象（Bean）
	- [[Spring DI]]
- 解决了什么问题
	- 对象之间的耦合问题
		- 如 Service 层依赖的是 Dao 层的借口，而不用依赖于具体的实现， ((616ed63a-72d0-4438-9331-f237f8a228ca))
- Bean 容器管理实现
	- xml 配置 Bean 生成参数
	- [Factory](((616ed525-f391-408b-b265-d9e74f36f118))) 类读取 xml 配置，并反射创建对象（懒加载，饿加载）
	- 客户端通过 Factory 获取 Bean
- 具体使用
  id:: 9b8ef439-4fe2-4425-9a2b-aeb370ef51e4
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
			- FileSystemXmlApplicationContext
			- AnnotationConfigApplicationContext
			- web 使用
				- ContextLoaderListener
				- AnnotationConfigWebApplicationContext
					- 用于纯注解形式的 web
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
	  id:: 5422c2f9-b7a6-4717-8738-9749087b7338
		- 加载 java 的配置类，配置了 ((a097a055-ec1d-40e8-b0de-5ccc5c5ff585)) 注解
		  id:: a69155c0-599e-41ed-987d-d1086bd3e5b8
		- id:: fa71f88c-58ee-43ef-a464-df0931e953f8
		  ```java
		  ApplicationContext applicationContext = 
		    	new AnnotationCOnfigApplicationContext(SpringConfig.class);
		  ```
	- ContextLoaderListener 
	  id:: 2275c93b-96cf-454a-ae61-5de52725f43d
		- 使用在 Web 项目中，有 AnnotationConfigApplicationContext 和 ClassPathXmlApplicationContext 两种方式，对应于有 xml 配置和纯注解的 spring ioc
		- 使用 spring-web 包中的 ContextLoaderListener 来配合使用上述的 IOC 容器，在 web 容器(如 [[tomcat]])启动完成后自动启用 IOC 容器
		- 在 web.xml 文件中配置，根据 web 项目版本不同配置会有差异
			- 有 xml 配置的
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
			- 纯注解的
				- 除了告诉 web 容器配置文件（SpringConfig.class）的位置外，还要声明是注解配置
					- ```xml
					  <!-- 额外声明这是注解的配置 -->
					  <context-param>
					    <param-name>contextClass</param-name>
					    <param-value>org.springframework
					      .web.context.support
					      .AnnotationConfigWebApplicationContext
					    </param-value>
					  </context-param>
					  
					  <!-- 配置类的路径 -->
					  <context-param>
					    <param-name>contextConfigLocation</param-name>
					    <param-value>top.maoyilan.config.SpringConfig</param-value>
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
- 实例化 IOC 的四种方式
	- 无参构造器，普通而广泛
		- 调用 Bean 的无参构造器来生成 Bean
		- ```xml
		  <bean id="beanId" class="top.maoyilan.beans.Bean" />
		  ```
	- 静态工厂和实例化工厂
		- > 两种方法一般用于将自己生成的对象放入 IOC 容器中，如一些配置文件
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
	- 注解生成
		- {{embed ((9db28d70-9a63-4e90-bfd5-34b8f969a6c7)) }}
- Bean 标签属性
	- id
	- class
	- name
		- 给 Bean 提供多个名称，允许重名
	- factory-bean
	- factory-method
	- scope
	- init-method
	- destroy-method
		- 销毁方法，对象需要销毁时回调，如 scope=singleton 的 bean 在容器销毁时会回调，scope=prototype 的 bean 不会管理 bean 的生命周期，也就不会回调
- scope 定义 Bean 的作用范围
	- singleton
		- ((616ed514-1c55-4800-b435-88b83ef07b26)) IOC 容器中总是唯一的
	- prototype
		- ((616ed51f-88ad-4d32-8eb6-a69d0c9f451d))，总是不唯一，每次获取都生成一个新的对象，获取后 IOC 不再管理 Bean 的生命周期，不负责销毁
	- web
		- 应用中才会用到的范围，不过现在都前后端分离，几乎不用了
	- request
		- 将 Bean 存放在 ServletRequest 上下文中
	- session
		- 将 Bean 存放在 HttpSession 中
	- application
		- 将 Bean 存放在 ServletContext 中，本质上和 singleton 没有本质区别
	- websocket
		- 不清楚
- 高级特性
	- lazy-init 延迟加载，默认 false
		- 只适用于 singleton bean
		- 如果为 true ，则在容器启动时不会创建 Bean，而是在使用的时候再创建
		- 应用场景
			- 提高容器启动速度
			- 对于可能完全使用不到的 Bean 使用
	- FactoryBean 和 BeanFactory
		- 用于构建那些构建过程复杂的 Bean，可以自定义普通 Bean 的生成逻辑
			- FactoryBean，Bean 有两种，普通 Bean 和 FactoryBean
				- 负责生成普通 Bean，通过自定义 FactoryBean，可以自定义普通 Bean 的生成逻辑
				- ```java
				  public interface FactoryBean<T> {
				    String OBJECT_TYPE_ATTRIBUTE = "factoryBeanObjectType";
				  
				    @Nullable
				    // 自定义构建过程，返回构建完成的 Bean
				    T getObject() throws Exception;
				  
				    @Nullable
				    // 返回 Bean 的类型
				    Class<?> getObjectType();
				  
				    // 是否是单例
				    default boolean isSingleton() {
				      return true;
				    }
				  }
				  ```
			- 例子
				- 实现 FactoryBean 接口，getObject 返回 Company 的实例
				- ```java
				  public class CompanyFactoryBean implements FactoryBean<Company> {
				     @Nullable
				    Company getObject() throws Exception {
				      return new Company();
				    }
				  
				    @Nullable
				    Class<Company> getObjectType(){
				      return Company.class;
				    }
				  }
				  ```
				- 配置
				- ```xml
				  <bean id="companyBean" class="top.maoyilan.factory.CompanyFactoryBean"/>
				  ```
				- 这将会生成两个实例，`CompanyFactoryBean(id=&companyBean)` 实例和 `Company(id=companyBean)` 实例
		- |id|实例|
		  |&companyBean|CompanyFactoryBean 实例|
		  |companyBean|Company 实例 |
	- 后置处理器
		- > 扩展接口：用来在 Bean 完成之后做一些额外的处理
		- BeanPostProcessor
			- 在普通 Bean 初始化完成后做一些处理
			- 组成了 Bean 声明周期的一部分
			- ```java
			  
			  @Configuration
			  public class MyBeanPostProcessor implements BeanPostProcessor {
			  
			    @Override
			    public Object postProcessBeforeInitialization(Object bean, String beanName)
			        throws BeansException {
			      if (beanName.equals("bean")) {
			  
			        System.out.println("== BeanPostProcessor postProcessBeforeInitialization is run");
			      }
			      // 这里可以直接返回 bean
			      // 但是这样可以进行多层的处理
			      return BeanPostProcessor.super.postProcessBeforeInitialization(bean, beanName);
			    }
			  
			    @Override
			    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
			      if (beanName.equals("bean")) {
			  
			        System.out.println("== BeanPostProcessor postProcessAfterInitialization is run");
			      }
			      return BeanPostProcessor.super.postProcessAfterInitialization(bean, beanName);
			    }
			  }
			  ```
		- BeanFactoryPostProcessor
			- 在普通 FactoryBean 初始化完成后做一些处理，回调方法的参数 configurableListableBeanFactory 内容非常多，其中就有封装了所有 Bean 配置的 [[BeanDefinition]] 对象，可以对其进行修改
			- ```java
			  @Configuration
			  public class MyBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
			    @Override
			    public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {
			      System.out.println("== BeanFactoryPostProcessor postProcessBeanFactory is run");
			    }
			  }
			  ```
			- Spring 中的使用例子
				- {{embed ((ec9417b1-f6f2-46ef-aae4-794e95d3df67))}}
				- 其中的 `${jdbc.driver}` SpringEL 配置就是在 postProcessBeanFactory 方法中被替换成相应的值的
- [[Bean 的生命周期]]
- [[Spring Aware]]
- [[Spring 循环依赖问题]]
- [[Spring IOC 源码剖析]]