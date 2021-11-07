-
  1. 实例化 Bean，构造方法调用
-
  2. 属性注入，setter 调用，如果配置了的话
-
  3. 调用多个 [[Spring Aware]]
-
  4. 调用 BeanPostProcessor 中的 `postProcessBeforeInitialization` 方法
-
  5. 调用 PostConstruct 注解的方法
-
  6. 调用 InitializingBean 的 afterPropertiesSet 方法
	- 类似于 [[Spring Aware]]
-
  7. 调用 init-method 配置的方法
-
  8. 调用 BeanPostProcessor 中的 `postProcessAfterInitialization` 方法
-
  9. 判断是单例还是多例
	- 多例，那么 Bean 的生命周期就从此脱离 IOC
	- 单例，放到 Spring 单例池中，等待调用
-
  10. 销毁逻辑，只有单例 Bean 才有
	- 指定 PreDestroy 注解的方法
	- 执行 DisposableBean 的 destroy 方法
	- 执行 destroy-method 配置的方法