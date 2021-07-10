#!/bin/bash
export FRP_VERSION=0.29.1
sudo mkdir -p /etc/frp
cd /etc/frp
sudo wget "https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_amd64.tar.gz"
sudo tar xzvf frp_${FRP_VERSION}_linux_amd64.tar.gz
sudo mv frp_${FRP_VERSION}_linux_amd64/* /etc/frp
#7000必开,7000以后为私网提供
firewall-cmd --add-port=7000-7100/tcp --permanent  && firewall-cmd --reload
vim /lib/systemd/system/frps.service
[Unit]
Description=fraps service
After=network.target syslog.target
Wants=network.target
[Service]
Type=simple
#启动服务的命令（此处写你的frps的实际安装目录）
ExecStart=/etc/frp/frp/frps -c /etc/frp/frp/frps.ini
[Install]
WantedBy=multi-user.target
