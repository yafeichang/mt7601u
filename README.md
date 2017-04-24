mt7601u
=======

Ralink Wireless Adapter Driver

从小米wifi驱动（https://github.com/eywalink/mt7601u）下载，解压
进入mt7601u-master/etc/Wireless/RT2870AP/文件夹，打开RT2870AP.dat文件，修改wifi名称和密码：SSID为wifi名，WPAPSK为密码，改为自己想要的

编译脚本，没有看到Error即可：
cd mt7601u-master
sudo ./miwifi_build.sh

安装DHCP服务器：
sudo apt-get install dhcp3-server

配置DHCP服务器：
sudo gedit /etc/dhcp/dhcpd.conf
# 在 log-facility local7; 下面添加：
# DNS需自行修改
subnet 192.168.199.0 netmask 255.255.255.0 {
range 192.168.199.10 192.168.199.20;
option routers 192.168.199.1;
option domain-name-servers 114.114.114.114;
}
sudo gedit /etc/default/isc-dhcp-server
# 末尾改为 INTERFACES="ra0"

可能需要重启DHCP 服务：
sudo service isc-dhcp-server restart

更改运行脚本：
sudo gedit miwifi_work.sh
# iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE 这一句中，
# eth0根据现在网络来修改
# 点击右上角网络图标，选择connection information即可知道

插上小米wifi
运行脚本
sudo ./miwifi_work.sh
即可看到wifi。
