---
title: Phabricator Code Review操作手册
date: 2018-08-27 13:48:19
tags: CodeReview
categories: Phabricator
---

#### 更新日志

2018-08-30:

- 更新代码审查申请备注信息的说明及对应截图；
- 新增arc diff信息提交后的截图；

#### 概述

Phabricator支持两条不同的代码审查工作流，一条叫做“reivew”（审查），是在推送前进行的；另一条叫做“audit“（审计），是在推送后进行的。本文主要介绍review。

Phabricator的代码审查功能目前没有图形化的客户端，只能通过命令行进行操作，使用的工具为Arcanist。

#### 环境的准备

*注：本步骤为**被审查者**需要进行的操作，其他人可以跳过。*

- Windows用户请参考[Arcanist环境的准备(for Windows)][arcanist-for-windows]
- macOS用户请参考[Arcanist环境的准备(for macOS)][Arcanist-for-macos]

#### 环境的配置

##### 配置工程

*注：本步骤为**仓库管理员**要进行的操作，其他人只需要拉取最新代码即可。*

在工程根目录下创建`.arcconfig`文件，并写入以下内容（注意修改phabricator.uri的值为对应地址）：

```json
{
  "phabricator.uri" : "https://phabricator.example.com/",
  "history.immutable" : true
}
```

然后将其提交并推送至远程仓库。

##### 安装Arcanist证书

*注：本步骤为**被审查者**需要进行的操作，其他人可以跳过。*

使用命令行工具执行

```bash
cd PROJECT_PATH/ #切换到你的项目目录下，注意替换PROJECT_PATH
arc install-certificate #安装Arcanist证书
```

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180829231644.png)

使用浏览器访问提示的地址获取API Token（需要登录）

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180829110002.png)

复制API Token，粘贴至`Paste API Token from that page:`处

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180829111648.png)

证书安装完成。

#### 提交代码审查申请

*注：本步骤为代码审查的**关键步骤**之一，请大家仔细阅读。*

代码审查发生在提交合并之后，推送之前。之前的git工作流保持不变，只在完成代码提交、分支合并后，使用Arcanist提交代码审查申请。此时**被审查者**无法再使用`git push`推送至远程仓库。强行推送时，会看到如下的报错：

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180903112624.png)

完成代码提交、分支合并后，使用命令行工具，切换到工程目录下执行：

```bash
cd PROJECT_PATH/ #注意替换成真实的工程目录
arc diff
```

此时会自动打开之前定义的文本编辑器

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180830215226.png)

*注：*

*1.首行为代码审查申请的标题，请简明扼要的写明本次申请所涉及的内容;*

*2.Summary项可以详细介绍本次申请所涉及的内容，使用"**Tn**"会自动引用对应的任务；*

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180830220147.png)

*3.Reviewers为必填项，务必准确无误地填入**审查者**的Phabricator的用户名；支持多人审查，用户名用“,”隔开；此处的admin仅做演示，实际使用时，请修改为自己的审查人。*

填写完成后，保存，并关闭文本编辑器（Arcanist会等待文本编辑器程序的退出，因而此处必须**完全退出**程序）。

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180830220241.png)

此时已完成代码审查的提交的，Phabricator会自动发送邮件通知**审查者**进行代码审查，也可以手动复制Revision URI发送给审查者。

*注意：此时需记下代码审查的ID，此处为“D32”，后面会用到。*

#### 代码审查

*注：本步骤为**审查者**需要进行的操作，其他人可以跳过。*

**审查者**收到代码审查申请后，直接访问对应的链接打开代码审查界面。

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180830220430.png)

在这里可以看到修订的文件列表、文件改动的具体内容等，审查者完成代码审查后，可以选择“Accept Revision”（接受该修订版）、“Request Changes”（请求变更）、“Change Reviewers”（改变审查者）等。此外，还可以添加备注信息，用于记录信息或者提醒被审查者做代码修改。

这里选择接收该修订版，被审查者将收到审查通过的通知邮件。

#### 推送代码

*注：本步骤为**被审查者**需要进行的操作，其他人可以跳过。*

1. 审查通过

**被审查者**收到审查通过的通知邮件后，使用命令行工具执行：

```bash
cd PROJECT_PATH/ #切换到工程目录下，注意替换成真实的工程目录
arc land --revision D32 #提交代码审查申请时自动分配的ID
```

![](https://raw.githubusercontent.com/JayceLau/PicBed/master/pictures/20180830222840.png)

代码推送成功。

2. 审查未通过

如果收到的是请求变更的邮件，请按照审查者的说明重新修改代码，并再次执行`arc diff`，同初次[提交代码审查申请](#提交代码审查申请)时相同，此时会自动打开之前定义的文本编辑器（与之前的文本内容略有不同），填写完成后，保存，退出编辑器程序，以更新代码审查申请。

该过程可能需要重复多次，直到审查者接受了变更，然后执行`arc land --revision DXX`进行代码推送。

[arcanist-for-windows]:../Arcanist环境的准备(for-Windows)/
[Arcanist-for-macos]:../../28/Arcanist环境的准备(for-macOS)/