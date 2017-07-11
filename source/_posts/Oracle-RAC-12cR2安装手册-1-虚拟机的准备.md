title: Oracle RAC 12cR2安装手册(1)--虚拟机的准备
categories:
  - Oracle
  - RAC
tags:
  - Tech
  - Installation
author: Jayce
date: 2017-06-23 14:07:00
---
<!-- toc -->
### 创建第一台虚拟机
{% asset_img node01-creation-0.png node01-creation %}
Name: node01; Type: Linux; Version: Oracle(64-bit)→Continue👆🏻
{% asset_img node01-creation-1.png node01-creation %}
Memory size:4096M→Continue👆🏻
{% asset_img node01-creation-2.png node01-creation %}
Create a virtual hard disk now→Create👆🏻
{% asset_img node01-creation-3.png node01-creation %}
VDI→Continue👆🏻
{% asset_img node01-creation-4.png node01-creation %}
Dynamically allocated→Continue👆🏻
{% asset_img node01-creation-5.png node01-creation %}
Name: node01; Size: 40G→Create👆🏻
{% asset_img node01-creation-6.png node01-creation %}
done👆🏻
### 安装操作系统
{% asset_img node01-os-installation-0.png node01-os-installation %}
Setting👆🏻
{% asset_img node01-os-installation-1.png node01-os-installation %}
Storage→Controller:IDE→Optical Drive👆🏻
{% asset_img node01-os-installation-2.png node01-os-installation %}
选择新的或者添加过的镜像文件，此处为Oracle Linux 6.9_x86_64👆🏻
{% asset_img node01-os-installation-3.png node01-os-installation %}
OK👆🏻
{% asset_img node01-os-installation-4.png node01-os-installation %}
启动👆🏻
{% asset_img node01-os-installation-5.png node01-os-installation %}
启动中👆🏻
操作系统的安装步骤省略，可参考[Oracle Linux 6.9_x86_64安装手册](../../23/Oracle-Linux-6-9-x86-64安装手册/)
### 配置第一台虚拟机
安装Oracle RAC至少需要两块网卡，一块为公用网卡，用于给用户和应用服务器读取数据；另一块为私用网卡，用于内部节点间通讯。另外，虚拟机需要访问互联网以安装必要的依赖包。
VitualBox支持多种网络接入模式，默认状态下各种模式中主机、虚拟机以及互联网之间的连通性如下：

{% table table-striped %}

| 网络接入模式 | 主机到虚拟机 | 虚拟机到虚拟机 | 虚拟机到互联网 |
|:------------:|:------------:|:--------------:|:--------------:|
|    Bridge    |      Y       |       Y        |       Y        |
|  Host-only   |      Y       |       N        |       N        |
|   Internal   |      N       |       Y        |       N        |
|     NAT      |      N       |       N        |       Y        |

{% endtable %}

注：
  - Bridge模式下，虚拟机IP受主机是否联网的影响，无法固定，不能很好的模拟真实的网络环境；
  - Host-only模式下，也可以通过配置使得虚拟机到虚拟机、虚拟机到互联网联通，但较复杂，不推荐。

