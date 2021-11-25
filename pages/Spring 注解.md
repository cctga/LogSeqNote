- @Autowired
  id:: 16e999fd-8479-457f-86aa-d2b262180114
  collapsed:: true
	- 根据类型注入，如果有相同类型的多个 Bean，可以配合 ((46a6510a-00e5-413c-9e48-17ea3904e0ff))
- @Resource
  id:: ff88a13e-0451-4886-b74a-5ba170a9d133
  collapsed:: true
	- JDK 11 已移除，要使用可以导入 `javax.annotation-api` 包
	- 先根据名称注入，然后根据类型注入，如果有相同类型的多个 Bean，可以使用参数 name 指定 Bean 的 id，也可以配合 ((46a6510a-00e5-413c-9e48-17ea3904e0ff))
- @Autowired 和 @Resource 比较
  id:: 066a9d79-1292-42ed-b9d1-e2294764ca26
  collapsed:: true
	- @Resource 比较方便，自动读取变量应用名称然后注入相同 id 的 Bean
	- @Resource 属于 JDK 的注解，@Autowired 属于 Spring
	- @Autowired 混淆的时候不用保留变量引用的名称
- @Qualifier
  id:: 46a6510a-00e5-413c-9e48-17ea3904e0ff
  collapsed:: true
	- 指定注入 Bean 的 id
- @Value
  id:: 47abb64b-9663-4043-bbe0-2ce9881a32e9
  collapsed:: true
	- 注入 SpringEL 的值
	- 相比于 ((16e999fd-8479-457f-86aa-d2b262180114)) 或 ((ff88a13e-0451-4886-b74a-5ba170a9d133)) 更加强大，允许使用 SpringEL 表达式
	- 下面是 `#{}` 和 `${}` 两者的用法
		- `${}` 简单的获取 properties 中的内容
		- ```java
		  // 获取 properties 配置文件中的值
		  @Value("${jdbc.dirver}")
		  private String driverClassName;
		  ```
		- `#{}` 使用 springEL 表达式
		- ```java
		  // 获取 Bean 的属性
		  @Value("#{user.name}")
		  private String name;
		  
		  // 获取一个直接量
		  @Value("#{1}")
		  private int number;
		  
		  // 调用 bean 的方法
		  @Value("#{user.getLostLife()}")
		  private int lostLife;
		  
		  // 使用运算符
		  @Value("#{1+2}")
		  private int sum;
		  
		  // 三元表达式
		  @Value("#{ok?1:2}")
		  private int times;
		  
		  // 链式调用，？判断是否为 null，防止空指针
		  @Value("#{user.getMoney()?.getNumber()}")
		  private int money;
		  ```
	- 支持基本的 **松散绑定**
		- ```java
		  @Value("#{user.first-name}")
		  private String firstName;
		  ```
		- 以下的几种情况都可以成功注入
			- user.first-name
			- user.firstName
- @Scope
  collapsed:: true
	- @ApplicationScope
	- @SessionScope
	- @RequestScope
	- 指定 Bean 的作用范围，生命周期，默认 singleton
	- 参数
	  collapsed:: true
		- value(scpleName) 指定使用的 Bean 的范围
		  collapsed:: true
			- WebApplicationContext.SCOPE_APPLICATION="application"
			- WebApplicationContext.SCOPE_REQUEST="request"
			- WebApplicationContext.SCOPE_SESSION="session"
	- singleton （默认）
	- prototype
	- proxyMode 是否使用代理生成 Bean，默认 NO，不使用
	  collapsed:: true
		- ScopedProxyMode.DEFAULT(NO)
		- ScopedProxyMode.INTERFACES 基于接口，也就是 ((616ed5b1-2b24-4af0-b189-42e13665039e))
		- ScopedProxyMode.TARGET_CLASS 子类，也就是 ((616ed5aa-445f-4b02-a7d6-38dd034dbf87))
- @Component
  id:: e67811d5-ec43-489b-b567-16158f8fc5eb
  collapsed:: true
	- IOC 注册生成 Bean
- @Controller
  collapsed:: true
	- 同 @Component
- @Service
  collapsed:: true
	- 同 @Component
- @Repository
  collapsed:: true
	- 同 @Component
- @Bean
  collapsed:: true
	- 讲一个方法的返回对象作为 Bean
- @Configuration
  id:: a097a055-ec1d-40e8-b0de-5ccc5c5ff585
  collapsed:: true
	- 配置类注解，也是一个 @Component，多了一些参数
	- 在纯注解 IOC 中作为容器的启动项，入口
- @Import
	- 导入其他的配置类，如果要导入 xml 或其他配置文件，可以使用 ((619f64c3-83a2-49a1-a9de-695897991da4))
	- collapsed:: true
	  ```java
	  @Configuration
	  @ComponentScan({"top.maoyilan"})
	  @PropertySource({"classpath:jdbc.properties"})
	  @Import({top.maoyilan.config.ConfigOne.class, 
	           top.maoyilan.config.ConfigTwo.class})
	  public class SpringConfig {
	    ...
	  }
	  ```
- @ImportResource
  id:: 619f64c3-83a2-49a1-a9de-695897991da4
	- 引入一个或多个配置文件
- @ComponentScan
  id:: 11c084cf-03ed-4d67-81a5-d582d4da1245
  collapsed:: true
	- 配置包的扫描
- @PropertySource
  id:: 4a74e5c0-622f-403f-9826-5b16e71f2982
  collapsed:: true
	- 引入 properties 配置文件，并使用 springel 获取其中的键值
