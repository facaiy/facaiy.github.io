---
layout: post
title: "如何从印象笔记中国版导出enex格式"
description: ""
category: misc
tags: [evernote]
modify: 2023-07-05 07:27:11
---

update: 2023-07-05


印象笔记广告太多，即使付费也避免不了，实在烦心。最近pdf批注功能用不了，成为压死骆驼的最后一根稻草，打算迁回到国际版evernote。操作时发现印象笔记已经不支持enex格式，只能选私有格式notes格式，非常头痛。

好在有开发者做了备份工具[evernote-backup](https://github.com/vzhd1701/evernote-backup)，它的原理相当于一个第三方笔记客户端，用你的账号和密码登陆读取印象笔记服务器数据，再自行导出为enex格式，测试了下非常完美。

### 安装

这个工具依赖python，需要用户有一定的动手能力，主要步骤如下：

1. 确认本机安装python，如果没有，建议安装[Anaconda](https://www.anaconda.com/download)发行版本。新手可以参考详细教程 [Anaconda安装（过程详细）](https://blog.csdn.net/weixin_42855758/article/details/122795125) 和 [Anaconda的详细安装教程](https://blog.csdn.net/qq_42535748/article/details/125913108)。
2. 找到命令行工具（新人见[Windows](https://blog.csdn.net/yao_yaoya/article/details/127862527)教程和[Mac](https://support.apple.com/zh-cn/guide/terminal/apd5265185d-f365-44cb-8b09-71a064a42125/mac#:~:text=%E6%88%91%E6%89%93%E5%BC%80%E2%80%9C%E7%BB%88%E7%AB%AF%E2%80%9D-,%E6%89%93%E5%BC%80%E2%80%9C%E7%BB%88%E7%AB%AF%E2%80%9D,%E7%84%B6%E5%90%8E%E8%BF%9E%E6%8C%89%E2%80%9C%E7%BB%88%E7%AB%AF%E2%80%9D%E3%80%82)教程），输入`pip install --user evernote-backup`安装。如果是Mac用户，也可以用包管理工具[Homebrew](https://brew.sh/)来安装（教程见[Mac系统HomeBrew安装过程](https://blog.csdn.net/ZCC361571217/article/details/127333754)），再输入命令`brew install evernote-backup`即可。


### 使用

**注意：虽然是开源工具，考虑到密码安全，最好在使用前/后改下密码。**

```bash
# 1. 登陆
evernote-backup init-db --backend china --force
# 依次输入账号和密码

# 2. 拉取笔记
evernote-backup sync

# 3. 导出为enex
evernote-backup export output_dir/
```

这样操作，本地会生成一个`output_dir`文件夹，每个笔记本都会导成单个enex文件放在里面，接下来在evernote国际版「导入」即可。
