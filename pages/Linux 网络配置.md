- netplay
	- Ubuntu 18.04+
	- 配置文件 `/etc/netplay/*.yml`
	- 配置例子
		- 单
		- ```yml
		  
		  ```
	- 生效
		-
		- 测试配置文件
		  ```bash
		  sudo netplay try
		  ```
		- 应用配置文件
		  ```bash
		  sudo netplay apply
		  ```
- network
	- 配置文件 `/etc/network/interfaces`
	- 配置例子
		- ```ini
		  auto enp3s0
		  iface enp10s0 inet static
		  address 192.168.1.162
		  netmask 255.255.255.0
		  gateway 192.168.1.100
		  dns-nameservers 1.0.0.1,1.1.1.1
		  ```
		- `auto` 表示开机启动网卡
	- 使配置生效
		- 重启网络 `sudo systemctl restart networking`
		- 或 `sudo /etc/init.d/networking restart`