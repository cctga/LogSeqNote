- watch
  > 监听命令的输出，动态变化
	- 参数
		- `-n` `--interval` 刷新周期，默认 2s
		- `-d` `--differences`
		- `-t` `-no-title`
	- 例子
		-
		- ```bash
		  watch 'ls /dev | grep ttyUSB'
		  ```