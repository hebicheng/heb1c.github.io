---
layout: post
title: '为服务器配置ssh PublicKey登录'
date: 2021-4-27
author: hebicheng
header-style: text 
tags: [linux, ssh]
---

> 通过为服务器配置SSH 密钥登录，提高服务器安全性

## 1. 准备工作
* 首先利用密钥生成工具生成密钥对：  
在服务器上使用`ssh-keygen`命令生成（一路回车即可）
* 生成的密钥对默认生成在当前用户的`~/.ssh/`目录下，`id_rsa`是密钥，`id_rsa.pub`是公钥。在`~/.ssh/`目录下新建`authorized_keys`文件，并将公钥内容复制到其中。或者也可以用`cat id_ras.pub >> authorized_keys`直接将公钥写入该文件。
* 下载密钥`id_rsa`到本地，以备后续使用。
* 到此我们的准备工作就完成了

## 2. 配置ssh服务
> (以下是在ubuntu上的操作方式，其他系统上可能稍有不同，但流程是一致的)
* 编辑`/etc/ssh/sshd_config`文件。
* 找到`PubkeyAuthentication` 设置为`yes`，开启密钥登录。
![图片](/img/in-post/为服务器配置SSH密钥登录/20210427110634.png)
* （可选）如果想要更安全，可以关闭ssh密码登录，防止被爆破。只需修改`PasswordAuthentication`为`no`即可.
![图片](/img/in-post/为服务器配置SSH密钥登录/20210427111149.png)
* 重启ssh服务`service sshd restart`

## 3. 登录
* 命令行登录`ssh username@host -i 你的密钥路径`。
* ssh工具如xshell等，只需要配置登录方式时选择公钥登录，并指定密钥路径即可。

### 推荐阅读
* [利用autossh实现内网穿透](https://hebicheng.github.io/2021/04/15/%E5%88%A9%E7%94%A8autossh%E5%AE%9E%E7%8E%B0%E5%86%85%E7%BD%91%E7%A9%BF%E9%80%8F/)