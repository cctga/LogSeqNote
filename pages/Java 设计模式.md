- 设计原则 7 #card
  card-last-interval:: -1
  card-repeats:: 1
  card-ease-factor:: 2.5
  card-next-schedule:: 2021-10-30T16:00:00.000Z
  card-last-reviewed:: 2021-10-30T06:42:06.269Z
  card-last-score:: 1
	- 单一职责原则
	  card-last-interval:: -1
	  card-repeats:: 1
	  card-ease-factor:: 2.5
	  card-next-schedule:: 2021-10-24T16:00:00.000Z
	  card-last-reviewed:: 2021-10-24T12:13:30.473Z
	  card-last-score:: 1
	- 开闭原则
	- 依赖倒置原则（面向接口）
	  id:: 616ed63a-72d0-4438-9331-f237f8a228ca
	- 迪米特法则
	- 合成复用原则（组合复用原则）
	- 接口隔离原则
	- 里氏替换原则
- 创建模式 5 #card
  card-last-interval:: -1
  card-repeats:: 1
  card-ease-factor:: 2.5
  card-next-schedule:: 2021-10-30T16:00:00.000Z
  card-last-reviewed:: 2021-10-30T06:42:20.225Z
  card-last-score:: 1
	- 单例模式
	  collapsed:: true
	  id:: 616ed514-1c55-4800-b435-88b83ef07b26
		- 饿汉
		- 懒汉
		- 双重校验锁
		  collapsed:: true
			- [[volatile]]
			- 防止 [[构造函数溢出]]
		- 内部静态类
		- 枚举
	- 原型模式
	  id:: 616ed51f-88ad-4d32-8eb6-a69d0c9f451d
	- 工厂模式  
	  id:: 616ed525-f391-408b-b265-d9e74f36f118
	  alias:: Facotry
	  collapsed:: true
		- 静态工厂
		- 实例工厂
		- 解耦
		  collapsed:: true
			- 工厂可以自己选择一个实现类来实例化返回
			  collapsed:: true
			  ```java
			  public class Factory {
			    public static Service getService(){
			      return new ServiceImpl();
			    }
			  }
			  
			  // 使用
			  // 使用者不知道具体的实现类细节
			  // 依赖倒置，面向接口编程
			  // 在修改实现类的时候不影响使用者，只需要修改工厂就好
			  Service service = Factory.getService();
			  ```
	- 抽象工厂
	- 建造者模式
- 结构模式 7 #card
  card-last-interval:: -1
  card-repeats:: 1
  card-ease-factor:: 2.5
  card-next-schedule:: 2021-10-30T16:00:00.000Z
  card-last-reviewed:: 2021-10-30T06:42:29.388Z
  card-last-score:: 1
	- 代理模式
		- 静态代理
		- 动态代理
		  id:: 616ed5a4-cb44-4fe2-839e-3ecb9a7c055f
			- ASM
			- CGLIB
			  id:: 616ed5aa-445f-4b02-a7d6-38dd034dbf87
				- 代理类就是委托对象的子类，所以不需要接口
				- pom 导入
				- ```xml
				  <!-- https://mvnrepository.com/artifact/cglib/cglib -->
				  <dependency>
				      <groupId>cglib</groupId>
				      <artifactId>cglib</artifactId>
				      <version>3.3.0</version>
				  </dependency>
				  
				  ```
				- ```java
				  Enhancer.create(Class<?> targetClz, MethodInterceptor mi);
				  ```
				-
			- JDK
			  id:: 616ed5b1-2b24-4af0-b189-42e13665039e
				- 委托类需要有一个接口，代理类就是一个接口的实现类
				- collapsed:: true
				  ```java
				  Proxy.newProxyInstance(ClassLoader loader, 
				                   	   Class<?>[] interfaces,
				                         InvocationHandler h);
				  ```
				-
			- Javassist
	- 装饰模式
	- 组合模式
	- 适配器模式
	- 桥接模式
	- 外观模式
	- 享元模式
- 行为模式 11 #card
  card-last-interval:: -1
  card-repeats:: 1
  card-ease-factor:: 2.5
  card-next-schedule:: 2021-10-30T16:00:00.000Z
  card-last-reviewed:: 2021-10-30T06:42:23.295Z
  card-last-score:: 1
	- 观察者模式
	- 中介者模式
	- 状态模式
		- [状态模式](https://juejin.cn/post/6844904005336825863)
	- 命令模式
	- 模版方法模式
	- 策略模式
	- 责任链模式
	- 迭代器模式
	- 访问者模式
	- 备忘录模式
	- 解释器模式