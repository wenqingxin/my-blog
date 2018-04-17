---
title: ssh
date: 2018-04-05 11:10:57
tags:
---
## 一、ssh是什么
SSH(安全外壳协议)为Secure Shell的缩写，是建立在应用层基础上的安全协议。SSH是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。
## 二、ssh的作用
利用SSH协议可以有效防止远程管理过程中的信息泄露问题。SSH客户端最初是UNIX系统上的一个程序，后来又迅速扩展到其他操作平台。几乎所有UNIX平台—包括HP-UX、Linux、AIX、Solaris、Digital UNIX、Irix，以及其他平台，都可运行SSH。
<!-- more -->
## 三、ssh的使用
### ssh服务端
以centOS为例
1. 安装
yum install openssh-server
2. 启动
service sshd start
3. 设置开机运行
chkconfig sshd on
4. 配置文件
/etc/ssh/sshd_config

### ssh客户端
1. 安装
yum install openssh-clients 
2. 连接服务端
ssh username@host
3. 免密登录
1）生成sshkey
执行
ssh-keygen -t rsa -C "xxxx@qq.com"
会在.ssh文件夹生成密钥对，分别为id_rsa（私钥）与id_rsa.pub（公钥）。
2）开启ssh-agent
eval `ssh-agent`
ssh-agent就是一个密钥管理器，运行ssh-agent以后，使用ssh-add将私钥交给ssh-agent保管，其他程序需要身份验证的时候可以将验证申请交给ssh-agent来完成整个认证过程
3）把私钥添加到ssh-agent缓存中
ssh-add ~/.ssh/id_dsa
4）服务器添加公钥
服务器的.ssh文件下需要新建一个authorized_keys文件，将本地生产的id_rsa.pub的内容复制到服务器athorized_keys里去。
4. 配置
～/.ssh/config 文件
``` yaml
# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
# aliyun
Host aliyun
    HostName 112.74.182.219
    User qxwen
    Port 22
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
```
以上配置可实现ssh简写服务名与免密登陆