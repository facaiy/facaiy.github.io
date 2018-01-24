---
layout: post
title: "Spark ML工程笔记汇总"
description: ""
category: misc
tags: []
modify: 2017-05-30 13:53:51
---

update: 2017-05-30


最近开始尝试进行Spark贡献，目前精力主要在ML部份。在反复读代码的过程中，做了些笔记资料，打算分享出来。一则自己备忘，二来也方便他人快速上手。

因为主要是自己看，代码的迭代也快，只会简要做些点注，并不会面面俱到。如果想要深而细，建议阅读[Mastering Apache Spark 2](https://www.gitbook.com/book/jaceklaskowski/mastering-apache-spark/details)。


+ 04-30，[Spark ML: Param架构笔记](https://www.evernote.com/l/ADDXNX_7MMhI6Kf8Kh6hnCi5Xj0UgkTouJ8)，比较简单轻量，主要用到了trait的特性。
+ 05-01，[Spark ML: Tree架构笔记](https://www.evernote.com/l/ADDTp7Nc6iRFmJRUDola9Jpa9v1J1BAsNeg)，为了性能，又是单例，RandomForest写得比较复杂，大量参数飞来飞去。
+ 05-25，[Spark: Pyspark Param架构笔记](https://www.evernote.com/l/ADA6P5HQHv1FTYRYe3HpUkfw4_ambRaBeTw)，追了下Pyspark参数接口和Scala原生接口的对接关系。
+ 05-30，[Spark: Pyspark Tree架构笔记](https://www.evernote.com/shard/s48/nl/1328285495/8016ca0c-2dc4-4f61-8fc6-562cbb72d656/)，详列了决策树、随机森林和GBDT在Pyspark中的继承关系。
