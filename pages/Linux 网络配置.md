- netplay *Ubuntu 18.04+*
	- 配置文件 `/etc/netplay/*.yml`
	- 配置例子
	  collapsed:: true
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
		  collapsed:: true
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
	  collapsed:: true
		-
		- 测试配置文件
		  collapsed:: true
		  ```bash
		  sudo netplay try
		  ```
		- 应用配置文件
		  collapsed:: true
		  ```bash
		  sudo netplay apply
		  ```
- network
	- 配置文件 `/etc/network/interfaces`
	- 配置例子
		- collapsed:: true
		  ```ini
		  auto enp3s0
		  iface enp10s0 inet static
		  address 192.168.1.162
		  netmask 255.255.255.0
		  gateway 192.168.1.1
		  dns-nameservers 8.8.8.8,114.114.114.114
		  ```
		- `auto` 表示开机启动网卡
		- `dns-nameservers` 配置需要依靠 resolve.conf 来生效，需要有 `resolvconf` 这个软件，如果没有，需要安装
			- 两个配置文件 `/etc/resolve.conf` 和 `/etc/resolvconf/resolv.conf.d/base`
				- 主要就是配置 nameserver，其他的配置不说了
				  ```ini
				  nameserver 114.114.114.114
				  nameserver 8.8.8.8
				  ```
			- 其中 前者 表示当前使用的 DNS 地址(配置 dns-nameservers 后，配置复制到这个文件中生效)，重启后会失效，所以永久生效要在 后者 中配置
			- 配置完后重启 `service resolvconf restart`
	- 使配置生效
		- 重启网络 `sudo systemctl restart networking`
		- 或 `sudo /etc/init.d/networking restart`
	- 重启网络的时候报错
		- collapsed:: true
		  ```powershell
		  root@kljc-28:~# /etc/init.d/networking restart
		  [....] Restarting networking (via systemctl): networking.serviceJob for networking.service failed because the control process exited with error code. See "systemctl status networking.service" and "journalctl -xe" for details.
		   failed!
		   
		  
		  root@kljc-28:~# systemctl status networking.service
		  ● networking.service - Raise network interfaces
		     Loaded: loaded (/lib/systemd/system/networking.service; enabled; vendor preset: enabled)
		    Drop-In: /run/systemd/generator/networking.service.d
		             └─50-insserv.conf-$network.conf
		     Active: failed (Result: exit-code) since 二 2021-10-26 16:59:11 CST; 15s ago
		       Docs: man:interfaces(5)
		    Process: 3534 ExecStart=/sbin/ifup -a --read-environment (code=exited, status=1/FAILURE)
		    Process: 3529 ExecStartPre=/bin/sh -c [ "$CONFIGURE_INTERFACES" != "no" ] && [ -n "$(ifquery --read-environment --
		   Main PID: 3534 (code=exited, status=1/FAILURE)
		  
		  10月 26 16:59:10 kljc-28 systemd[1]: Starting Raise network interfaces...
		  10月 26 16:59:10 kljc-28 ifup[3534]: RTNETLINK answers: File exists
		  10月 26 16:59:10 kljc-28 ifup[3534]: Failed to bring up enp3s0.
		  10月 26 16:59:11 kljc-28 systemd[1]: networking.service: Main process exited, code=exited, status=1/FAILURE
		  10月 26 16:59:11 kljc-28 systemd[1]: Failed to start Raise network interfaces.
		  10月 26 16:59:11 kljc-28 systemd[1]: networking.service: Unit entered failed state.
		  10月 26 16:59:11 kljc-28 systemd[1]: networking.service: Failed with result 'exit-code'.
		  ```
		- `networking.service` 服务起不来
			- 配置 network-manager ,配置文件在 `/etc/NetworkManager/NetworkManager.conf`
			- 将配置中的 managed 项，配置成 `managed=true`
			- 重启 network-manager ：`sudo service network-manager restart`
			- ...不灵的话重启一下吧
		-