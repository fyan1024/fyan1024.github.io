+++
title = 'Frpc配置'
date = 2024-07-02T14:38:16+08:00
draft = false
+++

在 FRP（Fast Reverse Proxy）中，通常有两个主要组件：客户端（frpc）和服务端（frps）。
角色解释

1. 客户端（frpc）：
    1. 运行在需要通过 FRP 进行内网穿透的机器上。这个机器通常在内网或者防火墙后，无法直接通过外网访问。
    2. frpc 会连接到 frps，并将本地的服务（例如，HTTP 服务、SSH 服务等）通过 frps 暴露到外网。
2. 服务端（frps）：
    1. 运行在有公共 IP 地址的服务器上，通常是外网可以直接访问的机器。
    2. frps 接收来自 frpc 的连接请求，并将外部的流量转发到相应的 frpc 连接。

流程示意

1.  配置 frpc 客户端：
    1. frpc 配置文件（例如 frpc.ini）中指定需要暴露的本地服务及 frps 的地址和端口。
2. 配置 frps 服务端：
    1. frps 配置文件（例如 frps.ini）中指定服务监听的端口和对应的协议。

举例说明
客户端（frpc）配置示例：

假设在内网中的一台机器上运行一个 HTTP 服务（本地端口 80），希望通过 frps 暴露到外网。

frpc.ini 配置文件：

ini

[common]
server_addr = your_frps_server_ip
server_port = 7000

[web]
type = http
local_port = 80
custom_domains = www.yourdomain.com

服务端（frps）配置示例：

运行在有公共 IP 的服务器上，负责接收 frpc 的连接并转发流量。

frps.ini 配置文件：

ini

[common]
bind_port = 7000

[vhost_http_port]
type = tcp
bind_port = 80

设置开机自启动
服务端（frps）的开机自启动配置：

    创建 frps 的 systemd 服务文件：

    bash

sudo nano /etc/systemd/system/frps.service

编辑服务文件：

ini

[Unit]
Description=FRP Server Service
After=network.target

[Service]
Type=simple
User=你的用户名
ExecStart=/home/chentianhao_sannian/Downloads/frp_0.54.0_linux_amd64/frps -c /home/chentianhao_sannian/Downloads/frp_0.54.0_linux_amd64/frps.ini
Restart=on-failure

[Install]
WantedBy=multi-user.target

启用和启动服务：

bash

    sudo systemctl daemon-reload
    sudo systemctl enable frps
    sudo systemctl start frps
    sudo systemctl status frps

客户端（frpc）的开机自启动配置：

    创建 frpc 的 systemd 服务文件：

    bash

sudo nano /etc/systemd/system/frpc.service

编辑服务文件：

ini

[Unit]
Description=FRP Client Service
After=network.target

[Service]
Type=simple
User=你的用户名
ExecStart=/home/chentianhao_sannian/Downloads/frp_0.54.0_linux_amd64/frpc -c /home/chentianhao_sannian/Downloads/frp_0.54.0_linux_amd64/frpc.ini
Restart=on-failure

[Install]
WantedBy=multi-user.target

启用和启动服务：

bash

    sudo systemctl daemon-reload
    sudo systemctl enable frpc
    sudo systemctl start frpc
    sudo systemctl status frpc

通过这些配置，你可以设置 FRP 服务端和客户端在开机时自动启动，从而实现内网穿透和流量转发功能。





