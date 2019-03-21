---
layout: post
title: '利用nginx实现多端口的https访问'
date: 2019-3-21
author: heb1c
cover: ''
tags: linux nginx
---

> 利用nginx反向代理，通过配置多域名，实现在一台机器上利用不同的域名访问不同的服务，并且实现https。

先上代码

```sh

events {
        worker_connections 1024;
}

http {
    upstream server1 {
        server 127.0.0.1:8000 weight=1;
    }

    upstream server2 {
        server 127.0.0.1:8080 weight=1;
    }

    server {
        listen 443;
        server_name heb1c.cn;
        location = / {
            proxy_pass   http://server1;
        }
   
        ssl on;
        ssl_certificate /xxx/full_chain.pem;
        ssl_certificate_key /xxx/private.key;     
    }

    server{
        listen 443;
        server_name www.sicnuacm.cn;
        location /{
            proxy_pass   http://server2;
        }
        ssl on;
        ssl_certificate /xxx/full_chain.pem;
        ssl_certificate_key /xxx/private.key;
   }

   server{
        listen 80;
        server_name heb1c.cn www.heb1c.cn;
        rewrite ^/(.*)$ https://heb1c.cn:443/$1 permanent;
    }

    server{
        listen 80;
        server_name sicnuacm.cn www.sicnuacm.cn;
        rewrite ^/(.*)$ https://sicnuacm.cn:443/$1 permanent;
    }

}

```

我们从代码开始解释：  
首先我们用 `upstream` 将我们放在不同端口上（8080和8000）的两个服务表示成 `server1` 和 `server2`。  
接着我们定义了两个server都监听`443`端口，在第一个server，我们设置它的域名为`server_name`为`heb1c.cn` ，`proxy_pass`为`http://server1`，意思是当访问的域名为`heb1c.cn`且端口为`443`时，nginx会自动代理到我们配置的`server1`,访问相应的服务。第二个`server`的配置同理.  
只需要这样简单的配置，我们就能实现在一台机器上利用不同的域名访问不同的服务。  
另外，注意到后面`80`端口的监听，这是为了将http请求转发到https，以实现服务的全面https化。  

> 利用nginx实现多端口的https访问    
> 原创作品，转载请注明来源 [https://hebicheng.github.io](https://hebicheng.github.io) 