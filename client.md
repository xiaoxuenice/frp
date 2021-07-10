#!/bin/bash
export FRP_VERSION=0.29.1
sudo mkdir -p /etc/frp
cd /etc/frp
sudo wget "https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_amd64.tar.gz"
sudo tar xzvf frp_${FRP_VERSION}_linux_amd64.tar.gz
sudo mv frp_${FRP_VERSION}_linux_amd64/* /etc/frp
cat >> /etc/frp/frpc.ini << EOF
[common]
server_addr = \公网ip\
server_port = 7000
[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 7002
EOF


vim /lib/systemd/system/frpc.service
[Unit]
Description=fraps service
After=network.target syslog.target
Wants=network.target
[Service]
Type=simple
#启动服务的命令（此处写你的frps的实际安装目录）
ExecStart=/etc/frp/frp/frpc -c /etc/frp/frp/frpc.ini
[Install]
WantedBy=multi-user.target
