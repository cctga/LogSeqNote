title:: Mysql 8.0 窗口函数

- [MySQL 8.0窗口函数](https://www.cnblogs.com/DataArt/p/9961676.html)
- 用法
	- `函数名 ([expr]) over 子句`
	- 函数可以是聚合函数，也可以是窗口函数
	- 子句可以省略，表示窗口包含所有行
		- `select *, sum(stu_count) over() as total_count from classes`
	- 完整写法
		- `select *, rank() over w as rank from classes`
		   `window w as (order by stu_count)`
- 序号函数：
	- row_number()
	- rank()
	- dense_rank()
- 分布函数：
	- percent_rank()
	- cume_dist()
- 前后函数：
	- lag()
	- lead()
- 头尾函数：
	- first_val()
	- last_val()
- 其他函数：
	- nth_value()
	- nfile()
-