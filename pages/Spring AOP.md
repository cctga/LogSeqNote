- Aspect oriented Programming 面向切面编程
- AOP 是什么
	- 是 [[OOP]] 的延续，横向的方面的抽取共有的逻辑
- 解决了什么问题
	- 业务代码和非业务代码解耦
	- 解决非业务代码（日志，鉴权，性能监控等）重复问题
- 实现方式
	- ((616ed5a4-cb44-4fe2-839e-3ecb9a7c055f))
	- 默认情况下，如果有接口，Spring 会优先使用 JDK 代理，如果没有接口，就会使用 CGLIB 代理
	- 可以使用配置来全部使用 CGLIB 代理
		- ((82a3c07b-6fd3-4b37-a873-b4daab7c7e1d)) 中的参数
- 术语/要素
	- 连接点
	  alias:: join point
		- 用来指定方法中的位置
		- 5 种连接点
			- 执行前，beforeMethod
				- 可以获取入参，通过 JoinPoint 对象参数来获取
			- 执行后，afterMethod
			- 正常结束，afterReturningMethod
				- 可以获取返回值，通过 JoinPoint 对象参数来获取，或使用参数 `returning` 来指定一个参数接受返回值
			- 抛出异常结束，afterThrowingMethod
				- 可以获取异常，，通过 （JoinPoint，Throwable） 两个对象参数来获取
			- 环绕，aroundMethod
				- 可以使用 ProceedingJoinPoint 对象参数
				- 可以控制执行逻辑
				- 可以替代其他所有类型的连接点
				- 为了逻辑清晰，尽量不要和其他连接点一起使用
	- 切入点
	  alias:: point cut
		- 具体影响哪些方法，用来指定方法
		- 使用 ((617fe5ba-66e6-47ae-805a-dd0cef3abf27)) 来指定
		- 指定的时候可以使用 `||` 或 `&&` 连接多个 execution
	- 通知/增强
	  alias:: advice
		- 要添加的功能，用来指定增强逻辑，选择不同的连接点，有不同的通知，如前置通知（通知+方法开始时的连接点），后置通知（通知+方法结束时的连接点）等
	- 织入
	  alias:: weaving
		- 动词，描述的是生成增强对象的过程，spring 使用动态代理织入
	- 切面
	  alias:: aspect
		- 切入点（指定方法）+连接点（指定方法的位置）+增强（要添加的功能）
- 具体使用
	- 配置
		- 纯 xml
			- ```xml
			  <!--开启 AOP-->
			  <aop:aspectj-autoproxy/>
			  
			  <bean id="aop" class="top.maoyilan.config.SpringAop"/>
			  
			  <aop:config>
			    <aop:pointcut id="all" expression="execution(* *..*(..))"/>
			    <aop:aspect ref="aop">
			      <aop:before method="after" pointcut-ref="all"/>
			    </aop:aspect>
			  </aop:config>
			  ```
		- xml+注解
			- 使用 xml 开启 AOP
			- 配合 ((8522f4d8-31d8-4164-8431-be8439590c8d))
		- 纯注解
			- 两个注解配合
			- ((82a3c07b-6fd3-4b37-a873-b4daab7c7e1d))
			- ((8522f4d8-31d8-4164-8431-be8439590c8d))