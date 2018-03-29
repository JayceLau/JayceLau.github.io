title: Oracle RAC 12cR2安装手册(2)--环境的准备
categories:
  - Oracle
  - RAC
tags:
  - Tech
  - Installation
author: Jayce
date: 2017-06-29 10:23:01
---
<!-- toc -->
> 注：
> 除特殊注明外，本文所有命令均使用root用户执行。

### 配置操作系统
#### 安装依赖包
node01和node02上分别执行
```bash
yum -y install binutils compat-libcap1 compat-libstdc++ compat-libstdc++-33 e2fsprogs e2fsprogs-libs glibc glibc glibc-devel glibc-devel ksh libgcc libgcc libstdc++ libstdc++ libstdc++-devel libstdc++-devel libaio libaio libaio-devel libaio-devel libXtst libXtst libX11 libX11 libXau libXau libxcb libxcb libXi libXi make net-tools nfs-utils sysstat smartmontools
```
#### 配置内核参数
node01和node02上分别执行
编辑/etc/sysctl.conf文件
`vi /etc/sysctl.conf`
添加或修改
```
kernel.shmall = 2097152
kernel.shmmax = 4294967295
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
kernel.panic_on_oops=1
```
执行`sysctl -p`使其生效
#### 关闭NTP
安装Oracle RAC要求各节点间时间同步，本文选用Oracle Cluster Time Synchronization Service进行同步，因而关闭NTP服务。
```
/sbin/service ntpd stop
chkconfig ntpd off
mv /etc/ntp.conf /etc/ntp.conf.org
rm /var/run/ntpd.pid
```
#### 配置PAM
node01和node02上分别执行
编辑/etc/pam.d/login文件
`vi /etc/pam.d/login`
添加
`session required pam_limits.so`
编辑/etc/security/limits.conf文件
`vi /etc/security/limits.conf`
添加
```
@oinstall hard nofile 65536
@oinstall soft nofile 10240
@oinstall hard nproc 16384
@oinstall soft nproc 16384
@oinstall hard stack 32768
@oinstall soft stack 10240
@oinstall soft memlock 475188563
@oinstall hard memlock 475188563
```
#### 关闭SELinux
node01和node02上分别执行
```
setenforce 0
vi /etc/sysconfig/selinux
```
修改为
`SELINUX=disabled`
#### 创建用户、组、文件夹
node01和node02上分别执行
```
groupadd -g 1000 oinstall
groupadd -g 1020 asmadmin
groupadd -g 1021 asmdba
groupadd -g 1022 asmoper
groupadd -g 1031 dba
groupadd -g 1032 oper
useradd -u 1100 -g oinstall -G asmadmin,asmdba,asmoper,oper,dba grid
useradd -u 1101 -g oinstall -G dba,asmdba,oper oracle
mkdir -p /u01/software
mkdir -p /u01/app/12.2.0/grid
mkdir -p /u01/app/grid
mkdir -p /u01/app/oracle
chown -R grid:oinstall /u01
chown oracle:oinstall /u01/app/oracle
chmod -R 775 /u01/
```
将下面的两个文件分别上传至node01的/u01/software文件夹下
linuxx64_12201_grid_home.zip
linuxx64_12201_database.zip
解压并修改对应权限
```
cd /u01/software
unzip linuxx64_12201_grid_home.zip -d /u01/app/12.2.0/grid/
chown -R grid:oinstall /u01/app/12.2.0/grid/
unzip linuxx64_12201_database.zip
chown oracle:oinstall database
```
#### 配置环境变量
##### 配置grid用户的环境变量
node01上执行
编辑/home/grid/.bash_profile文件
`vi /home/grid/.bash_profile`
添加
```
export ORACLE_SID=+ASM1
export TMP=/tmp
export ORACLE_BASE=/u01/app/grid
export ORACLE_HOME=/u01/app/12.2.0/grid
export PATH=$ORACLE_HOME/bin:/usr/sbin:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
export LC_ALL=en_US.UTF-8
```
node02上执行
编辑/home/grid/.bash_profile文件
`vi /home/grid/.bash_profile`
添加
```
export ORACLE_SID=+ASM2
export TMP=/tmp
export ORACLE_BASE=/u01/app/grid
export ORACLE_HOME=/u01/app/12.2.0/grid
export PATH=$ORACLE_HOME/bin:/usr/sbin:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
export LC_ALL=en_US.UTF-8
```
##### 配置oracle的环境变量
node01上执行
编辑/home/oracle/.bash_profile文件
`vi /home/oracle/.bash_profile`
添加
```
export TMP=/tmp
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/12.2.0/dbhome_1
export ORACLE_SID=orcl1
export PATH=$ORACLE_HOME/bin:/usr/sbin:$ORACLE_HOME/OPatch:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
export NLS_LANG=AMERICAN_AMERICA.UTF8
export LC_ALL=en_US.UTF-8
```
node02上执行
编辑/home/oracle/.bash_profile文件
`vi /home/oracle/.bash_profile`
添加
```
export TMP=/tmp
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/12.2.0/dbhome_1
export PATH=$ORACLE_HOME/bin:/usr/sbin:$ORACLE_HOME/OPatch:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
export NLS_LANG=AMERICAN_AMERICA.UTF8
export LC_ALL=en_US.UTF-8
```
### 配置网络
#### 配置hosts解析
如[Oracle RAC安装手册(1)虚拟机的准备](../../23/Oracle-RAC-12cR2安装手册-1-虚拟机的准备/#配置第一台虚拟机)中所述，Oracle RAC的安装至少需要两块网卡。前文已完成网卡的添加和IP的配置，此处介绍IP的规划。

{% table table-striped %}
|    Name    |    node01     |    node02     |
|:----------:|:-------------:|:-------------:|
| Public IP  | 172.16.10.11  | 172.16.10.12  |
| Private IP | 192.168.10.11 | 192.168.10.12 |
| Virtual IP | 172.16.10.21  | 172.16.10.22  |
|  SCAN IP   | 172.16.10.31  | 172.16.10.32  |
|            | 172.16.10.33  |               |
{% endtable %}

> 注：
> 1. Public IP, Virtual IP, SCAN IP必须在同一个子网下；
> 2. SCAN IP推荐配置三个，最少一个。

node01和node02上分别执行
编辑/etc/hosts文件
`vi /etc/hosts`
添加
```
# Public
172.16.10.11 node01.myCluster.com node01
172.16.10.12 node02.myCluster.com node02
# Private
192.168.10.11   node01-priv.myCluster.com   node01-priv
192.168.10.12   node02-priv.myCluster.com   node02-priv
# Virtual
172.16.10.21   node01-vip.myCluster.com    node01-vip
172.16.10.22   node02-vip.myCluster.com    node02-vip
# SCAN
172.16.10.31   nodes-scan.myCluster.com nodes-scan
172.16.10.32   nodes-scan.myCluster.com nodes-scan
172.16.10.33   nodes-scan.myCluster.com nodes-scan
```
#### 关闭虚拟网卡
如果安装了libvirt，需要关闭虚拟网卡功能。
node01和node02分别执行
```
virsh net-list
virsh net-destroy default
virsh net-undefine default
service libvirtd restart
```
#### 禁用ZEROCONF
node01和node02分别执行
编辑/etc/sysconfig/network文件
`vi /etc/sysconfig/network`
添加
```
NOZEROCONF=yes
```
### 配置SSH互信
使用grid用户登录node01或切换到grid用户，分别为grid用户和oracle用户配置SSH互信。

> 注：
> 配置过程中，需要输入oracle和grid的密码，可提前为其设置密码。

```
cd /u01/app/12.2.0/grid/oui/prov/resources/scripts/
./sshUserSetup.sh -hosts "node01 node02" -user grid  -advanced -noPromptPassphrase
./sshUserSetup.sh -hosts "node01 node02" -user oracle  -advanced -noPromptPassphrase
```
使用grid用户登录node01验证
```
ssh node02 date
ssh node02-priv date
```
使用grid用户登录node02验证
```
ssh node01 date
ssh node01-priv date
```
使用oracle用户登录node01验证
```
ssh node02 date
ssh node02-priv date
```
使用oracle用户登录node02验证
```
ssh node01 date
ssh node01-priv date
```
### 配置存储
本文选用ASM作为存储方案，前文[Oracle RAC安装手册(1)虚拟机的准备](../../23/Oracle-RAC-12cR2安装手册-1-虚拟机的准备/#添加共享磁盘)中已添加共享磁盘，接下来介绍如何配置ASM。
> 注：
> 1. 本文使用的磁盘为/dev/sdb，受限磁盘空间，OCR_VOT_GIMR磁盘组和DATA磁盘组的冗余类型都选择为External。/dev/sdb共60G空间，创建1个40G分区分配给OCR_VOT_GIMR磁盘组使用，创建一个20G分区分配至DATA磁盘组。
> 2. 本文仅演示安装过程，关于ASM磁盘的大小[官方文档](https://docs.oracle.com/database/122/CWLIN/oracle-clusterware-storage-space-requirements.htm#CWLIN-GUID-97FD5D40-A65B-4575-AD12-06C491AF3F41)有具体的最小限制，实际生产中请注意。

#### 安装ASMLIB
参考[Oracle Linux下安装ASMLIB](../../../07/04/Oracle-Linux下安装ASMLIB/),分别在node01和node02安装ASMLIB,并配置、启动oracleasm服务。
#### 创建分区
在node01上执行
```
fdisk /dev/sdb
```
创建过程参考如下：
```
[root@node01 ~]# fdisk /dev/sdb
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0xc379a890.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.
Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)
WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
         switch off the mode (command 'c') and change display units to
         sectors (command 'u').
Command (m for help): n
Command action
   e   extended
   p   primary partition (1-4)
p
Partition number (1-4): 1
First cylinder (1-7832, default 1):
Using default value 1
Last cylinder, +cylinders or +size{K,M,G} (1-7832, default 7832): +40G
Command (m for help): n
Command action
   e   extended
   p   primary partition (1-4)
p
Partition number (1-4): 2
First cylinder (5224-7832, default 5224):
Using default value 5224
Last cylinder, +cylinders or +size{K,M,G} (5224-7832, default 7832):
Using default value 7832
Command (m for help): w
The partition table has been altered!
Calling ioctl() to re-read partition table.
Syncing disks.
```
> 注：
> 仅创建分区即可，无需格式化。

创建完成后，磁盘分区情况如下：
{% asset_img nodes-storage-configure-0.png nodes-storage-configure %}
#### 创建磁盘组
node01执行如下命令创建磁盘组
```
oracleasm createdisk OCR_VOT_GIMR /dev/sdb1
oracleasm createdisk DATA /dev/sdb2
```
创建过程参考如下

{% asset_img nodes-storage-configure-1.png nodes-storage-configure %}

使用`oracleasm listdisks`查看已创建的磁盘组

{% asset_img nodes-storage-configure-3.png nodes-storage-configure %}

创建成功👆🏻
node02执行如下命令扫描node01上已创建的磁盘组
```
oracleasm scandisks
```
扫描过程参考如下

{% asset_img nodes-storage-configure-2.png nodes-storage-configure %}

使用`oracleasm listdisks`查看扫描到的磁盘组

{% asset_img nodes-storage-configure-4.png nodes-storage-configure %}

扫描成功👆🏻
至此，完成了环境的准备工作。
下一篇[Oracle RAC 12cR2安装手册(3)--网格基础设施的安装](../../../07/05/Oracle-RAC-12cR2安装手册-3-网格基础设施的安装/)
