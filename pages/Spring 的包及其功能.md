- spring-beans
  collapsed:: true
	- IOC 管理器，注册生成 Bean，以及 DI，包含 xml 配置和 注解内容
- spring-context
  collapsed:: true
	- 上下文管理器
	- 注解扫描，注解 ((11c084cf-03ed-4d67-81a5-d582d4da1245))
	  collapsed:: true
		- collapsed:: true
		  ```xml
		  <context:component-scan base-package="top.maoyilan"/>
		  ```
	- property 资源引入工具，注解 ((4a74e5c0-622f-403f-9826-5b16e71f2982))
	  collapsed:: true
		- id:: ec9417b1-f6f2-46ef-aae4-794e95d3df67
		  collapsed:: true
		  ```xml
		  <context:property-placeholder location="classpath:jdbc.properties"/>
		  
		  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
		    <!-- 使用 ${} 引入 properties 文件中的键 -->
		    <property name="driverClassName" value="${jdbc.driver}"/>
		    <property name="url" value="${jdbc.url}"/>
		    <property name="username" value="${jdbc.username}"/>
		    <property name="password" value="${jdbc.password}"/>
		  </bean>
		  ```
- spring-web
  collapsed:: true
	- web 项目支持模块
	- Bean 的 web 相关的生命周期
	- web 项目中 IOC 容器的开启
	  collapsed:: true
		- ((2275c93b-96cf-454a-ae61-5de52725f43d))
- spring-test
  collapsed:: true
	- spring 对 `junit` 的支持
	- 依赖 [[Junit]] ，可以引入 ((617fdb6d-05db-4b1c-9cb6-5c045bc7be78))  或 ((617fdb69-8a1c-4e05-83e3-7a010c2d2a77))
	- ((b975c2d7-9de9-47cb-b555-5d3f4abfef2b)) ，@ContextConfiguration，@RunWith 等
- spring-aop
  collapsed:: true
	- 支持 Spring 的 AOP 编程
	- 依赖 [[Aspect]]，需要单独引入
	- @EnableAspectJAutoProxy，@Aspect 等
- spring-tx
  collapsed:: true
	- 支持 Spring 的事务控制
	- @Transactional 等
- spring-jdbc
  collapsed:: true
	- Spring JDBC 的工具包
	- `JdbcTemplate` 工具等
- spring-webmvc
	- Spring MVC 功能包 [[SpringMVC]]
	- 很上层的包，依赖 beans， context，web 等