- watch
  collapsed:: true
  > 周期执行命令，监听命令的输出，动态变化
	- 参数
		- `-n` `--interval` 刷新周期，默认 2s
		- `-d` `--differences` 高亮显示，数值变化的部分
		- `-t` `-no-title` 不显示头
	- 例子
		- 定时查询串口
		  ```bash
		  watch 'ls /dev | grep ttyUSB'
		  ```
		- 定时查询内存
		  ```bash 
		  watch 'free mh'
		  ```
- route
	- 查看路由，显示为 IP
		- ```bash
		  route -n
		  ```
		- 显示为名称
		  ```bash
		  route
		  ```
- traceroute
-
- zip
	- 参数
		- `-r ` 递归处理，将目录下的文件和子文件夹一并处理
		- `-q` 不显示执行过程
	- 例子
- unzip
	- 参数
		- `-l` 显示压缩文件内所包含的文件
		- `-q` 执行时不显示任何信息
		- `-P` 使用 zip 的密码
	- 例子
-