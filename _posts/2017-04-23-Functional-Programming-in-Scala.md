---
layout: post
title: "笔记：Functional Programming in Scala"
description: ""
category: tech
tags: []
modify: 2017-06-23 09:55:59
---

update: 2017-06-23

早上四点就自然醒了，没了睡意，就整理下最近在看的函数式编程书籍[Functional Programming in Scala](https://www.manning.com/books/functional-programming-in-scala), 2015。同事推荐说有点烧脑，做了开头两章，确实挺好玩的。

官方在Github上有个仓库，放了习题、指导和答案，很方便，见[fpinscala/fpinscala](https://github.com/fpinscala/fpinscala)。遗憾的是我没有看到习题配套的单元测试用例来校验正确性，所以自己建了个mvn工程，作业都放在[facaiy/book_notes](https://github.com/facaiy/book_notes/tree/master/Manning_Functional_Programming_in_Scala)。希望尽量都做一遍，耗时预计在两个月。


### 目录

#### PART 1 INTRODUCTION TO FUNCTIONAL PROGRAMMING

##### 1 What is functional programming?

##### [2 Getting started with functional programming in Scala](https://github.com/facaiy/book_notes/blob/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c2/ChapterTwo.scala)

##### [3 Functional data structures](https://github.com/facaiy/book_notes/tree/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c3)

##### [4 Handling errors without exceptions](https://github.com/facaiy/book_notes/tree/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c4)

##### [5 Strictness and laziness](https://github.com/facaiy/book_notes/blob/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c5/Stream.scala)

##### [6 Purely functional state](https://github.com/facaiy/book_notes/blob/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c6/State.scala)


#### PART 2 FUNCTIONAL DESIGN AND COMBINATOR LIBRARIES

##### [7 Purely functional parallelism](https://github.com/facaiy/book_notes/tree/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c7)

##### [8 Property-based testing](https://github.com/facaiy/book_notes/tree/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c8)

##### [9 Parser combinators](https://github.com/facaiy/book_notes/tree/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c9)


#### PART 3 COMMON STRUCTURES IN FUNCTIONAL DESIGN

##### [10 Monoids](https://github.com/facaiy/book_notes/blob/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c10/Monoid.scala)

##### [11 Monads](https://github.com/facaiy/book_notes/tree/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c11)

##### [12 Applicative and traversable functors](https://github.com/facaiy/book_notes/tree/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c12)


#### PART 4 EFFECTS AND I/O

##### [13 External effects and I/O](https://github.com/facaiy/book_notes/tree/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c13)

##### 14 Local effects and mutable state

##### [15 Stream processing and incremental I/O](https://github.com/facaiy/book_notes/blob/master/Manning_Functional_Programming_in_Scala/src/main/scala/io/github/facaiy/fp/scala/c15/Process.scala)


### 后记

学习时，对于第一至三部分，下了很多工夫，基本完成了全部习题。而最后的第四部份，相对更复杂，也不在兴趣点上，只是泛泛一览而过。

主要收获有两点：一是熟悉了scala的诸多高级特性，如Generic, Implicit, Lambda type等等。二是了解了函数式编程的思想和方法。对函数式编程的印象中，有两点最深刻：一是它很灵活，扩展性和可组装性强大，擅长于抽出模式，很适合用于编写库；二是它很像是在写编译器，常常会用个数据结构描述计算，再用具体的辅助函数来解释执行，将描述与执行分离，可以在后级做优化，很有趣。

在编写工作代码时，我用书中的知识技巧完成了两个库。学以致用，好生快乐！详见：

+ [facaiy/DAG-lite](https://github.com/facaiy/DAG-lite): 轻量调度库。借鉴了书中懒惰求值和并行的设计。
+ [facaiy/math-expression-parser](https://github.com/facaiy/math-expression-parser)：数学表达式解析器。借鉴了书中Parser的处理。

两个月的时间，还是挺漫长的。总算结束了，大功告成，又如释重负。
