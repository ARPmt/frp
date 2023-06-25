# FRP 测试

## 1. 安装部署
- 1.1 安装 go
      

- 1.2 安装FRP
```
  在FRP git 下载 FRP 各平台已编译好的 可执行文件即可
  下载连接： https://github.com/fatedier/frp/releases
  linux amd 平台
  wget https://github.com/fatedier/frp/releases/download/v0.49.0/frp_0.49.0_linux_amd64.tar.gz
  tar -xzvf frp_0.49.0_linux_amd64.tar.gz
  frps 为服务端可执行程序，frps.ini 为服务端配置文件
  启动服务端
  ./frps -c frps.ini
  frpc 为客户端可执行程序，frpc.ini 为服务端配置文件
  启动客户端
  ./frpc -c frpc.ini

  windwos amdx86 平台
  https://github.com/fatedier/frp/releases/download/v0.49.0/frp_0.49.0_windows_amd64.zip
  frps.exe 为服务端可执行程序，frps.ini 为服务端配置文件
  在dos 界面下启动服务端
  ./frps -c frps.ini
  frpc.exe 为客户端可执行程序，frpc.ini 为服务端配置文件
  在dos 界面下启动客户端
  ./frpc -c frpc.ini
  
```

## 2. FRP配置
- 2.1 服务端配置
服务端常用配置：
```
frps.ini
  [common]
  bind_port = 7000      # 用户客户端连接的tcp 端口，主要用于控制面的连接
  vhost_http_port = 80    # 配置监听的HTTP端口
  vhost_https_port = 443   # 配置监听的HTTPS端口
  subdomain_host = metelcloud.com   # 如果有，可以配置服务器所用的主域名，客户端可配置每个http代理的子域名

  # frp监控页面的 端口号及用户名、密码
  dashboard_port = 7500
  dashboard_user = admin
  dashboard_pwd = admin

  # 代理 访问的端口白名单
  allow_ports = 2000-3000,3001,3003,4000-50000

```

- 2.2 客户端配置
  - 2.2.1 客户端常用配置，建议采用配置分离的方式，单独配置每个代理服务
```
frpc.ini
  [common]
	# 连接frps 服务器的基础配置
  server_addr = www.metelcloud.com    # 要连接服务端的IP地址或域名
  server_port = 7000          # 要连接服务端的tcp 端口
  includes = /etc/confd/*.ini     # 配置分离，加载该目录下.ini 配置文件

	# 客户端热重启的配置
  admin_addr = 127.0.0.1
  admin_port = 7400

	# 要代理的服务配置
  [ssh]        # 配置ssh 代理
  type = tcp      # 协议tcp协议
  local_ip = 127.0.0.1     # 本地IP地址
  local_port = 22         # 要代理的ssh 的端口
  remote_port = 6000     # 服务端代理访问的TCP 端口号，服务端一个端口号只能代理一个TCP/UDP服务，服务端防火墙要放开该端口号

	热加载配置：
  frpc reload -c ./frpc.ini
  查看代理状态
  frpc status -c ./frpc.ini

```

  - 2.2.2 配置tcp 代理
```
  配置 mstc 远程桌面代理，添加 mstc.ini 配置文件，服务器代理端口 13389,
mstc.ini
  [ssh]        # 配置ssh 代理
  type = tcp      # 协议tcp协议
  local_ip = 127.0.0.1     # 本地IP地址
  local_port = 3389         # 要代理的ssh 的端口
  remote_port = 13389     # 服务端代理访问的TCP 端口号，服务端一个端口号只能代理一个TCP/UDP服务，服务端防火墙要放开该端口号

  热加载配置：
  frpc reload -c ./frpc.ini
```

  - 2.2.3 配置udp 代理
```
  配置 dns查询代理，添加 dns.ini 配置文件，服务器代理端口 6000,
udp.ini
  [udp]        # 配置udp 代理
  type = udp     # 协议tcp协议
  local_ip = 127.0.0.1     # 本地IP地址
  local_port = 53        # 要代理的ssh 的端口
  remote_port = 6000     # 服务端代理访问的TCP 端口号，服务端一个端口号只能代理一个TCP/UDP服务，服务端防火墙要放开该端口号

  热加载配置：
  frpc reload -c ./frpc.ini
```

 - 2.2.4 配置http 代理
