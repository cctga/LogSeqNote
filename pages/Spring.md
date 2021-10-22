alias:: Spring Framework

- 简介
	- 分层，全栈， [[]] [^1]，胶水
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
		- 对象之间的耦合问题
		  collapsed:: true
			- 如 Service 层依赖的是 Dao 层的借口，而不用依赖于具体的实现， ((616ed63a-72d0-4438-9331-f237f8a228ca))
	- DI 是什么
	  collapsed:: true
		- IOC 和 DI 描述的是同一件事情，但是出发点不一样，IOC 从对象角度出发，将对象的控制交给了容器，DI 从容器角度出发，为相互依赖的对象进行依赖的注入
	- Bean 容器管理实现
		- xml 配置 Bean 生成参数
		- ((616ed525-f391-408b-b265-d9e74f36f118)) 读取 xml 配置，并反射创建对象（懒加载，饿加载）
		- 客户端通过 Factory 获取 Bean
- AOP
  id:: 616eb420-adf0-47f7-b0a9-01767e8cb189
  collapsed:: true
	- Aspect oriented Programming 面向切面编程
	- AOP 是什么
	  collapsed:: true
		- 是 [[OOP]] 的延续，横向的方面的抽取共有的逻辑
	- 解决了什么问题
	  collapsed:: true
		- 业务代码和非业务代码解耦
		- 解决非业务代码（日志，鉴权等）重复问题
- 声明式事务控制
  collapsed:: true
	-
-
-
-
-
-
- [^1]: 用来衡量组件对其环境的依赖程度
  如果这个依赖越小，就越轻量;反之就越重量。