- @Lazy
  collapsed:: true
	- 延迟加载 Bean
- @EnableAspectJAutoProxy
  id:: 82a3c07b-6fd3-4b37-a873-b4daab7c7e1d
  collapsed:: true
	- 是 Spring 能扫描那些 @Aspect 注解的类，也就是开启 AOP
	- 参数
	  collapsed:: true
		- proxyTargetClass 默认 false
		  collapsed:: true
			- 是否全部都使用 Cglib 代理来进行代理类的生成，默认如果有接口就使用 jdk 动态代理，如果没有就使用 cglib
- @Aspect
  id:: 8522f4d8-31d8-4164-8431-be8439590c8d
  collapsed:: true
	- id:: 5915ce0d-b511-44af-862a-a768061fd285
	  collapsed:: true
	  > 需要配合 ((e67811d5-ec43-489b-b567-16158f8fc5eb)) 才行
	- 标注一个类为 AOP 配置类
	- collapsed:: true
	  ```java
	  @Aspect
	  @Component
	  public class MyAspect {
	   //定义切入点表达式
	   @Pointcut("execution(* com.example.aspectjannotation.*.*(..))")
	   //定义一个返回值为void、方法体为空的方法来命名切入点
	   private void myPointCut(){}
	    
	   @Before("myPointCut()")
	   public void myBefore(JoinPoint joinpoint){
	    System.out.println("前置通知："+joinpoint);
	    System.out.println("被植入增强处理的目标方法为："+joinpoint.getSignature().getName());
	   }
	    
	   @AfterReturning("myPointCut()")
	   public void myAfterReturning(JoinPoint joinpoint){
	    System.out.println("后置通知："+joinpoint);
	    System.out.println(",被植入增强处理的目标方法为："+joinpoint.getSignature().getName());
	   }
	    
	   @Around("myPointCut()")
	   public Object myAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable{
	    System.out.println("环绕开始。。。");
	    Object obj = proceedingJoinPoint.proceed();
	    System.out.println("环绕结束。。。");
	    return obj;
	   }
	    
	   @AfterThrowing(value="myPointCut()",throwing="e")
	   public void myAfterThrowing(JoinPoint joinpoint,Throwable e){
	    System.out.println("异常通知："+e.getMessage());
	   }
	    
	   @After("myPointCut()")
	   public void myAfter(){
	    System.out.println("最终通知");
	   }
	    
	    
	    // value 配置的是类
	    @DeclareParents(value = "top.maoyilan.model.*", defaultImpl = OtherImpl.class)
	    public Other other;
	  }
	  ```
	- @pointcut 
	  collapsed:: true
		- 声明切入点
	- 五种连接点，可以指定使用哪一个切入点
	  collapsed:: true
		- @Before
		- @After
		- @AfterReturning
		- @AfterThrowing
		- @Around
	- @DeclareParents
	  collapsed:: true
		- 和连接点不同，本注解使用在成员变量（Field）上
		- 用于定义引介通知，相当于 `IntroductionInterceptor`
		- 满足表达式的类将自动实现某些接口
		  collapsed:: true
			- collapsed:: true
			  ```java
			  public class LogService {
			    void addLog(Log log);
			  }
			  
			  public class LogServiceImpl {
			    public void addLog(Log log){
			      // 实现
			    }
			  }
			  
			  @Component
			  @Aspect
			  public class AspectConfig {
			      /**
			       * "+"表示person的所有子类；defaultImpl 表示默认需要添加的新的类
			       */
			      @DeclareParents(value = "top.maoyilan.service.Person+", defaultImpl = FemaleAnimal.class)
			      public Animal animal;
			  }
			  
			  ```
		- 原理就是动态代理
- @SpringJUnitConfig
  id:: b975c2d7-9de9-47cb-b555-5d3f4abfef2b
  collapsed:: true
	- Spring 支持[[单元测试]]的注解，用来支持 ((617fdb69-8a1c-4e05-83e3-7a010c2d2a77))
	- collapsed:: true
	  ```java
	  
	  @SpringJUnitConfig
	  @ContextConfiguration(SpringConfig.class)
	  public class SpringTest {...}
	  
	  // -----> 可以简化为
	  
	  // 指定 spinrg 的纯注解配置类 SpringConfig.class
	  // 当然也可以指定 xml 的配置
	  @SpringJUnitConfig(SpringConfig.class)
	  public class SpringTest {...}
	  ```
- @RunWith
  id:: 138244a0-0488-4ee4-a579-c601756696bc
  collapsed:: true
	- Spring 支持[[单元测试]]的注解，用来支持 ((617fdb6d-05db-4b1c-9cb6-5c045bc7be78))
	- collapsed:: true
	  ```java
	  @RunWith(SpringJUnit4ClassRunner.class)
	  @ContextConfiguration(classes = SpringConfig.class)
	  public class SpringTest {...}
	  ```
- @ContextConfiguration
  collapsed:: true
	- Spring 支持[[单元测试]]的注解，需要配合 ((b975c2d7-9de9-47cb-b555-5d3f4abfef2b)) 或 ((138244a0-0488-4ee4-a579-c601756696bc)) 使用
	- 用来指定 Sping 配置的位置，可以是 xml 或 注解
-
-
- Enable 开头的注解作用
	- 借助 @Import 来收集和注册特定场景的 Bean
- [[SpringMVC 注解]]
- [[Spring Boot 注解]]
-
-