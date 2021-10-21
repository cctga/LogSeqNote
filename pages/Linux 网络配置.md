- netplay
  collapsed:: true
	- Ubuntu 18.04+
	- 配置文件 `/etc/netplay/*.yml`
	- 配置例子
		- [[DHCP]]
		  collapsed:: true
			- ```yml
			  network:
			    version: 2
			    renderer: networkd # 可以配置成 NetworkManager 将网络配置托管给网络管理器
			    ethernets: # 表示有线网卡, 无线使用 wifis:
			      enp3s0:
			        dhcp4: true
			  ```
		- 静态 IP
			- 单地址单网关
			  collapsed:: true
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
			- 多地址单网关
			  collapsed:: true
				- ```yml
				  network:
				    version: 2
				    renderer: networkd # 可以配置成 NetworkManager 将网络配置托管给网络管理器
				    ethernets: # 表示有线网卡, 无线使用 wifis:
				      enp3s0:
				         addresses:
				             - 10.100.1.38/24
				             - 10.100.1.39/24
				         gateway4: 10.100.1.1
				  ```
			- 多网关
			  collapsed:: true
				- ```yml
				  network:
				    version: 2
				    renderer: networkd # 可以配置成 NetworkManager 将网络配置托管给网络管理器
				    ethernets: # 表示有线网卡, 无线使用 wifis:
				    	enp3s0:
				        addresses:
				          - 9.0.0.9/24
				          - 10.0.0.10/24
				          - 11.0.0.11/24
				        #gateway4:  # unset, since we configure routes below
				        routes:
				          - to: 0.0.0.0/0
				            via: 9.0.0.1
				            metric: 100 # 默认为 100, 可以省略
				          - to: 0.0.0.0/0
				            via: 10.0.0.1
				            metric: 100
				          - to: 0.0.0.0/0
				            via: 11.0.0.1
				            metric: 100
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