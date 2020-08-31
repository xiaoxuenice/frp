#!/bin/bash
export FRP_VERSION=0.29.1
sudo mkdir -p /etc/frp
cd /etc/frp
sudo wget "https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_amd64.tar.gz"
sudo tar xzvf frp_${FRP_VERSION}_linux_amd64.tar.gz
sudo mv frp_${FRP_VERSION}_linux_amd64/* /etc/frp
cat >> /etc/init.d/frp << EOF
#!/bin/bash
# chkconfig: 345 85 15
#要执行的命令
nohup /etc/frp/frpc  -c /etc/frp/frpc.ini &
EOF
chmod +x /etc/init.d/frp
chkconfig --add frp
chkconfig frp on
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
nohup /etc/frp/frpc  -c /etc/frp/frpc.ini &
