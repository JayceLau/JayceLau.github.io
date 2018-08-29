title: 在OMS服务器上提取Management Agent·
date: 2017-08-04 14:06:42
tags: [Oracle, EMCC]
categories: Oracle
author: Jayce
---
### 登录emcli
`emcli login -username=sysman`
过程参考如下：
```
[oracle@emcc agent]$ emcli login -username=sysman
Enter password :
Login successful
```
### 提取agent
```
emcli get_agentimage -destination=/home/oracle/agent -platform="Linux x86-64" -version=13.2.0.0.0
```
过程参考如下：
```
[oracle@emcc agent]$ emcli get_agentimage -destination=/home/oracle/agent -platform="Linux x86-64" -version=13.2.0.0.0
 === Partition Detail ===
Space free : 31 GB
Space required : 1 GB
Check the logs at /u01/app/emcc/gc_inst/em/EMGC_OMS1/sysman/emcli/setup/.emcli/get_agentimage_2017-08-04_13-55-30-PM.log
Downloading /home/oracle/agent/13.2.0.0.0_AgentCore_226.zip
File saved as /home/oracle/agent/13.2.0.0.0_AgentCore_226.zip
Downloading /home/oracle/agent/13.2.0.0.0_Plugins_226.zip·
File saved as /home/oracle/agent/13.2.0.0.0_Plugins_226.zip
Downloading /home/oracle/agent/unzip
File saved as /home/oracle/agent/unzip
Executing command: /home/oracle/agent/unzip /home/oracle/agent/13.2.0.0.0_Plugins_226.zip -d /home/oracle/agent
Exit status is:0
Agent Image Download completed successfully.
```
### 安装agent
将提取出来的agent上传至目标服务器，并解压、安装
过程参考如下：·
```
cd /u01/software
unzip 13.2.0.0.0_AgentCore_226.zip
./agentDeploy.sh AGENT_BASE_DIR=/u01/app/emcc/agent OMS_HOST=foo.bar.com EM_UPLOAD_PORT=4903 LOG_DIR=/home/oracle/logs
```
