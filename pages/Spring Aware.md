- Aware 是什么
	- 感知类，用来使 Bean 类感知到 Spring 容器，使用其中的资源
- Aware 有什么用
	- 使用 Spring Aware 的目的是为了让 Bean 获得 Spring 容器的服务
- Aware 怎么用
	- 让Bean 类实现 Aware 接口，使用接口方法中的参数就可以了，这些重写的方法会在 Bean 创建后自动调用
	- ```java
	  public class BeanClass implements BeanNameAware {
	    @Override
	    public void setBeanName(String name){
	      System.out.println("本 Bean 的ID="+name);
	    }
	  }
	  ```
- Aware 有哪些
	- BeanNameAware
		- 获取当前 Bean 在容器中所在的名称
	- ApplicationContextAware
		- 获取容器的资源
	- MessageSourceAware
		- 获取 Message Source 相关的文本信息
	- ApplicationEventPublisherAware
		- 发布事件
	- > 以下四个 Aware 被 AutoConfigurationImportSelector Spring Boot 自动配置类实现
	- BeanFactoryAware
		- 获取当前 Bean 所在的 BeanFactory
	- ResourceLoaderAware
		- 获取资源加载器，以获得外部资源文件
	- BeanClassLoaderAware
	- EnviromentAware
		- 环境
- 使用多个 Aware  执行顺序是怎么样的
	- BeanNameAware -> BeanFactoryAware -> applicationContextAware