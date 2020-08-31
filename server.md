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
nohup /etc/frp/frps  -c /etc/frp/frps.ini &
EOF
chmod +x /etc/init.d/frp
chkconfig --add frp
chkconfig frp on
#7000必开,7000以后为私网提供
firewall-cmd --add-port=7000-7100/tcp --permanent  && firewall-cmd --reload
nohup /etc/frp/frps  -c /etc/frp/frps.ini &
