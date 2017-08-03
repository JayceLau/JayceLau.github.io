---
title: Oracle Enterprise Manager Cloud Control 13cR2安装手册
date: 2017-07-20 10:18:38
tags:
  - Tech
  - Installation
categories:
  - Oracle
  - EM Cloud
---
### 配置操作系统
#### 安装依赖包
```bash
yum -y install make binutils gcc gcc-c++ libaio glibc-common libstdc++ libXtst sysstat glibc-devel glibc
```
#### 配置PAM
编辑/etc/pam.d/login文件
`vi /etc/pam.d/login`
添加
`session required pam_limits.so`
编辑/etc/security/limits.conf文件
`vi /etc/security/limits.conf`
添加
```
@oinstall hard nofile 65536
@oinstall soft nofile 30000
```
#### 创建用户、组、文件夹

```
groupadd -g 1000 oinstall
useradd -u 1001 -g oinstall oracle
mkdir -p /u01/app/em13c/{middleware,agent}
chown oracle:oinstall /u01
```
### 安装EMCC
oracle登录服务器执行
```
cd /u01/software
tar zxvf em13200p1_linux64.tgz
cd em13200p1_linux64/
./em13200p1_linux64.bin
```
