- [[快捷键]]
- 小技巧
	- postfix completion (后缀自动完成 )
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
		- .return
- 问题
	- [[MyBatis]] mapper.xml 无法识别字段
		- 搜索 `sql Resolution Scopes` 选择数据库