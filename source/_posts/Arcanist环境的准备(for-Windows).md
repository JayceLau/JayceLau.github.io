---
title: Arcanist环境的准备(for Windows)
date: 2018-08-27 18:42:01
tags: [CodeReview,Windows]
categories: Phabricator
---

#### 需要的工具或组件

- git（包含Git Bash）
- PHP
- Arcanist

#### 安装步骤

##### git的安装

1. 安装

至[git官网](https://git-scm.com/downloads)选择最新稳定版下载，安装过程全部默认即可。

2. 检查确认

安装完成后，任意目录点击右键，观察到新增了「Git GUI Here」和「Git Bash Here」两个菜单项。

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180827165404.png)

点击「Git Bash Here」并输入`git --version`输出内容如下，即证明安装成功。

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180827165834.png)

##### PHP的安装

1. 安装

至[PHP官网](https://windows.php.net/download)选择最新稳定版下载（本文选择的版本为PHP-7.2.9-VC15-x64-Thread-Safe ），解压到C:\Program Files\目录或其他目录下，添加以下环境变量：

{% table table-striped %}

|  变量名  |                  变量值                   |
| :------: | :---------------------------------------: |
| PHP_HOME | C:\Program Files\php-7.2.9-Win32-VC15-x64 |

{% endtable %}

并将%PHP_HOME%/bin添加到Path变量中，参考如下：

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180828154244.png)

2. 配置

切换到`C:\Program Files\php-7.2.9-Win32-VC15-x64`目录下，将`php.ini-development`复制一份到同目录下，并命名为`php.ini`。

编辑php.ini，找到`; extension_dir = "ext"`，在该行下面添加如下两行：

```php
extension_dir = "C:\Program Files\php-7.2.9-Win32-VC15-x64\ext"
extension=php_curl.dll
```

*注：`C:\Program Files\php-7.2.9-Win32-VC15-x64`为PHP的安装目录。*

3. 检查确认

重启Git Bash使环境变量生效，执行`php -v`，观察到如下输出即证明PHP安装成功。

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180828161309.png)



##### Arcanist的安装

Arcanist没有图形化的安装工具，需要手动创建安装目录，并使用git将源码克隆至本地。

以下操作使用之前安装的Git Bash执行。

1. 安装

```bash
mkdir -p "C:\Program Files\arc" #创建安装目录
cd "C:\Program Files\arc" #切换至安装目录下
git clone https://github.com/phacility/libphutil.git #克隆工具库
git clone https://github.com/phacility/arcanist.git #克隆Arcanist

```

*注：*

*1.上述命令可以直接复制到Git Bash中执行。*

*2.安装目录可以根据需要，自行选择。*

克隆需要花费一定时间，观察到如下输出即完成克隆。

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180828141916.png)

2. 配置环境变量

添加以下环境变量

{% table table-striped %}

|  变量名  |            变量值             |
| :------: | :---------------------------: |
| ARC_HOME | C:\Program Files\arc\arcanist |

{% endtable %}

并将%ARC_HOME%/bin添加到Path变量中，参考如下：

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180828163146.png)

3. 检查确认

重启Git Bash使环境变量生效，执行`arc version`观察到如下输出即证明Arcanist安装成功。

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180829151626.png)

如果输出以下内容，则检查上一步中PHP是否配置正确。

```bash
PHP CONFIGURATION ERRORS

Your install of PHP does not have the 'php_curl.dll' extension enabled. Edit your php.ini file and uncomment the line which reads 'extension=php_curl.dll'.
```

#### 配置编辑器

在提交代码审查的过程中，需要输入或编辑大块的文本。默认的编辑器为vim，不是特别友好。Arcanist支持配置图形化的文本编辑器，如Notepad++、Sublime Text等，可以根据喜好，自行选择。

将编辑器配置为Notepad++

```bash
arc set-config editor "\"C:\Program Files (x86)\Notepad++\notepad++.exe\" -multiInst -nosession"
```

将编辑器配置为Sublime Text

```bash
arc set-config editor "\"C:\Program Files\Sublime Text 3\sublime_text.exe\" -w -n"
```

*请注意，不同的编辑器参数略有不同。*



	

	