title: Linux下配置静态IP
categories:
  - Linux
  - Network
tags:
  - Tech
  - Configuration
author: Jayce
date: 2017-06-27 18:06:00
---
使用ifconfig命令查看当前使用的网卡👇🏻
{% asset_img linux-config-static-ip-0.png linux-static-ip-configuration %}
当前使用的网卡为eth0👆🏻
使用如下命令查看当前网卡的MAC地址👇🏻
`cat /etc/udev/rules.d/70-persistent-net.rules`
{% asset_img linux-config-static-ip-1.png linux-static-ip-configuration %}
eth0网卡的MAC地址为08:00:27:44:9e:74👆🏻
使用如下命令查看当前网卡的UUID👇🏻
`nmcli co`
{% asset_img linux-config-static-ip-2.png linux-static-ip-configuration %}
eth0网卡的UUID为83121414-3da9-49fb-ba1b-ed5441278cba
创建或者编辑eth0网卡的脚本文件
```
cd /etc/sysconfig/network-scripts/
vi ifcfg-eth0
```
参考配置如下：
```
DEVICE=eth0
HWADDR=08:00:27:44:9e:74
TYPE=Ethernet
UUID=83121414-3da9-49fb-ba1b-ed5441278cba
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
IPADDR=172.16.10.11
GATEWAY=172.16.10.1
NETMASK=255.255.255.0
DNS1=8.8.8.8
DNS2=114.114.114.114
```
注：
  - 需将HWADDR和UUID修改为对应值。

使用如下命令重启网络
`service network restart`
由于IP的改变此时连接会断开，使用新的IP重新连接即可。
{% asset_img linux-config-static-ip-3.png linux-static-ip-configuration %}
连接成功，配置完成👆🏻