```
  配置 http代理 , custom_domains 和 subdomain 必须要配置其中一个，两者可以同时生效。，
test_web.ini
  [test_web]     # 配置http代理的名称
  type = http     # 代理协议
  local_ip = 127.0.0.1
  local_port = 8000   # 本地http 端口号
  subdomain = arp     # 如果有，http 代理的子域名，需服务端配置 subdomain_host，与custom_domains 配置二选一
  custom_domains = www.yourdomain.com  # 如果有，在自由域名的情况下，http 代理访问的域名，需添加域名的DNS解析记录到代理服务器，与subdomain配置 二选一，
	http_user = abc    # 访问配置 http 代理服务的用户名
	http_pwd = abc      # 访问配置 http 代理服务的密码


  热加载配置：
  frpc reload -c ./frpc.ini

```

 - 2.2.5 配置http url 路由到不同的代理
```
  配置两个web 代理服务 web01/web02,  url后缀为  /news,/about 代理到web02的 web.yourdomain.com:81， 
web.ini
	[web01]
	type = http    # 代理协议 
	local_port = 80    # 本地http 端口号
	custom_domains = web.yourdomain.com   # http代理的域名，在没有域名的情况下，可以配置 代理服务器域名的子域名
	locations = /     # url 路由后缀
	
	[web02]
	type = http   # 代理协议 
	local_port = 81    # 本地http 端口号
	custom_domains = web.yourdomain.com   # http代理的域名，在没有域名的情况下，可以配置 代理服务器域名的子域名
	locations = /news,/about     # url 路由后缀

  热加载配置：
  frpc reload -c ./frpc.ini

```

 - 2.2.6 配置本地http 服务启用https 代理
```
  配置两个web 代理服务 web01/web02,  url后缀为  /news,/about 代理到web02的 web.yourdomain.com:81， 
web.ini
	[web01]
	type = http    # 代理协议 
	custom_domains = web.yourdomain.com   # http代理的域名，在没有域名的情况下，可以配置 代理服务器域名的子域名
	plugin = https2http   # https2http 插件
	plugin_local_addr = 127.0.0.1:80  # 本地http 服务
	
	# HTTPS 证书相关的配置
	plugin_crt_path = ./server.crt
	plugin_key_path = ./server.key
	plugin_host_header_rewrite = 127.0.0.1
	plugin_header_X-From-Where = frp

  热加载配置：
  frpc reload -c ./frpc.ini

```
 
  - 2.2.7 配置文件访问 代理
```
  配置 dns查询代理，添加 dns.ini 配置文件，服务器代理端口 6000,
udp.ini
  [udp]        # 配置udp 代理
  type = udp     # 协议tcp协议
  local_ip = 127.0.0.1     # 本地IP地址
  local_port = 53        # 要代理的ssh 的端口
  remote_port = 6000     # 服务端代理访问的TCP 端口号，服务端一个端口号只能代理一个TCP/UDP服务，服务端防火墙要放开该端口号

	[test_static_file]  # 配置文件代理名称
	type = tcp     # 协议tcp协议
	remote_port = 6001    # 服务端代理访问的TCP 端口号
	plugin = static_file    # 文件代理启用的插件，
	plugin_local_path = /tmp/file   # 要对外暴露的文件目录
	plugin_strip_prefix = static  # 访问url 的后缀名称，如 http://x.x.x.x:6001/static/
	plugin_http_user = abc     # 访问文件代理服务的用户名
	plugin_http_passwd = abc    # 访问文件代理服务的用户名
	bandwidth_limit = 1MB     # 客户端代理限速 1m

  热加载配置：
  frpc reload -c ./frpc.ini

	访问代理文件服务，代理服务器有域名可以访问域名，没有域名访问公网IP：
	http://www.metelcloud.com:6001/static/
	http://43.231.196.25:6001/static/
```

