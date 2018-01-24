---
layout: post
title: "How to remove trailing spaces(行尾空格) for Scala in IntelliJ IDEA 2017"
description: ""
category: misc
tags: []
modify: 2017-06-08 10:06:31
---

update: 2017-06-08


最近IntelliJ升级到2017后，scala文件的行尾空格，不再自动消除，真是逼死洁癖程序员。

开始我的解决方案是在git提交时做防御，如下：

```bash
git commit -a -m "xxx"
git rebase --whitespace=fix HEAD~1
```

但每回都得敲俩命令，真是不胜其烦。

已经打算弄个bash化名前，幸好我查了下IntelliJ配置，结果找到了做怪的选项。
解决方案如下：

```
+ Preferences
  - Editor
    - General
      - Other
        Strip trailing spaces on Save: [Modified Lines]
        [ ] Always keep trailing spaces on caret line

+ Preferences
  - Editor
    - Code Style
      - Scala
        - Tabs and Indents:
          [ ] Keep indents on empty lines
```

注意，此功能只会在保存文件时触发。

所以，每次提交前，保存全部文件（Save All: Command key + S），即会自动清除。

总之，又可以愉快地搬砖了，以上。
