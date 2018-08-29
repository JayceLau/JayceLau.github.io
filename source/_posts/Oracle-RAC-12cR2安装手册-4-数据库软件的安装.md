title: Oracle RAC 12cR2安装手册(4)--数据库软件的安装

date: 2017-07-07 10:17:05

author: Jayce

categories: Oracle

tags: [RAC, Installation]

---
在node01上执行
使用oracle用户登录node01
```
cd /u01/software/database/
./runInstaller
```
安装过程如下
{% asset_img database-software-installation-0.png database-software-installation %}
取消勾选安全更新→下一步👆🏻
{% asset_img database-software-installation-1.png database-software-installation %}
确认不接收安全问题的通知👆🏻
{% asset_img database-software-installation-2.png database-software-installation %}
只安装数据库软件→下一步👆🏻
{% asset_img database-software-installation-3.png database-software-installation %}
RAC数据库安装→下一步👆🏻
{% asset_img database-software-installation-4.png database-software-installation %}
选择所有节点→下一步👆🏻
{% asset_img database-software-installation-5.png database-software-installation %}
选择企业版→下一步👆🏻
{% asset_img database-software-installation-6.png database-software-installation %}
确定安装目录→下一步👆🏻
{% asset_img database-software-installation-7.png database-software-installation %}
确认操作系统用户组→下一步👆🏻
{% asset_img database-software-installation-8.png database-software-installation %}
执行先决条件检查👆🏻
{% asset_img database-software-installation-9.png database-software-installation %}
同样忽略全部👆🏻
{% asset_img database-software-installation-10.png database-software-installation %}
确认继续👆🏻
{% asset_img database-software-installation-11.png database-software-installation %}
安装👆🏻
{% asset_img database-software-installation-12.png database-software-installation %}
安装进行中👆🏻
{% asset_img database-software-installation-13.png database-software-installation %}
依次在node01和node02上执行`/u01/app/oracle/product/12.2.0/dbhome_1/root.sh`👆🏻
{% asset_img database-software-installation-14.png database-software-installation %}
{% asset_img database-software-installation-15.png database-software-installation %}
执行完毕👆🏻
{% asset_img database-software-installation-16.png database-software-installation %}
OK👆🏻
{% asset_img database-software-installation-17.png database-software-installation %}
关闭👆🏻
至此，完成了数据库软件的安装。
下一篇[Oracle RAC 12cR2安装手册(5)--数据库实例的创建](../../../07/11/Oracle-RAC-12cR2安装手册-5-数据库实例的创建/)
