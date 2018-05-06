---
layout: post
title: 'markdown遇到的坑'
date: 2018-05-06
author: heb1c
cover: ''
tags: markdown
---

> markdown之有些坑要踩过之后才知道..

在写一篇post的过程中，需要在内容里面插入一些html码，如下:

```HTML
<html>      
<head>      
<script>  
    window.onload=function(){
        document.write("content 2");
    }
</script> 
</head>  
<body>  
<div>content 1</div> 
</body>  
</html>
```

本地预览的时候一切正常，但是一放到网站上这些代码就被执行了！
百思不得其解，后来经一位dalao指点才知道是"**```HTML**"代码标签前面没有空行导致的。    
所以，以后写markdown的时候记住一句话:

> 生命诚可贵，能加空行就加空行！

还有一些其他人总结过的坑mark一下 [[一踩一个准之Markdown](https://www.jianshu.com/p/7c4637c72900)]