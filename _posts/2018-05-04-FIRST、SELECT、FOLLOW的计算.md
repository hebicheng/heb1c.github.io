---
layout: post
title: ''
date: 2018-05-03
author: heb1c
cover: ''
tags: 编译原理
---

> LL(1)文法判断之first、select、follow集合的计算

# FIRST集合
* 若X ∈ $V_N$，则FIRST(X)＝{X}。(简单讲，终结符的FIRST集就是它本身)

* 若X ∈ V，且有产生式X → a..., a ∈ $V_T$, 则 a ∈ FIRST(X)

* X → ε,则ε ∈ FIRST(X)。　

* 若X,${Y_1},{Y_2},...,{Y_n}$ ∈ ${V_N}$,且有产生式$X →{Y_1},{Y_2},...,{Y_n}$;当${Y_1},{Y_2},...,{Y_{n-1}}$都能推导出ε时,则FIRST(X) 为所有$Y_i$(i ∈ (1,n-1))的集合-{∈},记为T。若$Y_n$ → ε,则有FIRST(X) = T ∪ {∈}。

* 重复计算，直到FIRST集合不再改变。

# FOLLOW集合
* 设S为文法中开始符号，把{#}加入FOLLOW(S)中(这里“#”  为句子括号)。

* 若A→αBβ是一个产生式，则把FIRST(β)的非空元素加入
  FOLLOW(B)中。如果β能够推导出ε则把FOLLOW(A)也加入FOLLOW(B)中。

* 重复计算，直到FELLOW集合不再改变。

# SELECT集合
* 给定上下文无关文法的产生式A→α,若α不能推导出ε,则SELECT(A→α)=FIRST(α).

* 如果α能推导出ε,则：SELECT(A→α)=（FIRST(α) –{ε}）∪FOLLOW(A).