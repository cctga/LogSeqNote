- [[快捷键]]
- 小技巧
	- postfix completion (后缀自动完成 )
	  collapsed:: true
		- `.sout`
		- `.soutv`
		  ```java
		  // before
		  item.soutv
		  // after
		  System.out.println("item = " + item);
		  ```
		- `.for` `.fori`
		- `.var`
		- `.null` `.nn`
		  ```java
		  // before
		  obj.null
		  // after
		  if(obj == null){
		    ...
		  }
		  
		  //.nn (not null)
		  if(obj != null){
		    ...
		  }
		  ```
		- .case
		  ```java
		  123.cast
		  ((float) 123)
		  ```
		- .return
		  ```java
		  "hello".return
		  // after
		  return "hello";
		  ```
	- Live Templates(动态模版)
		- 参考
- 问题
	- [[MyBatis]] mapper.xml 无法识别字段
		- 搜索 `sql Resolution Scopes` 选择数据库