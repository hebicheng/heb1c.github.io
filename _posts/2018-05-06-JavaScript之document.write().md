---
layout: post
title: 'javaScript之document.write()'
date: 2018-05-06
author: heb1c
cover: ''
tags: js
---

# 定义和用法
document.write() 方法可向文档写入 HTML 表达式或 JavaScript 代码。  
可列出多个参数(exp1,exp2,exp3,...) ，它们将按顺序被追加到文档中。
> ## 说明
>  虽然根据 DOM 标准，该方法只接受单个字符串作为参数。不过根据经验，write() 可接受任何多个参数。
>我们通常按照两种的方式使用 write() 方法：一是在使用该方在文档中输出 HTML，另一种是在调用该方法的的窗口之外的窗口、框架中产生新文档。在第二种情况中，请务必使用 close() 方法来关闭文档。

## 例：
```HTML
<html>
<body>
<script type="text/javascript">
    document.write("Hello World!");
</script>
</body>
</html>
```
# document.write()清空原内容
> 很多人遇到过这样类似的情况，document.write方法向网页写内容的时候，会把文档中的原来的内容清空。那么什么时候会被清空什么时候又不会被清空呢？我们来看下面几个例子。

* 例一
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
显示：  

    content 2

在这个例子中，页面会显示"**content 2**",这是因为window.onload事件是在文档内容完全加载完毕触发，当然文档流已经关闭，这个时候执行doucment.write方法会自动调用document.open方法创建一个新的文档流，并写入新的内容，再通过浏览器展现，这样就会覆盖原来的内容。

我们再看下一个例子。
* 例二
```HTML
<html>      
<head>      
<script>  
    document.write("content 2");
</script> 
</head>  
<body>  
<div>content 1</div> 
</body>  
</html>
```
显示：  

    content 2
    content 1

这个例子中，原来的文档内容"**content 1**"并没有被清空，因为当前文档流是由浏览器所创建，并且document.wirte方法身处其中，也就是执行此方法的时候文档流并没有被关闭，这个时候不会调用document.open方法创建新文档流，所以也就不会被覆盖。

* 例三
```HTML
<html>      
<head>      
<script>  
    document.close(); 
    document.write("content 2");
</script> 
</head>  
<body>  
<div>content 1</div> 
</body>  
</html>
```
显示：  

    content 2
    content 1
这个例子中，发现原来的文档内容"**content 1**"还是没有被清空，我们使用document.close关闭了文档流，为什么还是不能覆盖原来的内容呢，因为文档流是由浏览器创建，无权限手动关闭，document.close方法只能够关闭由document.open方法创建的文档流。

* 例四
```HTML
<html>      
<head>      
<script>  
    function func(){
        document.write("content 1");
        document.close();
        document.write("content 2");
    }
</script> 
</head>  
<body>  
<button onclick="func()">Try it</button>
</body>  
</html>
```
显示：  

    content 2
这个例子中，便能达到我们预期的效果，调用document.close()后，原文档内容被清空，显示新的内容。
## 参考资料
[HTML DOM write() 方法](http://www.w3school.com.cn/jsref/met_doc_write.asp)  
[http://www.softwhy.com/article-8326-1.html](http://www.softwhy.com/article-8326-1.html)