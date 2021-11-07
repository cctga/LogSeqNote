- 与为每次刷新创建新的内部 BeanFactory 实例的其他 ApplicationContext 实现相反，此上下文的内部 ^^BeanFactory 从一开始就可用^^，以便能够在其上注册 bean 定义。refresh()只能被调用一次
-
- 用法示例：
	- ```java
	  GenericApplicationContext ctx = new GenericApplicationContext();
	  XmlBeanDefinitionReader xmlReader = new XmlBeanDefinitionReader(ctx);
	  xmlReader.loadBeanDefinitions(new ClassPathResource("applicationContext.xml"));
	  PropertiesBeanDefinitionReader propReader = new PropertiesBeanDefinitionReader(ctx);
	  propReader.loadBeanDefinitions(new ClassPathResource("otherBeans.properties"));
	  ctx.refresh();
	  
	  MyBean myBean = (MyBean) ctx.getBean("myBean");
	  ```
- 对于 XML bean 定义的典型情况，只需使用 ClassPathXmlApplicationContext或 FileSystemXmlApplicationContext ，它们更容易设置 - 但不太灵活，因为您可以只使用 XML bean 定义的标准资源位置，而不是混合任意 bean 定义格式。 Web 环境中的等效项是org.springframework.web.context.support.XmlWebApplicationContext 。
  对于应该以可刷新方式读取特殊 bean 定义格式的自定义应用程序上下文实现，请考虑从AbstractRefreshableApplicationContext基类派生。