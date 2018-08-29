---
title: Arcanist环境的准备(for macOS)
date: 2018-08-28 17:03:27
tags: [CodeReview,macOS]
categories: Phabricator
---

#### 需要的工具和组件

- git
- PHP
- Arcanist

#### 安装步骤

##### git的安装

1. 安装

首先安装[brew](https://brew.sh/)，如果已安装brew，这步跳过。

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

然后使用brew来安装git

```bash
brew install git
```

2. 检查确认

安装完成后，执行`git --version`,观察到如下输出即证明安装成功：

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180829094340.png)

##### PHP的安装

macOS默认安装有PHP，执行`php -v`检查确认。

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180829094637.png)

如果没有安装，同样可以使用brew进行安装`brew install php`。

##### Arcanist的安装

1. 安装

```bash
sudo mkdir /usr/local/arc #创建安装目录，此步会提示输入登录密码
sudo chown jayce:admin /usr/local/arc/ #修改目录所有者为自己
cd /usr/local/arc/ #切换至安装目录下
git clone https://github.com/phacility/libphutil.git #克隆工具库
git clone https://github.com/phacility/arcanist.git #克隆Arcanist

```

克隆需要花费一定时间，观察到如下输出即完成克隆。

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180829102239.png)

2. 配置环境变量

编辑`~/.bash_profile`文件，添加如下两行：

```bash
export ARC_HOME=/usr/local/arc/arcanist
export PATH="$PATH:$ARC_HOME/bin"
```

3. 检查确认

执行`source ~/.bash_profile`使环境变量生效，接着执行`arc version`观察到如下输出即证明Arcanist安装成功。

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180829151852.png)

#### 配置文本编辑器

在提交代码审查的过程中，需要输入或编辑大块的文本。默认的编辑器为vim，不是特别友好。Arcanist支持配置图形化的文本编辑器，如Sublime Text。

配置编辑器为Sublime Text

```
arc set-config editor "open -a \"/Applications/Sublime Text.app\" -W"
```

