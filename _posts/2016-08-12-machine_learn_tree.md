---
layout: post
title: "机器学习：树相关算法"
description: ""
category: tech
tags: []
modify: 2017-03-15 09:23:17
---
update: 2017-03-15

+ 2017-03-11，新浪微博算法平台，大部门分享：[GBDT、TreeBoost 和 XGBoost：树模型的进化之路 - 颜发才](/assets/ml_tree/GBDT_TreeBoost_XGBoost_facaiy.pdf)


### 缘起
看了不少机器学习的书籍，却始终是略懂皮毛的程度。最近项目训练GBDT模型，更是深切地认识到「学以致用」的必要性。故打算由浅入深地重新学习树相关的方法，大致思路是：首先实现 demo 阐明原理，再阅读常用的工程实现，最后阅读论文打通数学推导。

本文用于整理记录学习笔记。因为是初学者，肯定纰漏错识之处甚多，敬请审阅。


### 0 决策树

#### 0.0 Demo
+ [决策树简介和 Python 实现](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/tree/decision_tree/demo.ipynb)

#### 0.1 sklearn 实现
总纲：

+ [决策树在 sklearn 中的实现简介](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/tree/decision_tree/sklearn/intro.ipynb)

细节：

+ [评价函数模块 _criterion.* 详解](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/tree/decision_tree/sklearn/_criterion.ipynb)

+ [分割函数模块 _splitter.* 详解](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/tree/decision_tree/sklearn/_splitter.ipynb)

+ [树构建模块 _tree.* 详解](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/tree/decision_tree/sklearn/_tree.ipynb)


#### 0.2 spark 实现


### 1 随机森林

+ [随机森林简介](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/tree/random_forest/intro.ipynb)


### 2 GBDT(Gradient Boosting Decision Tree)

#### 原理

+ [GBDT(Gradient Boosting Decision Tree) 原理简介](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/tree/gbdt/intro.ipynb)

+ [TreeBoost简介](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/tree/gbdt/treeboost/intro.ipynb)

#### 实现
+ [GBDT 在 spark 中的实现简介](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/tree/gbdt/spark/intro.ipynb)

+ [Spark ML: Tree架构笔记](https://www.evernote.com/l/ADDTp7Nc6iRFmJRUDola9Jpa9v1J1BAsNeg)


### 3 xgboost

+ [xgboost的基本原理与实现简介](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/tree/gbdt/xgboost/intro.ipynb)


### 结语

耗时近三个月，比预计的时间长。树相关的概念比较简单，但确实有些实现细节较为繁琐。回顾前面的文章，还是有问题的，有些是笔误，有些是当时的理解片面。后续打算整理一份详细的文档，汇总上述文章并修正错点。

总之，树的专题可以暂时告一段落了，收获挺多。
