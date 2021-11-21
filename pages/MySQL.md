- 小问题
	- 编码问题
	  collapsed:: true
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
			-
			  1. 字符编码，确定字符码值
			      其中的 utf8，意思就是使用 unicode 编码
			  2. 存储传输规则的设定（character set）
			      编码确定了但是不知道怎么存储传输，所以需要 utf8（其他还有 utf16，utf32 等）
			      其中 utf8 和 utf8mb4 的区别：
			      > utf8 为早期版本，最长字节为 3 ，实际上也就是 utf8mb3
			      > utf8mb4 兼容了 4 字节的字符，比如 😊，😭，🚗 等
			  3. 排序规则（collation）
			      前两者都确定了之后，数据库还要确定排序规则
			      `utf8_general_ci`  `utf8mb4_0900_ai_ci`
	- 大写表名找不到表
		- 加上配置
			- 注意：mysql 服务配置位置为：
				- `/etc/mysql/mysql.conf.d/mysqld.cnf`
			- ```ini
			  lower_case_table_names=1
			  ```
		- 重启 mysql 服务
			- `service mysql restart`
- 小知识
	- 插入或更新 [🔗](https://baijiahao.baidu.com/s?id=1644358136491778500&wfr=spider&for=pc)
	  collapsed:: true
		- ignore
		- on duplicate key update
		- insert...select...where not exists
		- replace into
	- 备份和还原
	  collapsed:: true
		- 备份
			- `--all-databases` 备份所有库
			- ```bash
			  mysqldump -uroot -p --all-databases > dump.db
			  ```
			- 备份一个库
			- ```bash
			  mysqldump -uroot -p dbname > dump.db
			  ```
			- `--ignore-table=dbname.table` 来排除一些表，可以多个
			- ```bash
			  mysqldump -uroot -p dbname --ignore-table=dbname.table --ignore-table=dbname.table2 > dump.db
			  ```
			- 备份一个库中指定的表
			- ```bash
			  
			  mysqldump -uroot -p dbname table1 table2 > dump.db
			  ```
		- 还原
			- 如果没有指定的库需要手动创建
			- mysql 命令来还原
			- ```bash
			  mysqladmin -uroot -p create db_name 
			  mysql -uroot -p  db_name < /backup/mysqldump/db_name.db
			  ```
			- mysql 中的 source 来还原，即先登录 mysql，然后用 source 命令
			- ```bash
			  use dbname;
			  source /root/dump.db
			  ```
	- 查看索引
	  collapsed:: true
		- `show index from <table> from [<database>]`
	- `escape` 关键词
	  collapsed:: true
		- 指定字符替代 `like` 查询中的转译字符 `\`，可以指定多个
			- `%` 匹配若干个字符
			- `_` 匹配单个字符
		- 使用 `a` 来作为转译字符
		  
		  ```sql
		  SELECT * FROM user WHERE name LIKE "a%_" ESCAPE "a"
		  
		  # 和以下查询相同
		  
		  SELECT * FROM user WHERE name LIKE "\%_"
		  ```
	- 时间戳和时间互转
		- ```sql
		  -- 日期字符串转时间戳，10位，数据库存储的是13位
		  select UNIX_TIMESTAMP('2019-09-01 00:00:00');
		  -- 时间戳转日期
		  select FROM_UNIXTIME(1567267200, '%Y-%m-%d %H:%i:%s')
		  ```
		-
- [[MySQL 8.0]]
-
-