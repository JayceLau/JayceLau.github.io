title: Oracle RAC 12cR2安装手册(3)--网格基础设施的安装

categories: Oracle

tags: [RAC, Installation]

author: Jayce

date: 2017-07-05 11:08:00

---
> 注：
> 除特殊注明外，本文所有命令均使用root用户执行。

### 安装CVU
node01上执行
```
cd /u01/app/12.2.0/grid/cv/rpm/
rpm -ivh cvuqdisk-1.0.10-1.rpm
```
在node01上检查
使用grid用户登录node01或切换至grid用户
```
cd /u01/app/12.2.0/grid/
./runcluvfy.sh stage -pre crsinst -n node01,node02 -fixup -verbose
```
{% asset_img grid-infrastructure-cvu-0.png grid-infrastructure-cvu %}
按照提示,在node02上执行修复脚本
```
/tmp/CVU_12.2.0.1.0_grid/runfixup.sh
```
执行完后，回车👇🏻
{% asset_img grid-infrastructure-cvu-1.png grid-infrastructure-cvu %}
修复成功👆🏻
### 安装网格基础设施
在node01上执行
使用grid用户登录node01或切换至grid用户
```
cd /u01/app/12.2.0/grid/
./gridSetup.sh
```
安装过程如下
> 注：
> 1、本安装实验使用笔记本进行，部分条件无法满足，不影响使用，直接忽略即可；
> 2、安装前需保证node02上ORACLE_BASE和ORACLE_HOME目录为空，如果报错，可以手动清理掉。

{% asset_img grid-infrastructure-installation-0.png grid-infrastructure-installation %}
为新集群配置网格基础设施→下一步👆🏻
{% asset_img grid-infrastructure-installation-1.png grid-infrastructure-installation %}
配置一个独立的集群→下一步👆🏻
{% asset_img grid-infrastructure-installation-2.png grid-infrastructure-installation %}
输入集群名称、SCAN名称（需要与hosts配置的一致）和SCAN端口（默认1521）→下一步👆🏻
{% asset_img grid-infrastructure-installation-3.png grid-infrastructure-installation %}
添加新节点👆🏻
{% asset_img grid-infrastructure-installation-4.png grid-infrastructure-installation %}
输入node02的信息→OK👆🏻
{% asset_img grid-infrastructure-installation-5.png grid-infrastructure-installation %}
添加完成→下一步👆🏻
{% asset_img grid-infrastructure-installation-6.png grid-infrastructure-installation %}
指定网卡用途→下一步👆🏻
{% asset_img grid-infrastructure-installation-7.png grid-infrastructure-installation %}
使用块设备配置ASM→下一步👆🏻
{% asset_img grid-infrastructure-installation-8.png grid-infrastructure-installation %}
不为GIMR配置单独的ASM磁盘组→下一步👆🏻
{% asset_img grid-infrastructure-installation-9.png grid-infrastructure-installation %}
命名磁盘组为OCR_VOT_GIMR，选择外部冗余，修改探索目录→OK👆🏻
{% asset_img grid-infrastructure-installation-10.png grid-infrastructure-installation %}
选择OCR_VOT_GIMR→下一步👆🏻
{% asset_img grid-infrastructure-installation-11.png grid-infrastructure-installation %}
使用统一密码→下一步👆🏻
{% asset_img grid-infrastructure-installation-12.png grid-infrastructure-installation %}
不使用IPMI→下一步👆🏻
{% asset_img grid-infrastructure-installation-13.png grid-infrastructure-installation %}
不注册EMCloud→下一步👆🏻
{% asset_img grid-infrastructure-installation-14.png grid-infrastructure-installation %}
默认→下一步👆🏻
{% asset_img grid-infrastructure-installation-15.png grid-infrastructure-installation %}
默认→下一步👆🏻
{% asset_img grid-infrastructure-installation-16.png grid-infrastructure-installation %}
输入root用户密码，自动运行配置脚本→下一步👆🏻
{% asset_img grid-infrastructure-installation-17.png grid-infrastructure-installation %}
执行先决条件检查👆🏻
{% asset_img grid-infrastructure-installation-18.png grid-infrastructure-installation %}
忽略全部→下一步👆🏻
{% asset_img grid-infrastructure-installation-19.png grid-infrastructure-installation %}
确认继续👆🏻
{% asset_img grid-infrastructure-installation-20.png grid-infrastructure-installation %}
安装→下一步👆🏻
{% asset_img grid-infrastructure-installation-21.png grid-infrastructure-installation %}
安装进行中👆🏻
{% asset_img grid-infrastructure-installation-22.png grid-infrastructure-installation %}
确认使用root执行脚本👆🏻
{% asset_img grid-infrastructure-installation-23.png grid-infrastructure-installation %}
集群校验失败，确认即可👆🏻
{% asset_img grid-infrastructure-installation-24.png grid-infrastructure-installation %}
下一步👆🏻
{% asset_img grid-infrastructure-installation-25.png grid-infrastructure-installation %}
确认继续👆🏻
{% asset_img grid-infrastructure-installation-26.png grid-infrastructure-installation %}
关闭👆🏻
使用grid用户分别在node01和node02上执行`crs_stat -v -t`查看服务运行状态👇🏻
{% asset_img grid-infrastructure-check-0.png grid-infrastructure-check %}
node01👆🏻
{% asset_img grid-infrastructure-check-1.png grid-infrastructure-check %}
node02👆🏻
> 注：
    部分服务处于OFFLINE状态，可以忽略。

### 创建其他磁盘组
使用grid用户登录node01或切换至grid用户执行`asmca`
{% asset_img asm-disk-group-creation-0.png asm-disk-group-creation %}
欢迎界面👆🏻
{% asset_img asm-disk-group-creation-1.png asm-disk-group-creation %}
选择磁盘组→创建👆🏻
{% asset_img asm-disk-group-creation-2.png asm-disk-group-creation %}
创建DATA磁盘，选择外部冗余，选择DATA磁盘→OK👆🏻
{% asset_img asm-disk-group-creation-3.png asm-disk-group-creation %}
创建中👆🏻
{% asset_img asm-disk-group-creation-4.png asm-disk-group-creation %}
创建完成→退出👆🏻
{% asset_img asm-disk-group-creation-5.png asm-disk-group-creation %}
确认退出👆🏻
至此，完成了网格基础设施的安装。
下一篇[Oracle RAC 12cR2安装手册(4)--数据库软件的安装](../../../07/07/Oracle-RAC-12cR2安装手册-4-数据库软件的安装/)
