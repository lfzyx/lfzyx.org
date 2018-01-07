Pptd
====
apt-get install pptpd
编辑配置文件
vi /etc/pptpd.conf
localip 10.10.10.70 VPN 服务器和VPN客户端通讯时使用的ip。
remoteip 10.10.10.71-80 VPN客户端自动分配时使用的IP地址段
设置用户名和密码 vi /etc/ppp/chap-secrets
user pptpd password *
设置IP地址转发 vi /etc/sysctl.conf
net.ipv4.ip_forward=1
sysctl -p /etc/sysctl.conf