title:: MySQL 8.0

	-
- 安装并登录
	- apt 安装
		- ```bash
		  ## 如果失败可以进行 apt update
		  
		  sudo apt install mysql-server-8.0
		  ```
	- 在不启动服务的时候进行免密码操作
	  **使用 root 用户**
		- ```bash
		  mysqld --initialize-insecure
		  ```
	- 启动 mysql 服务
	  **使用 root 用户**
		- ```bash
		  service mysql start
		  ```
	- 空密码登录
- 新特性
	- 隐藏索引
		- 当一个索引被隐藏时，查询时它将不会被使用(**但是依旧会实时更新**)，方便调试，可以在不删除索引的情况下测试
		- 查看索引
			- ```sql
			  show index from [table_name]
			  ```
		- 隐藏
			- ```sql
			  alter table t alter index i invisible
			  ```
		- 显示
			- ```sql
			  alter table t alter index i visible
			  ```
	- 设置持久化
		- 可以将设置持久化，写入 `mysqld-auto.cnf` 文件中，重启后也有效
		- 命令 `set presist`
		- ```sql
		  set presist max_connections = 500
		  ```
	- 默认编码将使用 `utf8mb4`
	- 通用表达式 **CTE**
		- 对子查询 SQL 格式的优化 **WITH**
		- ```sql
		  SELECT t1.*, t2.* FROM 
		    (SELECT col1 FROM table1) t1,
		    (SELECT col2 FROM table2) t2;
		  ```
		- 优化为
		- ```sql
		  WITH
		    t1 AS (SELECT col1 FROM table1),
		    t2 AS (SELECT col2 FROM table2)
		  SELECT t1.*, t2.* 
		  FROM t1, t2;
		  
		  ```
	- [窗口函数]([[Mysql 8.0 窗口函数]])
		- 优雅的实现排名，占比统计等功能
		- 样例表
		  ```powerShell
		  mysql> select * from classes;
		  +--------+-----------+
		  | name   | stu_count |
		  +--------+-----------+
		  | class1 |        41 |
		  | class2 |        43 |
		  | class3 |        57 |
		  | class4 |        57 |
		  | class5 |        37 |
		  +--------+-----------+
		  ```
		- 排名
		- ```powershell
		  
		  
		  mysql> select *, rank() over w as `rank` from classes
		      -> window w as (order by stu_count);
		  +--------+-----------+------+
		  | name   | stu_count | rank |
		  +--------+-----------+------+
		  | class5 |        37 |    1 |
		  | class1 |        41 |    2 |
		  | class2 |        43 |    3 |
		  | class3 |        57 |    4 |
		  | class4 |        57 |    4 |
		  +--------+-----------+------+
		  ```
		- 占比
		  ```powershell
		  mysql> select *,
		      -> (stu_count)/(sum(stu_count) over()) as rate
		      -> from classes;
		  +--------+-----------+--------+
		  | name   | stu_count | rate   |
		  +--------+-----------+--------+
		  | class1 |        41 | 0.1745 |
		  | class2 |        43 | 0.1830 |
		  | class3 |        57 | 0.2426 |
		  | class4 |        57 | 0.2426 |
		  | class5 |        37 | 0.1574 |
		  +--------+-----------+--------+
		  ```
		-
-