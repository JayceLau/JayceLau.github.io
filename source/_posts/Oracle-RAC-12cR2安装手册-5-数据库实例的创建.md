title: Oracle RAC 12cR2安装手册(5)--数据库实例的创建

date: 2017-07-11 23:49:34

author: Jayce

categories: Oracle

tags: [RAC, Installation]

---

在node01上执行
```
dbca
```
{% asset_img database-instance-creation-0.png database-instance-creation %}
创建一个数据库→下一步👆🏻
{% asset_img database-instance-creation-1.png database-instance-creation %}
高级配置→下一步👆🏻
{% asset_img database-instance-creation-2.png database-instance-creation %}
选择自定义数据库→下一步👆🏻
{% asset_img database-instance-creation-3.png database-instance-creation %}
选择所有节点→下一步👆🏻
{% asset_img database-instance-creation-4.png database-instance-creation %}
设置全局数据库名和SID前缀，选择创建空白容器数据库→下一步👆🏻
> 注：
>   此处的SID前缀需要与环境变量里的ORACLE_SID对应：
    例如，SID前缀设置orcl，ORACLE_SID需要设置为orcl1和orcl2。

{% asset_img database-instance-creation-5.png database-instance-creation %}
选择ASM存储类型并选择DATA磁盘组→下一步👆🏻
{% asset_img database-instance-creation-6.png database-instance-creation %}
启用快速闪回去，并选择DATA磁盘组，大小设置为5120M→下一步👆🏻
{% asset_img database-instance-creation-6-1.png database-instance-creation %}
确定继续👆🏻
{% asset_img database-instance-creation-7.png database-instance-creation %}
默认→下一步👆🏻
{% asset_img database-instance-creation-8.png database-instance-creation %}
选择自动共享内存管理，并设置Memory Target约为物理内存的80%👆🏻
{% asset_img database-instance-creation-9.png database-instance-creation %}
块大小设置为32768B，进程数设置为1500👆🏻
{% asset_img database-instance-creation-10.png database-instance-creation %}
字符集选择AL32UTF8👆🏻
{% asset_img database-instance-creation-11.png database-instance-creation %}
连接模式选择专用服务器模式→下一步👆🏻
{% asset_img database-instance-creation-12.png database-instance-creation %}
确认继续→下一步👆🏻
{% asset_img database-instance-creation-13.png database-instance-creation %}
不配置EM→下一步👆🏻
{% asset_img database-instance-creation-14.png database-instance-creation %}
使用统一密码→下一步👆🏻
{% asset_img database-instance-creation-15.png database-instance-creation %}
创建数据库→下一步👆🏻
{% asset_img database-instance-creation-16.png database-instance-creation %}
再次确认继续👆🏻
{% asset_img database-instance-creation-17.png database-instance-creation %}
执行先决条件检查👆🏻
{% asset_img database-instance-creation-18.png database-instance-creation %}
忽略全部→下一步👆🏻
{% asset_img database-instance-creation-19.png database-instance-creation %}
确认继续👆🏻
{% asset_img database-instance-creation-20.png database-instance-creation %}
完成👆🏻
{% asset_img database-instance-creation-21.png database-instance-creation %}
创建进行中👆🏻
{% asset_img database-instance-creation-22.png database-instance-creation %}
创建完成→关闭👆🏻
分别在node01和node02上执行`lsnrctl status`查看实例运行状态👇🏻
{% asset_img database-instance-creation-23.png database-instance-creation %}
node01状态正常👆🏻
{% asset_img database-instance-creation-24.png database-instance-creation %}
node02状态正常👆🏻
执行`sqlplus / as sysdba`登录数据库👇🏻
{% asset_img database-instance-creation-25.png database-instance-creation %}
登录成功👆🏻
执行`show pdbs`查看PDB状态👇🏻
{% asset_img database-instance-creation-26.png database-instance-creation %}
可以看到一个种子PDB👆🏻
至此，完成了数据库实例的创建。
