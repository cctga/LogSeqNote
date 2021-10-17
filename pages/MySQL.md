- 小问题
	- 插入或更新 [🔗](https://baijiahao.baidu.com/s?id=1644358136491778500&wfr=spider&for=pc)
		- ignore
		- on duplicate key update
		- insert...select...where not exists
		- replace into
	- 编码问题
		- 从一个报错开始：
		  ```sql
		  SELECT count(0) FROM ms_city WHERE city_name LIKE concat('%', '😊', '%')
		  
		  # 报错
		  # 1267 - Illegal mix of collations (utf8_general_ci,IMPLICIT) 
		  # 	and (utf8mb4_0900_ai_ci,COERCIBLE) for operation 'like', Time: 0.006000s
		  ```
		  使用 😊 作为筛选条件去查询记录的时候，数据库报错，看起来是编码的问题
		  两种编码不兼容：
		  `utf8_general_ci`  `utf8mb4_0900_ai_ci`
		- 解答
		  先要明白 mysql 编码是如何使用的，包括三步：
			- 1.