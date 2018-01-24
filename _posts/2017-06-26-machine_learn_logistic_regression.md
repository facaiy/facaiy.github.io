---
layout: post
title: "机器学习：逻辑回归"
description: ""
category: tech
tags: []
modify: 2017-07-11 10:52:40
---

update: 2017-07-11

逻辑回归在点击率推荐中仍占有半壁江山，于是打算细致做下梳理，包括常见理论变形和工业成熟实现。预计耗时一个月。


### 0. 理论

#### 0.1 原理

+ [逻辑回归算法简介和Python实现](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/logistic_regression/demo.ipynb)

#### 0.2 寻优算法

常规的机器学习算法，通常有两个过程：一是构建模型，并给出损失函数；二是用数值寻优根据损失函数找到合适参数。

数值寻优本身门类很多，如下面博文所述：

+ [常见的几种最优化方法](http://www.cnblogs.com/maybe2030/p/4751804.html)
+ [学习总结：局部搜索](http://blog.sciencenet.cn/blog-628137-497041.html)
+ [数值优化（Numerical Optimization）学习系列-目录](http://blog.csdn.net/fangqingan_java/article/details/48951191)

这个领域数理要求很高。不过我个人认为，把它们作为工具看待，了解其特性和区别即可。于是，简要梳理下常见的寻优算子：

+ 最速下降法：用一阶导数做步长搜索。简单，支持导步，但收敛较慢，震荡。
  - 批量梯度下降法(Batch Gradient Descent, BSD)：全量数据更新。
  - 随机梯度下降法(Stochastic Gradient Descent, SGD)：数据随机割成多份，逐份更新。

+ 牛顿法(Newton-Raphson method)：[简单介绍](http://blog.csdn.net/luoleicn/article/details/6527049)，其实质是用二维曲面拟合坐标点，再用曲面的切线进行搜索。优点是利用二阶导信息，收敛更快，缺点需借助Hessian矩阵$O(n^2)$，无法处理大量数据。

+ 拟牛顿法(Quasi-Newton Methods)：用一阶导信息来近似构造Hessian矩阵，简化运算复杂度。
  - DFP
  - BFGS:
    - LBFGS：内存更省。
      - WQL-QN: 支持L1正则。

+ 共轭梯度法(Conjugate gradient method)：搜索方向由当前梯度和以前搜索方向合成，介于最速下降法与牛顿法之间的一个方法。

未来有时间，打算读下经典教材，都是厚部头：

+ Convex Optimization, Stephen Boyd, Lieven Vandenberghe.
+ Numerical Optimization, Jorge Nocedal, Stephen J. Wright.


### 1. 工程实现

+ [逻辑回归在spark中的实现简介](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/logistic_regression/spark_ml_lr.ipynb)
+ [逻辑回归在scikit-learn中的实现简介](http://nbviewer.jupyter.org/github/facaiy/book_notes/tree/master/machine_learning/logistic_regression/sklearn_lr.ipynb)
+ [逻辑回归在TensorFlow contrib.learn中的实现简介](http://nbviewer.jupyter.org/github/facaiy/book_notes/blob/master/machine_learning/logistic_regression/tensorflow_lr.ipynb)
+ Angel
+ R pacakge: glm / glmet
+ Liblinear


### 2. 结语

逻辑回归的理论既简单又复杂，简单是说二分类的代数式很容易入门看懂，复杂是说对它的拓展可以很深入，如用矩阵式来求解、多分类、复杂的数值优化方法等等。易于入门，难于精通。

虽然线性分类器后面的数理知识坚实又深奥，然而工程实现相对简单，主要是损失函数和导数、及寻优算子二部份。大多数工程实现都大同小异，如sklearn是标签-1/+1的推导，spark在最后代数变换，将0/1标签转成-1/+1，tensorflow是标签0/1的推导。总体而言，代码并不太复杂。

花费了两周时间来梳理基础知识，以后有时间再深入。
