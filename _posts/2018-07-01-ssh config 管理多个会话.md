---
layout: post
title: 'ssh config 管理多个会话'
date: 2018-07-01
author: heb1c
cover: ''
tags: linux ssh
---

# ssh config 管理多个会话

我们在 `.ssh` 目录下新建一个 `config` 文件， 内容如下：

```python
Host server1
HostName server1.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/rsa
User server1

Host server2
HostName server2.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/rsa
User server2

```
参数说明：  
`Host`: 主机名，用于区分不同的主机  
`HostName`: 主机地址  
`PreferredAuthentications`: 连接方式  
`IdentityFile`: 私钥文件位置  
`User`: 要登录的用户名

