- netplay
	- Ubuntu 18.04+
	- 配置文件 `/etc/netplay/*.yml`
	- 配置例子
		- 单地址单网关
		- ```yml
		  network:
		    version: 2
		    renderer: networkd # 可以配置成 NetworkManager 将网络配置托管给网络管理器
		    ethernets: # 表示有线网卡, 无线使用 wifis:
		      enp3s0:
		        addresses:
		          - 10.100.56.6/24
		        gateway4: 10.100.56.1  # 网关
		        nameservers:
		          addresses: [114.114.114.114, 8.8.8.8]
		  ```
		-
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