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
- traceroute