综上所述，虚拟机需要三块网卡以模拟需要的网络环境：一块Host-only网卡使主机可以访问到虚拟机、一块Internal网卡用于节点间通讯和一块NAT网卡来访问互联网。
使用VirtualBox安装虚拟机时，会默认安装一块NAT网卡，因而只需要再添加两块网卡即可。配置方式如下：
#### 配置子网
{% asset_img node01-configure-4.png node01-configure %}
点击VirtualBox菜单栏的偏好设置👆🏻
{% asset_img node01-configure-3.png node01-configure %}
网络→选择NAT网卡→编辑👆🏻
{% asset_img node01-configure-5.png node01-configure %}
Network CIDR:10.10.10.0/24→OK👆🏻
{% asset_img node01-configure-6.png node01-configure %}
接着选择Host-only网卡→编辑👆🏻
{% asset_img node01-configure-7.png node01-configure %}
设置适配器👆🏻
{% asset_img node01-configure-8.png node01-configure %}
设置DHCP服务器→OK👆🏻
{% asset_img node01-configure-9.png node01-configure %}
配置完成→OK👆🏻
注：此时需要重启VirtualBox，使配置生效。
#### 添加网卡
{% asset_img node01-configure-0.png node01-configure %}
选择node01→设置→网络👆🏻
{% asset_img node01-configure-1.png node01-configure %}
选择第二个适配器→启用并选择Host-only适配器且选择vboxnet0👆🏻
{% asset_img node01-configure-2.png node01-configure %}
选择第三个适配器→启用并选择Internal网络且命名为Private IP→OK👆🏻
启动虚拟机，点击未连接的网卡使其联网，待所有网卡都连接成功后，查看网络配置
{% asset_img node01-configure-10.png node01-configure %}
ifconfig👆🏻
观察到，前两块网卡均已分配至指定子网下，Internal网卡由于未设定子网和DHCP服务器，未获取到IP。
#### 固定IP
尝试在主机中使用Host-only网卡的IP（eth1，172.16.10.3）连接到虚拟机👇🏻
{% asset_img node01-configure-11.png node01-configure %}
连接成功👆🏻
参考[Linux下配置静态IP](../../27/Linux下配置静态IP/)来固定eth1和eth2网卡的IP（eth0只用来连接互联网无需固定）
配置完成后，node01网络配置如下👇🏻
{% asset_img node01-configure-13.png node01-configure %}
### 克隆第二台虚拟机
克隆第二台虚拟机时，需关闭第一台虚拟机。
{% asset_img node02-clone-0.png node02-clone %}
选择node01右键→克隆👆🏻
{% asset_img node02-clone-1.png node02-clone %}
虚拟机名设置为node02→克隆👆🏻
{% asset_img node02-clone-2.png node02-clone %}
等待克隆完成👆🏻
{% asset_img node02-clone-3.png node02-clone %}
克隆完成👆🏻
{% asset_img node02-clone-4.png node02-clone %}
启动node02👆🏻
{% asset_img node02-clone-5.png node02-clone %}
启动成功👆🏻
{% asset_img node02-clone-6.png node02-clone %}
查看node02的IP👆🏻
由于node02是由node01克隆而来，主机名和IP完全与之一致，需修改。
### 配置第二台虚拟机
#### 修改主机名
参考[Linux下修改hostname](../../28/Linux下修改hostname/)将node02的hostname修改为node02.myCluster.com
#### 修改IP
仍旧参考[Linux下配置静态IP](../../27/Linux下配置静态IP/)来固定eth1和eth2网卡的IP
配置完成后，node02网络配置如下👇🏻
{% asset_img node02-configure-0.png node02-configuration %}
### 添加共享磁盘
添加共享磁盘时，需关闭node01和node02。
#### 为第一台虚拟机添加磁盘
{% asset_img nodes-add-share-disk-0.png nodes-add-share-disk %}
选择node01→设置👆🏻
{% asset_img nodes-add-share-disk-1.png nodes-add-share-disk %}
选择存储→添加硬盘👆🏻
{% asset_img nodes-add-share-disk-2.png nodes-add-share-disk %}
创建新的硬盘👆🏻
{% asset_img nodes-add-share-disk-3.png nodes-add-share-disk %}
选择VDI→继续👆🏻
{% asset_img nodes-add-share-disk-4.png nodes-add-share-disk %}
选择固定大小→继续👆🏻
{% asset_img nodes-add-share-disk-5.png nodes-add-share-disk %}
目录选择两个虚拟机的父级目录并命名为asm.vdi，大小设置为60G👆🏻
{% asset_img nodes-add-share-disk-6.png nodes-add-share-disk %}
创建中👆🏻
{% asset_img nodes-add-share-disk-7.png nodes-add-share-disk %}
OK👆🏻
{% asset_img nodes-add-share-disk-8.png nodes-add-share-disk %}
添加完成👆🏻
#### 将磁盘设置为共享磁盘
{% asset_img nodes-add-share-disk-9.png nodes-add-share-disk %}
点击VirtualBox菜单栏的文件→虚拟介质管理器👆🏻
{% asset_img nodes-add-share-disk-10.png nodes-add-share-disk %}
选择刚创建的磁盘文件→修改👆🏻
{% asset_img nodes-add-share-disk-11.png nodes-add-share-disk %}
设置磁盘类型为可共享→OK👆🏻
{% asset_img nodes-add-share-disk-12.png nodes-add-share-disk %}
设置完成👆🏻
#### 将共享磁盘添加到第二台虚拟机
{% asset_img nodes-add-share-disk-13.png nodes-add-share-disk %}
选择node02→设置👆🏻
{% asset_img nodes-add-share-disk-14.png nodes-add-share-disk %}
选择存储→添加硬盘👆🏻
{% asset_img nodes-add-share-disk-15.png nodes-add-share-disk %}
选择已存在的磁盘👆🏻
{% asset_img nodes-add-share-disk-16.png nodes-add-share-disk %}
选择上一步中创建的asm.vdi→打开👆🏻
{% asset_img nodes-add-share-disk-17.png nodes-add-share-disk %}
添加完成👆🏻
分别启动两台虚拟机，使用`fdisk -l`查看磁盘是否添加成功👇🏻
{% asset_img nodes-add-share-disk-18.png nodes-add-share-disk %}{% asset_img nodes-add-share-disk-19.png nodes-add-share-disk %}
添加成功👆🏻
由于算法的不同，实际大小略有差异，属于正常范围。
至此，完成了虚拟机的准备工作。
下一篇[Oracle RAC 12cR2安装手册(2)--环境的准备](../../29/Oracle-RAC-12cR2安装手册-2-环境的准备/)
