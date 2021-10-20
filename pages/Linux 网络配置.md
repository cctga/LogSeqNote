- netplay
	- Ubuntu 18.04+
	-
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
		-