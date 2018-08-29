title: Oracle Linux下安装ASMLIB

tags: [Linux, yum,asm]

categories: Oracle

author: Jayce

date: 2017-07-04 22:22:00

---
### 关于ASMLIB
Oracle ASMLIB（Oracle Automatic Storage Management library）是一个ASM的支持库，它使Oracle数据库可以更加有效地读写磁盘。
### 安装ASMLIB
#### 启用public_ol6_u3_base仓库
编辑/etc/yum.repos.d/public-yum-ol6.repo文件
```
vi /etc/yum.repos.d/public-yum-ol6.repo
```
找到
```
[public_ol6_u3_base]
name=Oracle Linux $releasever Update 3 installation media copy ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL6/3/base/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
```
设置为启用
```
enabled=1
```
#### 安装依赖包
```
yum -y install oracleasm oracleasm-support
```
> 注：
> 如果使用CentOS，需要安装与操作系统对应的版本。

### 配置oracleasm服务
执行如下命令进行配置
```
oracleasm configure -i
```
配置过程参考如下
```
This will configure the on-boot properties of the Oracle ASM library
driver.  The following questions will determine whether the driver is
loaded on boot and what permissions it will have.  The current values
will be shown in brackets ('[]').  Hitting <ENTER> without typing an
answer will keep that current value.  Ctrl-C will abort.
Default user to own the driver interface []: grid
Default group to own the driver interface []: asmadmin
Start Oracle ASM library driver on boot (y/n) [n]: y
Scan for Oracle ASM disks on boot (y/n) [y]: y
Writing Oracle ASM library driver configuration: done
```
### 启动oracleasm服务
执行如下命令启动oracleasm服务
```
/etc/init.d/oracleasm enable
```
