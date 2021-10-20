- watch
  > 监听命令的输出，动态变化
	- 参数
		- `-n` `--interval` 刷新周期，默认 2s
		- `-d` `--differences` 数值变化的
		- `-t` `-no-title`
	- 例子
		- 定时查询串口
		  ```bash
		  watch 'ls /dev | grep ttyUSB'
		  ```
		- 定时查询内存
		  ```bash 
		  watch 'free mh'
		  ```