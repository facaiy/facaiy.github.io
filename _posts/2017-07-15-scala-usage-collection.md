---
layout: post
title: "收集：scala语法用例和疑难点"
description: ""
category: misc
tags: [scala]
modify: 2017-07-15 17:26:11
---

update: 2017-07-15


### A. 语法

#### method vs function

```scala
// method
scala> def m(x: Int, y: Int): Int = x + y
res1: m: (x: Int, y: Int)Int

// function
scala> val f = (x: Int, y: Int) => x + y
res2: f: (Int, Int) => Int = <function2>

// method to function
scala> m _
res3: (Int, Int) => Int = <function2>

// curried
scala> f.curried
res4: Int => (Int => Int) = <function1>

scala> (m _).curried
res5: Int => (Int => Int) = <function1>
```

#### 传参时，每个`_`对应一个参数：

```scala
// (1) wrong
List(1, 2, 3).map(x => x + 1)

List(1, 2, 3).map(_ => _ + 1) // won't compile
// = List(1, 2, 3).map(x => {y => y + 1})

// (2) right
List(1, 2, 3, 4).reduce((x, y) => x + y)
// is equivalent to
List(1, 2, 3, 4).reduce(_ + _)
```

参考：[In Scala, what is the difference between using the `_` and using a named identifier?](https://stackoverflow.com/questions/2353222/in-scala-what-is-the-difference-between-using-the-and-using-a-named-identif)


#### Implicit

+ [Where does Scala look for implicits?](http://docs.scala-lang.org/tutorials/FAQ/finding-implicits.html)



### B. 用例

#### 遍历Option[Array[A]]类型

```scala
val oas: Option[Array[Int]] = Some(Array(1, 2, 3))

val res =
 for {
   as <- oas.toArray // or use .toSeq
   a <- as
 } yield a + 1
```

这里之所以要使用.toArray变换，是为了使外层的Option.flatMap转成Array.flatMap，从而和内层的Array.map类型兼容。

参考：[For comprehension over Option array](https://stackoverflow.com/questions/35304550/for-comprehension-over-option-array)


#### 函数用元组做为参数列表

```scala
scala> def f(i : Int, j : Int) = i + j
scala> val args = (0, 1)

scala> val tf = (f _).tupled
tf: ((Int, Int)) => Int = <function1>

scala> tf(args)
res7: Int = 1
```

参考：[How to apply a function to a tuple?](https://stackoverflow.com/questions/1987820/how-to-apply-a-function-to-a-tuple)
