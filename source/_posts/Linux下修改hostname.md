title: Linux下修改hostname
tags:
  - Tech
  - Configuration
categories:
  - Linux
  - System
author: Jayce
date: 2017-06-28 10:49:00
---
{% asset_img linux-hostname-configuration-0.png linux-hostname-configuration %}
查看当前hostname👆🏻
使用 `hostname yourHostName` 命令，临时修改hostname（重启失效）👇🏻
{% asset_img linux-hostname-configuration-1.png linux-hostname-configuration %}
修改成功👆🏻
编辑 `/etc/sysconfig/network` 文件，永久修改hostname（重启有效）👇🏻
{% asset_img linux-hostname-configuration-2.png linux-hostname-configuration %}
修改HOSTNAME的值并保存👆🏻
重启检查👇🏻
{% asset_img linux-hostname-configuration-3.png linux-hostname-configuration %}
修改成功👆🏻
