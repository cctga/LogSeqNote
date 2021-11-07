- 准备
	- Github 下载源码 5.1.x
	- Idea 配置 [[Gradle]]
		- Spring 源码采用 Gradle
	- 导入源码
	- 编译工程
		- 编译方法
			- Gradle-> tasks-> other-> compileTestJava
		- 依次编译
			- core
			- oxm
			- context
			- beans
			- aspects
			- aop
- 容器初始化主流程
	- 入口
		- ((9b8ef439-4fe2-4425-9a2b-aeb370ef51e4))
		- ```java
		  String source = "classpath:applicationContext.xml";
		  ApplicationContext ac = 
		    new ClassPathXmlApplicationContext(source);
		  ```
-
- [[BeanFactory 初始化获取]]
- [[BeanDefinition 读取注册]]
- [[包的扫描]]