---
layout: post
title: 'first、select、follow集合的计算'
date: 2018-05-04
author: heb1c
cover: ''
tags: 编译原理
---

> LL(1)文法判断之first、select、follow集合的计算  
> 原创作品，转载请注明来源 [heb1cheng.hithub.io](https://heb1cheng.hithub.io)  

# FIRST集合
* 若$X ∈ V_N$，则$FIRST(X)＝{X}$。(简单讲，终结符的FIRST集就是它本身)

* 若$X ∈ V$，且有产生式$X → a..., a ∈ $$V_T$, 则$ a ∈ FIRST(X)$

* $X → ε$,则$ε ∈ FIRST(X)$。　

* 若$X,{Y_1},{Y_2},...,{Y_n} ∈ {V_N}$,且有产生式$X →{Y_1},{Y_2},...,{Y_n}$;当${Y_1},{Y_2},...,{Y_{n-1}}$都能推导出$ε$时,则$FIRST(X)$ 为所有$Y_i(i ∈ (1,n-1))$的集合$-{∈}$,记为$T$。若$Y_n → ε$,则有$FIRST(X) = T ∪ {∈}$。

* 重复计算，直到$FIRST$集合不再改变。

# FOLLOW集合
* 设$S$为文法中开始符号，把${#}$加入$FOLLOW(S)$中(这里“$#$”  为句子括号)。

* 若$A→αBβ$是一个产生式，则把$FIRST(β)$的非空元素加入
  $FOLLOW(B)$中。如果$β$能够推导出$ε$则把$FOLLOW(A)$也加入$FOLLOW(B)$中。

* 重复计算，直到$FELLOW$集合不再改变。

# SELECT集合
* 给定上下文无关文法的产生式$A→α$,若$α$不能推导出$ε$,则$SELECT(A→α)=FIRST(α)$.

* 如果$α$能推导出$ε$,则：$SELECT(A→α)=(FIRST(α) –{ε})∪ FOLLOW(A)$.