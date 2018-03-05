---
layout: post
title: "笔记：TensorFlow框架"
description: ""
category: tech
tags: []
modify: 2018-03-05 08:59:26
---

update: 2018-03-05


去年秋，因工作原因接触TensorFlow。在使用过程中，逐渐尝试向社区做贡献，已经快有半年了。走过些弯路，也遇到困惑，好在慢慢都趟过来。受[图解tensorflow 源码](https://github.com/yao62995/tensorflow)启发，打算系统地梳理下自己的理解认识，主要面向源码开发者。自己还是个新手，很多方面不足，望读者斧正。

计划边读源码的过程中边更新文章，战线会比较拖。但会以半年定个限，来回顾已有进展，再确定下一步。


### 目录

+ 反向梯度
+ 图构建
+ Eigen
+ GPU


### 后记
