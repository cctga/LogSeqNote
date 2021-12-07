- @ConfigurationProperties
  id:: 619bb344-da22-4492-8e06-a78e832860ed
  collapsed:: true
	- 用于 `类` 和 `方法` 上，注入，支持 **松散绑定** 注入
		- **松散绑定** 指要绑定的属性不需要和类属性名称完全一样
			- ```java
			  public class User {
			    private String firstName;
			  }
			  ```
			- 对于上面的 firstName 属性，以下的配置都可以注入
				- first-name
				- first_name
				- FIRST_NAME
	- **批量注入** 的功能，将 application 配置文件中的配置注入到当前类的属性 或 @Bean 返回的对象的属性中
	- 用在类上
		- 就可以通过配置 `spring.devtools.restart.exclude` 来将配置的值注入到 exclude 属性中
		  id:: 619bb438-74ba-49c6-8d1f-7b300e7b6043
		- ```java
		  @ConfigurationProperties(prefix = "spring.devtools")
		  public class DevToolsProperties {
		    
		    private Restart restart = new Restart();
		    
		    //...
		    
		    public static class Restart {
		  
		      private static final String DEFAULT_RESTART_EXCLUDES = "META-INF/maven/**,"
		        + "META-INF/resources/**,resources/**,static/**,public/**,templates/**,"
		        + "**/*Test.class,**/*Tests.class,git.properties,META-INF/build-info.properties";
		  
		      /**
		  		 * Whether to enable automatic restart.
		  		 */
		      private boolean enabled = true;
		  
		      /**
		  		 * Patterns that should be excluded from triggering a full restart.
		  		 */
		      private String exclude = DEFAULT_RESTART_EXCLUDES;
		  }
		  ```
	- 用在方法上，配合 @Bean，注入 Bean 的属性值
		- ```java
		  // 会把属性注入到 AnotherComponent 对象中
		  @ConfigurationProperties(prefix = "another")
		  @Bean
		  public AnotherComponent anotherComponent(){
		    return new AnotherComponent();
		  }
		  ```
	- 和 ((47abb64b-9663-4043-bbe0-2ce9881a32e9)) 的比较
		- |特性|@ConfigurationProperties|@Value|
		  |--|--|--|
		  |松散绑定|yes|部分|
		  |元数据注入|yes|no|
		  |SpringEL|no|yes|
		  |使用场景|批量注入|单个值注入|
	- 相关
		- ((47abb64b-9663-4043-bbe0-2ce9881a32e9))
		- ((16e999fd-8479-457f-86aa-d2b262180114))
- @EnableConfigurationProperties
  collapsed:: true
	- 批量指定 ((619bb344-da22-4492-8e06-a78e832860ed)) 配置的类，方便使用，配置过的类，只要使用 ((619bb344-da22-4492-8e06-a78e832860ed)) 注解就可以生效，而无需使用 ((e67811d5-ec43-489b-b567-16158f8fc5eb)) 或 ((a097a055-ec1d-40e8-b0de-5ccc5c5ff585))
	- ```java
	  @EnableConfigurationProperties({Person.class, Pet.class})
	  public class configurationClass {...}
	  ```
	- ⚠️ 指定的类会被扫描注入属性，但是不会让 `@Bean` 注解生效，也就是 `@Bean` 不会生成 Bean 加入 IOC
- @SpringBootConfiguration
  id:: 619f6329-0af6-4024-8ab2-4c7e78a4db3e
	- ((a097a055-ec1d-40e8-b0de-5ccc5c5ff585)) 注解的包装，其实就是 @Configuration 的功能
- @AutoConfigurationPackage
  id:: 619f64c3-15fb-44ed-8934-b62be65384d1
	- 自动配置包
	- 为容器导入一个组件：`AutoConfigurationPackage.Registar`，注册 `BasePackage` 类
- @EnableAutoConfiguration
  id:: 619f6393-607b-4357-877c-6e66bfd9f4fd
	- 启用自动配置功能，完成 SpringBoot 自动配置的功能，核心注解
	- 引用注解： ((619f64c3-15fb-44ed-8934-b62be65384d1))
	- 导入组件：`AutoConfigurationImportSelector`
- @SpringBootApplication
  id:: 619f64c3-c09e-4531-b23d-30ddc19c25bd
  collapsed:: true
	- 项目启动入口
	- 一个复合注解
		- ((619f6329-0af6-4024-8ab2-4c7e78a4db3e))
		- ((619f6393-607b-4357-877c-6e66bfd9f4fd))
		- ((11c084cf-03ed-4d67-81a5-d582d4da1245))
- @SpringBootTest
	- Springboot 的单元测试注解
-