- 简单原理
	- 两个类加载器，AppClassLoader 和 RestartClassLoader
		- AppClassLoader ：加载一些第三方 jar 包和一些不变的资源
		- RestartClassLoader：加载 classpath 下变更的资源，变更时重新加载
	- 监听 classpath 下的资源，当变更时使用 RestartClassLoader 进行重新加载
- 排除监听资源
	- 默认排除
		- 可以在 DevTools 包中的 `spring.factories` 配置文件中找到自动配置的一些类，在 `LocalDevToolsAutoConfiguration` 类中 看到类 `DevToolsProperties` 中有默认的排除资源配置：`DEFAULT_RESTART_EXCLUDES`
	- 配置
		- ```properties
		  spring.devtools.restart.exclude=static/**,public/**
		  ```
-
-