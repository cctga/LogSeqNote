alias:: aspectweaver

- 一个面向切面编程的 API 包
- 被 [[Spring AOP]] 依赖
- 引入
	- ```xml
	  <dependency>
	    <groupId>org.aspectj</groupId>
	    <artifactId>aspectjweaver</artifactId>
	    <version>1.9.8.RC1</version>
	  </dependency>
	  ```
- AspectJ 语法
  id:: 617fe5ba-66e6-47ae-805a-dd0cef3abf27
	- 指定一个具体的方法
	  ```text
	  
	  execution(public void top.maoyilan.service.impl.LogServiceImpl.insertLog(String,int))
	  ```
	- 模糊匹配，匹配 service 包中的所有类所有方法
	  ```text
	  execution(* top.maoyilan.service.*..*(..))
	  ```
	- `..` 表示有或没有
	- `*` 表示有，但是不知道是什么
	- 匹配所有方法
	  ```text
	  execution(* *..*(..))
	  ```
-